# Serenity's Skyrim

So, this is a guide on how to do skyrim good. For the future in which i inevitably wipe my install again and have to consult this guide again. Guide is broken into segments. `// clean this up in the future`

The end-goals of the modding guide are to create a skyrim install that is idempotent and reproducible. Ideally, you should be able to just do some base setup in MO2, copy the entire modlist over with all the necessary files, and you'll have a working skyrim mod install.

This might form the basis of a declarative modding DSL like Nix in the future, if I really bother with it. Yes, I know that Wabbajack exists.

As for the modlist itself, the end-result of modding is a Skyrim that should feel brand new. Everything from visuals to gameplay and questing should be overhauled.

This guide and modlist is inspired by the following resources.

- Step Modifications (Skyrim Special Edition Guide and System Setup Guide)
- NOLVUS Skyrim AE Modpack and Guide
- [biggie-boss/NGVO](https://github.com/biggie-boss/NGVO) (Next Generation Visual Overhaul)

## Table of Contents

0. Pre-requisites
1. Clean install and intial setup
2. Pre-modding
3. Mods and modding guide
4. Caveats, annexes
5. Conclusion
6. Feedback

## 0. Pre-requisites

0. Reading comprehension
1. 7zip or some other archive program (WinRar is also fine)
2. VSCode (or Codium), or Notepad++
3. Mod Organizer 2 (No Vortex, Next, whatever. Use MO2.)
4. Root Builder for MO2 (plugin).

The above are hard requirements to get started with this mod guide. Pay attention to the installation instructions for the Root Builder plugin for MO2, it is used to ensure that the installation of Skyrim that we are given is completely pristine if launched from Steam.