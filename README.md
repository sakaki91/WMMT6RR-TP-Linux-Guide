# WMMT6RR-TP-Linux-Guide
Guide to running WANGAN MIDNIGHT MAXIMUM TUNE 6RR on specific hardware in Linux via Teknoparrot, This project is 100% in favor of the existence of [Teknoparrot](https://teknoparrot.com/) and emulation itself!

> [!IMPORTANT]  
>__THIS GUIDE DOES NOT PROVIDE UNAUTHORIZED DUMPS/ROMS IN ANY WAY; IT IS SIMPLY A WAY TO TEACH SIMPLE EMULATION THROUGH A DIFFERENT HOST SYSTEM.__  
>
> __Status__: It's working fine, but it's __NOT__ saving progress, but I'm looking into fixing that!  

*Obviously, every experience is unique when it comes to Teknoparrot and Linux, but this guide has been tested and prepared using the following* [configuration](#configuration-used).

- [[Getting Started]](#getting-started)
	- [[Preparing Prefix]](#preparing-prefix)
	- [[Dependencies]](#dependencies)
	- [[Installation]](#installation)
	- [[Extras]](#extras)
		- [[Explanations]](#explanations)
		- [[Tweaks]](#tweaks)
		- [[Configuration Used]](#configuration-used)

## Getting Started

First, we will need:  
wine, winetricks, lutris, [wine-mono](https://github.com/wine-mono/wine-mono/releases) (Wine Mono, in case your distro doesn't install it automatically).

and also __[Teknoparrot](https://teknoparrot.com/en/Home/Download)__ (more specifically the version: *__TPBootstrapper (Web Installer for TeknoParrot__*)

and also __[WineGE](https://github.com/GloriousEggroll/wine-ge-custom/releases/tag/GE-Proton8-26)__ (*__it was tested several times with pure Wine and Proton, but both showed some additional instabilities__*).

## Preparing Prefix

First, you need to choose a directory where your game and TeknoParrot installation, along with the prefix, will be located. I will use the following location as an example: `~/Teknoparrot`

We will continue creating the basic structure:
	
	$ mkdir ~/Teknoparrot

And then we will organize it this way:

	$ cd ~/Teknoparrot
	$ mkdir Games Prefix Program WineGE

In summary, the three folders we created are: the Games folder, where your memory dump files will be stored; the Program folder, where the Teknoparrot installation will take place; and the Prefix folder, which is the environment in which Teknoparrot will run, and the WineGE folder will be where the wine runner files will be located.

and now:

	$ export WINEPREFIX=~/Teknoparrot/Prefix

This will store that path in the __*WINEPREFIX*__ variable, so we don't have to type the same line every time.

With Wine installed, we will type:

	$ wineboot

so that the basic structure of the prefix can be created.

and for extracting WineGE:

	$ tar -xvf ~/Downloads/wine-lutris-GE-Proton8-26-x86_64.tar.xz ~/Teknoparrot/WineGE/

Remember... if you're using different paths, the commands need to be adapted to the path you're using!

## Dependencies

The Teknoparrot installer itself has dependency issues; the external dependencies we need to download and install are as follows:  

`Microsoft .NET Runtime - 8.0.22`  
`Microsoft Windows Desktop Runtime - 8.0.22`	

Depending on when you're reading this guide, the versions may have been updated, but these are the ones I used!

And in case you need the wine-mono mentioned above:

	$ wine msiexec /i ~/Downloads/wine-mono-9.0.0-x86.msi

After configuring *__WINEPREFIX__* and *__Downloading the .exe Files/Wine-Mono.msi__* above, we will begin maintaining the prefix by first installing the following dependencies:

	$ wine ~/Downloads/dotnet-runtime-8.0.22-win-x64.exe
	$ wine ~/Downloads/windowsdesktop-runtime-8.0.22-win-x64.exe

After that, extract TPBootstrapper.zip to the same folder where it's located (it's just the installer).

	$ cd ~/Downloads && unzip TPBootstrapper.zip

And now we'll finally begin the mess:

	$ winetricks d3dcompiler_43
	$ winetricks d3dcompiler_47
	$ winetricks dotnet48
	$ winetricks vcrun2019

> [!WARNING]
> On GPUs with older drivers like mine (GTX970 + 535), the latest version of dxvk may cause problems with Vulkan 1.3, so we will use an older version with:

	$ winetricks --force dxvk2010

Remember, ideally you should always use the latest version, in this case `DXVK`. If your GPU doesn't have driver issues, you DON'T need to use that specific version.

## Installation

And now we'll install and prepare everything. Don't worry if something seems wrong in terms of performance/window flickering, we'll fix everything! Or at least we'll try, hehe.

	$ wine ~/Downloads/TPBootstrapper.exe

When the installation window appears, select the path we created: `~/Teknoparrot/Program`

and then click on __Full Install__.

In that case, it will look something like this:  

	Games:
	WMMT6RR
	
	Prefix:
	dosdevices  lutris.json  userdef.reg  winetricks.log
	drive_c     system.reg   user.reg
	
	Program:
	CrediarDolphin     MD5                       RPCS3
	ElfLdr2            Metadata                  TeknoParrot
	exception.txt      N2                        TeknoParrotUi_d3d9.log
	FFBBlaster         OpenParrotWin32           TeknoParrotUi.dxvk-cache
	GameProfiles       OpenParrotx64             TeknoParrotUi.exe
	GameSetup          ParrotData.xml            TeknoParrotUi.exe.config
	GLCache            ParrotPatcher.exe         UserProfiles
	HookedWindows.txt  ParrotPatcher.exe.config  Utf8Json.dll
	Icons              ParrotPatcher.pdb
	libs               Play

After that, we will set the prefix version:

	$ winecfg /v win10

And after that, we will open Lutris, and we will add it as a game manually.  
Check the "advanced" box in the upper right corner next to the save button.

In the (Lutris) Game Information:

__Name: Teknoparrot__  
__Runner: Wine__

in the (Lutris) Game Options:

__Executable: /home/youruser/Teknoparrot/Program\TeknoParrotUi.exe__  
__Wine Prefix: /home/youruser/Teknoparrot/Prefix__

in (Lutris) Runner Options:

__Wine Version: Customized (select the executable below)__  
__Customized Wine Executable: /home/youruser/Teknoparrot/WineGE/lutris-GE-Proton8-26-x86_64/bin/wine__

And to add your game to Teknoparrot, look for __"Add Game"__ in the three bars in the upper left corner and search for the game __Wangan Midnight Maximum Tune 6RR__ (or __6__ or __6R__ if you are using another version!). Then, in __Game Settings__, select the path to __wmn6r.exe__ in `~/Teknoparrot/Games/WMMT6RR`.

After that, the game will most likely be working alongside Teknoparrot, and you can open it now!

## Extras

In this section, we will dedicate some adjustments and explanations.

- [[Explanations]](#explanations)
- [[Tweaks]](#tweaks)
- [[Configuration Used]](#configuration-used)

### Explanations

It has been tested several times, the game seems stable, but it most likely has some kind of write problem, as it doesn't save game progress. I'm still trying to intercept this with logs to try and figure out why. I did this in less than one night; it's still very experimental but entirely possible. Please be patient, I'm doing something complex for myself completely for free and I'm putting a lot of effort into it.

Also... if you have any problems, I STRONGLY recommend that you open issues so you can help me keep this project alive. I intend to expand this to other arcade games in the future. Well, that's all for now... have fun!

### Tweaks

I recommend loading the NTSync module for improved overall performance and latency, with:

	$ sudo modprobe ntsync && lsmod | grep ntsync

He needs to return something like:

	ntsync                 20480  0

I highly recommend these tweaks within Teknoparrot itself:  
Go to Teknoparrot __Settings__ and select the option:  
`Disable Hardware Acceleration in UI`  
This will help remove window flickering along with WineGE.

### Configuration Used

__OS:__ Linux Mint 22.2 x86_64   
__Kernel:__ 6.14.0-37-generic  
__CPU:__ Intel i7-9700 (8) @ 4.700GHz  
__GPU:__ NVIDIA GeForce GTX 970  
__Memory:__ 16GiB  

GPU Driver Version: __NVIDIA GTX970 - nvidia-driver-535.274.02__

Teknoparrot UI: __1.0.0.1820__

Wine Version: __wine-9.0 (Ubuntu 9.0~repack-4build3)__ && __lutris-GE-Proton8-26-x86_64__  
Wine Mono: __9.0.0__  
Winetricks Version: __20240105__  
Lutris Version: __lutris-0.5.14__
