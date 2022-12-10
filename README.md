# Hacker Zelda: Ocarina of Time (HackerOoT)

This project, based on the [Zelda: Ocarina of Time decompilation project](https://github.com/zeldaret/oot/), aims to be an easy-to-use base to make Zelda: Ocarina of Time mods. **It is not producing a PC port.**

The only build currently supported is Master Quest (Debug), other versions will eventually be supported.

It builds the following ROM:

* zelda_ocarina_mq_dbg.z64

**Note: This repository does not include any of the assets necessary to build the ROM. A prior copy of the game is required to extract the needed assets.**

## HackerN64

**Discord:** <https://discord.gg/brETAakcXr>

## Zelda Decompilation
**Website:** <https://zelda64.dev>

**Discord:** <https://discord.zelda64.dev>

## Installation

Note: instructions for setting up Visual Studio Code can be found [here](docs/vscode.md).

We recommend using WSL on Windows, or native Linux, which the rest of this readme describes. We currently have instructions for

* [Windows](#Windows), with and without WSL
* [macOS](docs/BUILDING_MACOS.md)
* [Linux](#Linux-Native-or-under-WSL--VM), natively or using WSL / VM
* [Docker](docs/BUILDING_DOCKER.md)

(These will also depend on the Linux instructions.)
Some of these may also be out of date or unmaintained; usually our contributors use WSL, Linux, and macOS, so these instructions should be up to date.

### Windows

For Windows 10 or 11, install WSL and a distribution by following this
[WSL Installation Guide](https://docs.microsoft.com/en-us/windows/wsl/install).
We recommend using Ubuntu 20.04 as the Linux distribution.

For older versions of Windows, install a Linux VM or refer to either [Cygwin](docs/BUILDING_CYGWIN.md) or [Docker](docs/BUILDING_DOCKER.md) instructions.


### Linux (Native or under WSL / VM)

#### 1. Install build dependencies

The build process has the following package requirements:

* git
* build-essential
* binutils-mips-linux-gnu
* python3
* libpng-dev

Under Debian / Ubuntu (which we recommend using), you can install them with the following commands:

```bash
sudo apt-get update
sudo apt-get install git build-essential binutils-mips-linux-gnu python3 libpng-dev
```

If you are using GCC as the compiler for Ocarina of Time, you will also need:

* gcc-mips-linux-gnu

#### 2. Clone the repository

**N.B.** If using WSL, we strongly encourage you to clone into WSL's Linux filesystem using Linux's `git`.
Cloning into the Windows filesystem will result in much slower read/write speeds, and often causes issues when Windows copies the files with the wrong line endings, which the compiler IDO cannot handle correctly.

Clone `https://github.com/HackerN64/HackerOoT.git` where you wish to have the project, with a command such as:

```bash
git clone https://github.com/HackerN64/HackerOoT.git
```

This will copy the GitHub repository contents into a new folder in the current directory called `HackerOoT`. Change into this directory before doing anything else:

```bash
cd HackerOoT
```

#### 3. Prepare a base ROM

Copy over your copy of the Master Quest (Debug) ROM inside the root of this new project directory.
Rename the file to "baserom_original.z64", "baserom_original.n64" or "baserom_original.v64", depending on the original extension.

#### 4. Setup the ROM and build process

Setup and extract everything from your ROM with the following command:

```bash
make setup
```

This will generate a new ROM called "baserom.z64" that will have the overdump removed and the header patched.
It will also extract the individual assets from the ROM.

#### 5. Build the ROM

Run make to build the ROM.
Make sure your path to the project is not too long, otherwise this process may error.

```bash
make
```

If all goes well, a new ROM called "zelda_ocarina_mq_debug.z64" should be built and the following text should be printed:

```bash
tools/elf2rom -cic 6105 zelda_ocarina_mq_dbg.elf zelda_ocarina_mq_dbg.z64
```

**NOTE:** to speed up the build, you can either:

* pass `-jN` to `make setup` and `make`, where N is the number of threads to use in the build. The generally-accepted wisdom is to use the number of virtual cores your computer has.
* pass `-j` to `make setup` and `make`, to use as many threads as possible, but beware that this can use too much memory on lower-end systems.

Both of these have the disadvantage that the ordering of the terminal output is scrambled, so for debugging it is best to stick to one thread (i.e. not pass `-j` or `-jN`).

## Compression and Virtual Console

If you are wondering why other n64 zelda roms ar 32 megabytes that is because they are compressed. HackerOOT supports yaz0 and lzo compression. For more information on compression visit [Z64.me's repo](https://github.com/z64me/z64enc#readme). You can compress your rom by doing this comand instead:

```bash
make compress
```
With compression comes Virtual Console support. As of right now put an original USA wad into the root folder, named `basewad.wad`. Then you must run the following commands. Yes you must `make clean` currently.

```bash
make clean
make wad
```

## Contributing

All contributions are welcome. This is a group effort, and even small contributions can make a difference.
Some tasks also don't require much knowledge to get started.

Most discussions happen on our [Discord Server](https://discord.gg/brETAakcXr), where you are welcome to ask if you need help getting started, or if you have any questions regarding this project and other decompilation projects.
