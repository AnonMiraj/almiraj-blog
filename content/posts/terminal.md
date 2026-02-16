+++
date = '2026-02-18T08:43:14+02:00'
title = 'The Terminal'
draft = true
+++

## CLI and TUI

I think TUIs (Terminal User Interfaces) are the perfect bridge between the GUI and the terminal. You get the best of both worlds.

They're easy to use like a GUI while still being very powerful. They're minimal, but you can extend them as much as you like.

That's why, even though I consider myself proficient in Git, I still really like using lazygit. It's convenient — all the information you need is right in front of you, and you can perform all actions with the press of a button.

You don't need to keep rewriting commands, and you avoid the slowness and complexity of GUI Git clients.

### Neovim

The first time I learned to code was with VS Code.

And man, it was confusing. I didn't understand much and felt like I was fighting the editor instead of it helping me.

It made me feel stupid, and I didn't like that feeling.

It didn't help that it was resource-hungry, and I had a very weak PC at the time.

When I discovered Vim, I was fascinated. It looked like magic.

Once I started using it, I could wrap my head around most of it. I could add any feature I wanted whenever I wanted. It was really empowering. It really improved my confidence as a developer.

\+ It gave me a lot of bragging rights on the internet XD

There are two automation-related parts I especially like:

- Macros

Macros in Vim are really powerful. They let you record a sequence of actions and replay them whenever you want, automating repetitive edits and Vim motions make them even more powerful.

If I were crazy enough, I could probably do the entire LLVM refactoring just with macros.

- Lua

Lua is one of the things I like most about Neovim.

With Lua, you can modify or add anything in Vim, and it's surprisingly simple.

For example, while adding new math functions in LLVM, I needed to quickly convert between decimal representation and float16 hex.

Instead of opening Python every time, I built a small Lua helper in about half an hour that made the task super convenient.

## TUI File Browser

Instead of using a graphical file manager, I prefer a TUI file browser like Yazi.

The reasons are similar to why I like lazygit.

But honestly, file management in the terminal was a life changer.

Everything is more convenient — moving between folders with integrated zoxide, fuzzy searching with fzf, sending files directly to my phone with the press of a key.

And much more.

There are a lot of cool plugins for Yazi that can do almost anything, making it even more powerful.

With image preview possible in the terminal thanks to the Kitty protocol, there's honestly no reason to use a GUI file manager. In fact, I feel crippled when I'm inside one.

---

# End

And that's it.
I have a lot more things to talk about, but this is enough for now.

I guess I just really like automation :)
