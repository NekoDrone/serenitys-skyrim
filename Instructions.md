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

## 1. Clean install and initial setup

For a truly clean install, you'll want to install Skyrim outside of the following directories:

- `C:\Program Files\`
- `C:\Program Files (x86)\`
- `C:\Users\`
- `C:\Windows\`

The reason you'd want this is because some of the tooling, or the way mods themselves work, might conflict with these paths, which are restricted paths in Windows. If you're doing this on Linux, similarly you wouldn't install it to directories like `/opt/` or `/tmp/` or `/dev/`.

If you need a secondary Steam Library location on the same drive as your usual `C:\Program Files (x86)\Steam\` installation location, please see the following.

### Option A: Edit `libraryfolders.vdf`

0. Close Steam.
1. Create the directory. E.g. `C:\my_new_steam_library\steamapps\`
2. Locate your existing `libraryfolders.vdf`. It should be at `C:\Program Files (x86)\Steam\steamapps\libraryfolders.vdf`.
3. Edit the file. Copy the last entry in the file (e.g. `1 "C:\Program Files (x86)\Steam"`) and paste it as a new line, changing the number and path (e.g. `2 "C:\my_new_steam_library"`).
4. Restart Steam.

### Option B: Using the Console in Steam.

0. Close Steam.
1. Create the directory. E.g. `C:\my_new_steam_library\steamapps\`
2. Press `WIN + R` to open the Run dialog.
3. Enter the command `steam://open/console` and hit `Enter`.
4. Steam should launch you into a hidden Console tab with a CLI.
5. Run the command `library_folder_add [DIRECTORY]`. E.g. `library_folder_add C:\my_new_steam_library\`. You should get an ouput: `Mounted path "C:\my_new_steam_library" : index [something].`.
6. Restart Steam.

---

### Clean Installation

**Once you have a Steam Library outside of the restricted directories, you can proceed with installation.**

To get a pristine install, uninstall Skyrim from Steam and nuke any existing Skyrim Special Edition directory in any of your `steamapps` libraries.

Then, reinstall Skyrim from Steam and launch the game **once**.

After launching the game for the first time, with AE, you'll download 74 Creations from the Creation Club. We will be letting MO2 manage this in the next section.

Once you have finished downloading the AE Creations, your main menu in Skyrim should be the Empire symbol in gold, not silver like without AE.

Once you have reached this state, you may exit Skyrim. From here on out, you may start Skyrim to do testing at any time except for sections where the guide tells you explicitly not to restart between changes.

It is also wise to do a full backup of the `Data` directory in Skyrim. Specifically, the `/Data` directory located at `.../steamapps/common/Skyrim Special Edition/Data/`. This is called the `Data` directory and will be referenced as such.

The `Data` directory should contain all `.esm`, `.esp`, `.esl`, and `.bsa` files relevant to Skyrim, including:

- `Skyrim.esm`, which is the main Skyrim Master file;
- `Dragonborn.esm`, `Hearthfire.esm`, and `Dawnguard.esm`, which are DLC Masters for Dragonborn, Hearthfire, and Dawnguard respectively (these are older DLCs that have since been bundled with SE/AE.);
- `cc<...>.esl` and `cc<...>.esm` files, which is the Creation Club content bundled with AE. You should have a combined total of 74 `.esm` and `.esl` files. You should also have exactly one `.bsa` file for each `.esl` or `.esm` in the Data directory with the same `cc<...>` code. E.g. `ccbgssse037-curios.esl` has an associated `ccbgssse037-curios.bsa`;
- Miscellaneous `.bsa` files for Skyrim itself;
- `_ResourcePack.bsa` and `_ResourcePack.esl` files, the `ShaderCache` directory, the `Video` directory, `Update.esm`, and a `MarketplaceTextures.bsa` file.

## 2. Pre-modding

This section of the guide will bring you through installing the AE Content Picker mod which lets you opt in and out of specific AE content, and more importantly, letting MO2 manage the AE Creation Club content, as well as installing SKSE through MO2/Root Builder in order to get you familiarised with the process of using Root Builder.

It is called pre-modding because we haven't actually done anything to affect changes in the game yet, just doing things to make managing our install easier.

### a. Anniversary Edition Content Picker

This 'mod' doesn't actually come with any content, as AE content is paid for and must be purchased through the Creation Club or Steam. Instead, what it is is a pre-written FOMOD installer for AE content, meaning that you can pick and choose what AE content actually goes into your install of Skyrim.

More importantly, it also lets you just manage AE content as if it were a regular mod in MO2 (or any other mod manager for that matter), which is its main purpose for inclusion in this guide.

#### Installing the Anniversary Edition Content Picker mod.

Following the steps on the mod page's Installation section would be sufficient. This is a rehash of those same instructions. The GamerPoets video on the same page is also a good resource.

The purpose of this installation process is to actually provide the CC content into the downloaded mod archive. Without it, the 'mod' contains no content.

1. Download the mod from [the nexus page](https://www.nexusmods.com/skyrimspecialedition/mods/58890) WITHOUT a mod manager. Do it on the browser through the 'Manual download` button on the Nexus.
2. Create an empty directory. E.g. `AE Picker`.
3. Extract the contents of the downloaded `.7z` file into the empty directory. It should look something like `.../AE Picker/images/...`, `.../AE Picker/fomod/...`, and `.../AE Picker/CC Content/`.
4. In a second file explorer window, go to your Skyrim `Data` directory (see above).
5. (If you have already made a backup of your entire `Data` directory, you can skip this step) Copy all files beginning with `cc<...>` into a backup directory somewhere else on your PC. E.g. `ccbgssse037-curios.esl`, `ccbgssse037-curios.bsa`, `cctwbsse001-puzzledungeon.esm`.
6. Move (or cut) all files beginning with `cc<...>` into the `CC Content` directory in the directory created in step 2. E.g. `.../AE Picker/CC Content/ccbgssse037-curios.esl`. The end result of this step should be a Skyrim `Data` directory that has no files with `cc<...>`. This is intentional and is why we backed up our CC content. From this point on, do NOT start Skyrim up again until step 10.
7. Re-compress the directory into an archive, such that the root of the archive contains the `/images`, `/fomod`, and `/CC Content` directories.
8. Drag and drop the resulting new archive into MO2.
9. Select your options or install everything. This guide assumes you have installed everything. In the future, you may 'reinstall' this mod to select what you want with granularity.
10. Done! You may start Skyrim again from here on out. It would be wise to launch it once right now through MO2 to check that the game still recognises the AE content, and that MO2 is able to properly manage the content as well.

After this point, all AE content can be treated as just a regular mod in MO2. Meaning that you can 'reinstall' it using MO2 to access the FOMOD installer again, or simply enable or disable the mod as you wish.

The only downside is that all AE content is treated as a single mod, meaning that you can't simply enable the mods one at a time, and instead, must enable the entire collection of AE content in one shot.

You *can* manually extract each CC mod into its own archive and install it through MO2 similar to the above steps, but it's tedious and you'll need to do this for each one of the 74 CC mods included in AE.

Instead, it is recommended that you keep all of the AE mods as one, and if you have conflicts or issues, if you can identify which AE mod is the culprit, reinstall the content picker mod on MO2 and disable that specific `.esl`.

### b. Skyrim Script Extender (skse)

This section assumes that you have installed [Root Builder for MO2](https://www.nexusmods.com/skyrimspecialedition/mods/31720) correctly. If you have not, you MUST ensure that you have Root Builder and MO2 installed. You must set Root Builder to use USVFS + Links mode.

This section exists as a tutorial for Root Builder. It covers the expectations of the plugin, how it works, why we use it, and how to use it.

You can skip directly to the 'Installing skse' sub-section if you aren't interested in the background.

#### How MO2 works

The reason Mod Organizer 2 is so powerful is because of its Virtual File System (VFS). When you launch MO2 with mods, no data is actually copied into the game's directories. Instead, the mods are put into MO2's VFS and an API hook is used to 'trick' the game into thinking the mod files are there.

This means that your modding install doesn't actually exist on the disk, because the VFS exists in memory. This means that it is ephemeral, making it a perfect candidate for an idempotent, reproducible install that can be replicated so long as the inputs to the install are the same.

The downside of this is that the VFS must first be mounted into memory, which can cause additional memory usage, but the utility and flexibiliy of this approach is a worthy trade-off for the additional memory and CPU overhead.

For more information on MO2's VFS, see [this github repository](https://github.com/ModOrganizer2/usvfs)

#### How Root Builder works

Root Builder is similar, but instead of interacting with the VFS*, it instead uses Symbolic Links (symlinks) to manage your root mods. Because MO2's API hooking happens after the core game files are loaded, we can hook into this process again (as a plugin) to use symlinks to manage these "root" files without permanently cluttering the directory before the VFS is created,

Again, the downside of this is that now we have an additional 'deploy' step before MO2 even loads the VFS in order to actually create the symlinks, meaning that our overall load time increases. Furthermore, in order to ensure that our directory remains untouched, there is an additional cleanup step after the main MO2 VFS process exits, to restore our installation back into its initial state.

\* this is not strictly true, as you can set Root Builder to run in one of many modes.

1. Copy mode, which actually does populate your game installation to ensure maximum compatibility with `.exe` and `.dll` files,
2. Link mode, which is described here,
3. USVFS mode, which simply follows MO2's VFS method (lowest support for `.exe` and `.dll` files), and
4. USVFS + Link mode, which is a hybrid of the USVFS and Link modes, where `.exe` and `.dll` files are symlinked while the rest are in USVFS.
5. Custom mode, which lets you set specific files/file extension to run in USVFS, or Link modes.

For the intent of this guide, we are using USVFS + Links mode.

#### Installing SKSE

1. Download SKSE for Anniversary Edition (build 2.2.6; game version 1.6.1170) [here](https://skse.silverlock.org/).
2. Again, you must set Root Builder to use USVFS + Link mode.
3. Drag and drop the downloaded `skse64_2_02_06.7z` (or any other number of the `skse` archive) into MO2.
4. Inside the MO2 install dialog, un-collapse the `Data` directory (reveal it in the filetree).
5. Right click that `Data` directory and create a directory called `Root` (next to the existing `Scripts` directory).
6. Move the `skse64_1_6_1170.dll` and `skse64_loader.exe` files into the newly created `Root` directory.
7. Right click the `Data` directory again, and select "Set as \<data\> directory". This should change the message at the bottom of the install dialog from "The content of \<data\> does not look valid." to "The content of \<data\> looks valid"
8. Click ok to install the mod (you can rename it to whatever you want as well).

#### Running SKSE

At this point, SKSE is installed. But, if you have experience with SKSE, you'll know that you no longer launch Skyrim through the regular `SkyrimSE.exe`, and instead, launch it using the SKSE loader (`skse64_loader.exe`).

Root Builder accounts for this as well, and lets you point MO2 to existing `.exe`s inside of its own managed folders to launch the VFS with. These are steps for that.

1. Double click the existing installed "skse" (or whatever you named it) mod inside MO2.
2. Click the "Filetree" tab.
3. Click Open Mod in Explorer.
4. Enter the `Root` folder in the opened Explorer window.
5. Copy the path to that `Root` folder.
6. Back in MO2, click the dropdown on "Skyrim Special Edition" at the top right of the MO2 window and click "\<Edit...\>".
7. Click the "+" icon and click "Add from file."
8. Paste the copied path to the `Root` folder into the explorer file path bar and hit enter.
9. Select `skse64_loader.exe`.
10. Click "Apply" in the executables dialog then close it.

And you're done! Now you can launch SKSE using MO2 as usual and it'll be as if you installed SKSE the 'normal' way. If you'd like to confirm that SKSE is in fact running now, you can open the Skyrim console (`~`) and type `getskseversion`. If you did it correctly, it should return the version number.