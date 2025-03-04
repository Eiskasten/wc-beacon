# Wondercard Beacon

[![License: GPLv3](https://img.shields.io/badge/License-GPLv3-darkgreen.svg)](https://opensource.org/license/gpl-3-0/)

Wondercard Beacon is a command-line application to distribute Pokémon generation IV wondercards over Wi-Fi using a
computer.
It is just a Pokémon Distribution Rom, but for the PC.

## Features

- Distributing wondercards via Wi-Fi
- Decrypting wondercards dumped from distributions
- Edit wondercards or create new from scratch or from existing PGTs

## Differences to the Pokémon Distribution Rom

advantages:

- Does not require an additional Nintendo DS or a flashcart
- No patching of roms is required, thus making interchanging the wondercard much quicker
- Configuring (e.g. region) does not require any patching
- Fully legal since no roms which are intellectual property of Nintendo are involved

disadvantages:

- Does not behave exactly like the Pokémon Distribution Rom
- May not be compatible with all Wi-Fi chips

## Compile From Source

To compile Wondercard Beacon, you will need to have Rust and Cargo installed. You can install them
using [rustup](https://rustup.rs/).

Once you have Rust and Cargo installed, follow these steps:

1. Clone this repository: `git clone https://github.com/Eiskasten/wc-beacon.git`
2. Navigate to the project directory: `cd wc-beacon`
3. Build the application: `cargo build --release`
4. The resulting application will be located at: `target/release/wc-beacon`

## Usage

Currently, Wondercard Beacon is only tested on Linux.
However, it is very likely, that it will work on other operating systems as well.
If you manage to get it to run on an operating system different from Linux, a pull request with a tutorial is
appreciated.

Before you can use the application, you need to make a few preparations.

1. Put your Wi-Fi card into monitor mode
2. Listen to Wi-Fi channel 7

> :warning: Your Wi-Fi card will not be able to retain the internet connection, when in monitor mode.
> If you need an internet connection during the procedure, make sure your computer has an additional network interface.

Please research for yourself how to achieve these requirements.
However, for Linux users, a [script](scripts/prepare-wifi.sh) is available for that.
Just run it using: `sudo ./scripts/prepare-wifi.sh <device>`.

The restriction for channel 7 may be removed in the future.

After these steps, you may finally distribute your wondercards using the example below.

If you want to edit a wondercard, please consult

```sh
./wc-beacon set --help
```

## Examples

Here are some example usages of Wondercard Beacon:

## Linux

```sh
# Distribute the membercard using wlp0s20f3, requires root/sudo
sudo ./wc-beacon dist -p membercard.pcd -r en -d wlp0s20f3
# You can then receive the mystery gift in your pokemon game.

# Decrypt encrypted membercard
./wc-beacon dec -e membercard.pcd.enc -c 1cb4 -a a4:c0:e1:6e:76:80 -p decryped.pcd
```

For further options, run `./wc-beacon dist --help` and `./wc-beacon dec --help`.

`membercard.pcd` and `membercard.pcd.enc` are assumed to be located in the same directory as your executable.

To set the icons on an existing wondercard to darkrai, none and dialga, add a note to the title and make the wondercard
compatible with diamond as well use:

```sh
./wc-beacon set -p membercard.pcd -i 491 -i 0 -i 483  -t 'The \x01d1 Member Card!' -g platinum -g diamond -o membercard.pcd
```

Hint: Some symbols require special encoding, e.g. to put a note into the title or description use `\x01d1`.
For other symbols refer to https://bulbapedia.bulbagarden.net/wiki/Character_encoding_(Generation_IV)#Character_set and
prepend the symbol from the table with `\x`.

Show the new wondercard:

```sh
./wc-beacon info -p membercard.pcd 
```

will output:

```
title: The ♪ Member Card!	icons: Darkrai(491),None(0),Dialga(483)
type: MemberCard	instance: 0	card ID: 18

For more info on how to get DARKRAI,
visit the official Pokémon website.
Be sure to save your game after you
pick up the Member Card at a
Poké Mart.


games: [Diamond, Platinum]
redistribution limit: 0
received: 2009-08-03
```

## Windows

You have to open a cmd windows with administrator privileges and change to the directory where the `wc-beacon.exe` is
located at.
`membercard.pcd` and `membercard.pcd.enc` are assumed to be located in the same directory.

```bat
:: Distribute the membercard using wlp0s20f3, requires administrator
wc-beacon.exe dist -p membercard.pcd -r en -d wlp0s20f3
:: You can then receive the mystery gift in your pokemon game.

:: Decrypt encrypted membercard
wc-beacon.exe dec -e membercard.pcd.enc -c 1cb4 -a a4:c0:e1:6e:76:80 -p decryped.pcd
```

For further options, run `wc-beacon.exe dist --help` and `wc-beacon.exe dec --help`.

## Links

- https://github.com/projectpokemon/EventsGallery: Repository with many wondercards
- https://gbatemp.net/threads/reverse-engineering-pokemon-gen4-wonder-card-wi-fi-distribution.488212: Thread which
  explains the distribution structure

## Other Pokémon Generations

Currently, only generation IV is supported by this application.
However, since the generation V data structure of distributions seem quite similar - although not the same - it may be
possible to distribute generation V wondercards in the future.

## Contributing

Contributions are welcome!
If you encounter any issues or have suggestions for improvements, please open an issue on the GitHub repository.
Pull requests are also appreciated.

## Credits

- [Yuuto](https://gbatemp.net/members/yuuto.435475/): reverse-engineered the distribution process
- [Eiskasten](https://github.com/Eiskasten): implemented this application

## License

This project is licensed under the GPLv3 License - see the [LICENSE.md](LICENSE.md) file for details.
