+++
date = '2026-02-18T08:43:14+02:00'
title = 'Automating LLVM Refactoring'
+++

## Intro

I like automation. I mean, I really like automation.

For me, it is the fundamental purpose of computing. It's one of the main reasons behind my choice of OS and software, and why I love open source so much.

It's why I use Linux. It's why I like to write code in Neovim. It's why, other than the browser, almost all the apps I use live in the terminal.

There's nothing more satisfying than successfully automating something and seeing how much time it saved me. I feel really cool when I do that too (:

In this small article, I'd like to talk about some of the situations I've automated and some of the tools that facilitate this urge to automate everything.

I also plan on writing another related article that covers more of the CLI and TUI tools I use that helps me automate.
___

## LLVM

There's a refactoring going on in LLVM right now ([RFC](https://discourse.llvm.org/t/rfc-make-clang-builtin-math-functions-constexpr-with-llvm-libc-to-support-c-23-constexpr-math-functions/86450)). I won't go into all the details, but it basically involves moving files around, modifying function signatures, and editing dependencies in CMake and Bazel.

Here's an example of how the refactoring is done with `fabsf`:

- Extract the implementation from `libc/src/math/generic/fabsf.cpp` into `libc/src/__support/math/fabsf.h`
- Rewrite the original `.cpp` to just call the new header-only version
- Create a shared wrapper at `libc/shared/math/fabsf.h`
- Add the include to `libc/shared/math.h`
- Update `libc/src/__support/math/CMakeLists.txt` with the new header library target
- Update `libc/src/math/generic/CMakeLists.txt` to depend on the new support target
- Update the Bazel `BUILD.bazel` file with the support library and math function deps
- Add a test case to `libc/test/shared/shared_math_test.cpp` (this is the only step requiring knowledge about the actual function being refactored)

There are more than 400 functions that need to be refactored.

Initially, each function was picked up by someone doing their first one. Mistakes were natural. PRs were reviewed one to three times, and after each merge, all other PRs had to be rebased.

You can imagine how much time this took.

Progress so far:

Weeks 1-2:
7 functions merged (25 pending that still needed reviewing and rebasing)

Weeks 3-4:
27 functions merged (24 pending)

Even though it got faster, it still felt slow. At that pace, refactoring could easily take 2-3 months.

After two of my manual PRs got merged, I was already bored. I didn't want to keep doing this manually.

But I realized most of these operations could be automated which is a much more interesting problem.

### Treesitter and Regex

I wrote [the tool](https://github.com/AnonMiraj/refactinator) in Rust since I wanted more experience with it (Python might have been easier, though).

The main tool behind the refactoring was Tree-sitter.

I first learned about Tree-sitter when I started using Neovim. Back then, I thought it was just a fancy way to do syntax highlighting. It's much more than that.

With Tree-sitter, I can parse C++ files and directly manipulate the AST.

Now I can:

- Inject specific namespaces around certain functions
- Extract function contents, signatures, and arguments
- Modify them programmatically

And with Regex, I can:

- Find the correct place to inject information into the shared test file
- Edit Bazel and CMake files automatically
- Modify dependency lists

With the first version of the tool, it already handled about 90% of the refactoring. I only needed minimal manual work and to fix nitpicks caused by small bugs.

After refactoring five functions with it, I caught most of the nitpicks. Now the entire process is automated.

The only exception is adding proper test cases in `shared_test`, which requires some basic knowledge about the function's inputs and outputs. Currently, the tool inserts placeholder zeros for inputs and expected return values.

Hypothetically, this could be automated with an LLM, but I didn't feel the need. Learning more about math functions is interesting, so I'm fine doing that part manually.

### Bash Script

Since I got bored manually creating a new PR and issue for each function family, I wrote a small Bash script to automate that too.

The script:

* Takes the names of the functions to refactor
* Runs the refactorinator on each one
* Lets me inspect the changes
* Runs the tests
* Publishes the issue and PR if everything passes

And just like that, most of the remaining functions were refactored by me and a friend in less than a week.

