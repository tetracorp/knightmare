# Knightmare disassembled source

The files in this directory are disassembled from the various program files of
the dungeon crawler _Knightmare_ (Amiga, 1991).

- __RaTtDOS__: Tony Crowther's disk operating system.
- __fed_bootfile__: A small startup program which loads the intro and main game.
- __fed_Demo__: Intro.
- __federation__: The main game program of Knightmare. It's named "federation"
  after _Federation Wars_, the working title for Crowther's previous game,
  _Captive_.
- __EndDEMO__: Outro, which plays when you beat the game.

Disassembly was made using
[Aira Force](https://howprice.itch.io/aira-force), a GUI based on the
[ira disassembler](https://aminet.net/package/dev/asm/ira). For each of the
game's programs, there is a `.asm` file, which is 68k assembly language, and a
`.cnf` file, which is the ira config file used to produce the disassembly from
the program. The original programs can be extracted from the disks using the
Knightmare WHDLoad.

Disassembly of this game is believed to constitute fair use for the purposes of
analysis and commentary.

More information and analysis can be found at the project website:

<https://tetracorp.github.io/knightmare>
