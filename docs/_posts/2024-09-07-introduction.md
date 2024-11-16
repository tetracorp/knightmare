---
title: "Introduction"
categories: about
---

_Knightmare_ is a first-person _Dungeon Master_ style RPG released by Mindscape
for the Commodore Amiga in 1991. It is based on a popular British gameshow which
ran from 1987 to 1994, although the game differs somewhat from the show.

The game was written by legendary Amiga coder Tony "Ratt" Crowther, who
previously released sci-fi RPG _Captive_ in 1990. Crowther coded the entirety of
_Captive_ by himself in only nine months, including graphics. _Knightmare_
re-used the _Captive_ engine with improvements, this time with graphics by
dedicated artist Jan Thwaites.

### This project

Back in 2014 I started analyzing Gremlin's 1994 real-time strategy game
[K240](https://tetracorp.github.io/k240/), followed by a similar project for the
turn-based dungeon crawler 
[Dungeons of Avalon](https://tetracorp.github.io/dungeons-of-avalon) in 2019.
The goal of these projects, which have been very successful, was to
reverse-engineer the games and understand the various undocumented game
mechanics.

This next project seeks to disassemble _Knightmare_, a task made more
challenging due to Crowther's especially clever programming methods. Instead of
standard file read routines, it uses a custom disk operating system called
RATT-DOS, an anti-piracy measure to prevent copying the disk. The game also uses
multiple executable files.

As usual, the goals are to document the game mechanics and extract graphics and
map data. I also hope to discover the truth of whether the infinite hit points
cheat involving apples and rabbit pies is real or fake.

### Progress

To date, the project has made notable discoveries, including:

- Decoded the [map format](../data/map-format.html),
  including triggers, switches, and monster spawn locations, and presented
  annotated maps.
- Ripped the game's graphics, both
  [graphics files](../data/graphics.html) and
  [internal graphics](../data/internal-graphics.html).
- Debunked the [cheat](../analysis/cheats.html) involving throwing apples at a
  shield.
- Made a rudimentary analysis of the
  [experience and level system](../analysis/experience.html).

Still to determine:

- How damage is calculated
- How statistics affect combat mechanics
- How statistics increase with level
- Build a table of items and their statistics
- The exact mechanics of each spell

### Thanks

- howprice for [Aira Force](https://howprice.itch.io/aira-force) disassembler
- Tim Ruehsen, Frank Wille, and Nicolas Bastien for the original
  [ira](https://aminet.net/package/dev/asm/ira) disassembler
- Codetapper for [Maptapper](https://codetapper.com/amiga/maptapper/), a sprite
  ripper
- Tony Crowther, for developing this game
- Tony McGarry, JOTD and CFou! for the
  [Knightmare WHDLoad](https://whdload.de/games/Knightmare.html)
- Pierre "Lyverbe" Fournier, for the 
  [The Ultimate Captive Guide](https://captive.atari.org/), which was useful for
  some details of Crowther's works
- Pete Jefferson, for the
  [MMDepaCk](https://aminet.net/package/mus/misc/MMDepaCk) music convertor
