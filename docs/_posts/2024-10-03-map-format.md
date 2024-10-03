---
title: Map format and raw data
categories: data
---

The main maps for the game are stored in a 26,584 byte file called `bl_maps`.
The following article breaks down the format of this file and extracts the data.

1. Table of Contents
{:toc}

### File layout

The file is the following format:

Start  | Length | Content
-------|--------|---------------
$0     |     90 | Header
$5a    |  8,192 | Maps
$205a  |    284 | ? (71 x 4)
$2176  |  1,892 | Text strings
$28da  |    292 | Teleporters (73 x 4)
$29fe  |    484 | Pits (80 x 6 + 4)
$2be2  |  1,228 | Signs (307 x 4)
$30ae  |  1,516 | ? (379 x 4)
$369a  |    988 | Traps (247 x 4)
$3a76  |    372 | Doors (62 x 6)
$3bea  |    672 | Switches (112 x 6)
$3e8a  |    880 | Text strings
$41fa  |  7,144 | Monsters
$5de2  |  2,560 | Items
$67d6  |      2 | Padding: `/_` ($2f $5f)

### $0: Header

The file begins with a 90-byte header, mostly consisting of an index to the
sections of the file:

Bytes                                   | Meaning
----------------------------------------|------------------------------
0000 67d6                               | 26,582 - filesize minus 2
035d                                    | ?
0000 005a                               | Offset: Map
0001 0203 0301 0402 0001 0100 0100 0101 | Probably per-map data
0000 0100 0000 0000 0360 a003 2203 0c90 | Probably per-map data
0000 2176                               | Offset: Text strings, signs
0000 28da                               | Offset: Teleporters
0000 30ae                               | Offset: ?
0000 29fe                               | Offset: Pits
0000 3a76                               | Offset: Doors
0000 369a                               | Offset: Traps
0000 2be2                               | Offset: Signs
0000 3bea                               | Offset: Switches
0000 3e8a                               | Offset: Text strings, NPCs
0000 41fa                               | Offset: Monsters
0000 5de2                               | Offset: Items
0000 205a                               | Offset: ?

### $5a: Maps

There are eight maps in the game. Each is 32 x 32 tiles in size, and each tile
is represented by a single byte. The maps in order are:

1. Forest area
2. Quest Three: Sword of Freedom, floor 1
3. Quest Three: Sword of Freedom, floor 2
4. Quest Three: Sword of Freedom, floor 3
5. Quest Two: Cup of Life
6. Quest One: Shield of Justice
7. Quest Four: Crown of Glory, various
8. Quest Four: Crown of Glory, floor 1

All maps are automatically surrounded by solid walls. This differs from the
[Dungeons of Avalon II map format](https://tetracorp.github.io/dungeons-of-avalon/data/dungeon-format.html),
where the map wraps north-south and east-west, allowing for more dynamic map
shapes.

Full maps for each quest will be found at the relevant pages; see the main
index.

### Tile key

Hex| Tile
---|----------------
00 | Solid wall. If any decorations (e.g. switches, Treguard) are defined for
this block, they are on the North side. Blocks 01 to 0f bitwise determine which
faces the decorations are on.
01 | Solid wall (Decorations: N)
02 | Solid wall (Decorations: E)
03 | Solid wall (Decorations: N, E) (unused)
04 | Solid wall (Decorations: S)
05 | Solid wall (Decorations: N, S)
06 | Solid wall (Decorations: E, S)
07 | Solid wall (Decorations: N, E, S) (unused)
08 | Solid wall (Decorations: W)
09 | Solid wall (Decorations: N, W) (unused)
0a | Solid wall (Decorations: E, W)
0b | Solid wall (Decorations: N, E, W)
0c | Solid wall (Decorations: S, W) (unused)
0d | Solid wall (Decorations: N, S, W) (unused)
0e | Solid wall (Decorations: E, S, W)
0f | Solid wall (Decorations: N, E, S, W)
10 | Wall hatch, closed, north
11 | Wall hatch, closed, east
12 | Wall hatch, closed, south
13 | Wall hatch, closed, west
14 | Wall hatch, open, north
15 | Wall hatch, open, east
16 | Wall hatch, open, south
17 | Wall hatch, open, west
18 | Arrowslit
19 | Barred window
1a | Roller? wall
1e | Water
20 | Empty space
21 | Empty space
22 | Pit, instant kill
26 | Grass
27 | Grass
2a | Grass, red flowers
2c | Boat
2e | Floor grate
30 | Floor pit
31 | Floor pit, ladder down
33 | Ceiling pit
34 | Ceiling pit, ladder up
36 | Floor switch
3c | Turn right
3d | Turn left
3e | Illusory wall
40 | Door, closed, north-south, with button
41 | Door, closed, north-south, without button
42 | Door, closed, north-south, windowless
43 | Door, closed, north-south, face
44 | Portcullis, closed, north-south, top opening
45 | Portcullis, closed, north-south, two openings
46 | Portcullis, closed, north-south, no openings
47 | Door, closed, north-south, face
48 | Door, closed, east-west, with button
49 | Door, closed, east-west, without button
4a | Door, closed, east-west, windowless
4b | Door, closed, east-west, face
4c | Portcullis, closed, north-south, top opening
4d | Portcullis, closed, north-south, two openings
4e | Portcullis, closed, north-south, no openings
50 | Door, open, north-south, with button
51 | Door, open, north-south, without button
52 | Door, open, north-south, windowless
53 | Door, open, north-south, face
54 | Portcullis, open, north-south, top opening
55 | Portcullis, open, north-south, two openings
56 | Portcullis, open, north-south, no openings
57 | Door, open, north-south, face
58 | Door, open, east-west, with button
59 | Door, open, east-west, without button
5a | Door, open, east-west, windowless
5b | Door, open, east-west, face
5c | Portcullis, open, north-south, top opening
5d | Portcullis, open, north-south, two openings
5e | Portcullis, open, north-south, no openings
63 | Sprig of Life and Death
64 | Portal
65 | Portal
66 | Invisible switch
68 | Tree
69 | Two trees
6a | Four trees
6b | Hedge
6c | Invisible wall
6f | Well
70 | Fog
71 | Railway
72 | Railway with cart
73 | ?

Additionally, any square appears to be able to add `0x80` to signify that the
square is occupied by a monster and cannot be entered. For example, `0xa0` is
just an occupied `0x20`.

The following tile IDs are not used in any map: 03, 07, 09, 0c, 0d, 14, 15, 16,
17, 1b, 1c, 1d, 1f, 23, 24, 25, 28, 29, 2b, 2d, 2f, 32, 35, 37, 38, 39, 3a, 3b,
3f, 4f, 5f, 60, 61, 62, 67, 6d, 6e. Additionally, no maps use 14 to 17 (open
hatches) or 50 to 5e (open doors); all start closed.

### $205a: ? (71 x 4)

 ? | ?  | ?  | ?
---|----|----|----
00 | 10 | 01 | 01
08 | 90 | 02 | 04
02 | 01 | 0a | 02
00 | 0a | 14 | 00
00 | 10 | 05 | 01
00 | 03 | 05 | 0f
0f | 01 | 4f | 0f
00 | 87 | 14 | 00
00 | 10 | 06 | 01
00 | 00 | 00 | 0f
20 | 5a | b3 | 19
03 | a7 | 14 | 00
00 | 10 | 07 | 01
00 | 01 | 02 | 09
1f | 64 | 8b | 0f
01 | b3 | 14 | 00
00 | 1c | 08 | 04
00 | 02 | 04 | 05
1d | 00 | bf | 0a
00 | eb | 00 | 00
00 | eb | 02 | 00
00 | eb | 06 | 00
00 | eb | 08 | 00
00 | 10 | 09 | 01
00 | 02 | 0a | 0b
10 | 6e | 6d | 1e
04 | 6f | 14 | 00
00 | 10 | 0a | 01
11 | 01 | 03 | 0b
0c | 6e | 6d | 1e
02 | 7b | 14 | 00
00 | 14 | 0b | 02
00 | 03 | 02 | 05
1d | 00 | bf | 0a
00 | 9b | 00 | 00
00 | 9b | 02 | 00
00 | 10 | 0c | 01
00 | 00 | 03 | 05
17 | 00 | 6f | 19
01 | b3 | 14 | 00
00 | 10 | 0d | 01
00 | 02 | 04 | 05
1c | 00 | 6f | 14
01 | 4f | 14 | 00
00 | 10 | 03 | 01
11 | 00 | 08 | 0f
0c | 01 | 4f | 19
01 | b3 | 14 | 00
00 | 10 | 0e | 01
00 | 01 | 03 | 05
1c | 00 | 6f | 0a
00 | 87 | 14 | 00
00 | 10 | 04 | 01
00 | 93 | 04 | 06
06 | 01 | 6e | 16
00 | c8 | 14 | 00
00 | 10 | 00 | 01
00 | 02 | 08 | 0f
0d | 01 | 77 | 23
00 | 23 | 14 | 00
00 | 14 | 02 | 02
00 | 02 | 03 | 0f
13 | 01 | 4f | 19
00 | 73 | 00 | 00
00 | 73 | 02 | 00
00 | 14 | 0f | 02
00 | 03 | 01 | 03
21 | 00 | 6f | 5a
00 | eb | 00 | 00
00 | eb | 02 | 00
00 | 00 | 00 | 00

### $2176: Text strings, signs

A set of 63 text strings, of variable length. The structure is as follows: two
bytes for the length of the string (including all its headers), two bytes for
the map offset address, four unknown bytes, and the text in ASCII terminated by
null bytes. Non-standard control codes are used for newlines.

These text strings are used for the Treguard messages on the walls.

Addr | Map        | Coords      | ?           | Text                      
-----|------------|-------------|-------------|---------------------------
033d | 0: Forest  | X: 29 Y: 25 | 00 01 02 09 | WELCOME TO YOUR KNIGHTMARE
0321 | 0: Forest  | X: 01 Y: 25 | 00 01 02 10 | YOUR CARRIAGE AWAITS YOU
031a | 0: Forest  | X: 26 Y: 24 | 01 02 02 0f | TAKE THIS SHORT CUT TO THE<br>FOREST
000e | 0: Forest  | X: 14 Y: 00 | 00 01 02 13 | YOUR QUEST STARTS HERE
0332 | 0: Forest  | X: 18 Y: 25 | 00 02 02 34 | QUEST ONE<br>THE SHIELD OF JUSTICE
0180 | 0: Forest  | X: 00 Y: 12 | 00 02 02 34 | QUEST TWO<br>THE CUP OF LIFE
0138 | 0: Forest  | X: 24 Y: 09 | 00 02 02 30 | QUEST THREE<br>THE SWORD OF FREEDOM
00e0 | 0: Forest  | X: 00 Y: 07 | 00 02 02 32 | QUEST FOUR<br>THE CROWN OF GLORY
04c4 | 1: Sword 1 | X: 04 Y: 06 | 00 02 02 10 | HIT THE BUTTON WHEN READY<br>FOR HELL
04a7 | 1: Sword 1 | X: 07 Y: 05 | 00 01 02 25 | THE GAME ROOMS
0471 | 1: Sword 1 | X: 17 Y: 03 | 01 01 02 10 | ONLY PUSH THE RIGHT ONES
04ec | 1: Sword 1 | X: 12 Y: 07 | 00 01 02 1e | THE WORD IS SECRET
050d | 1: Sword 1 | X: 13 Y: 08 | 00 02 02 0e | YOU MAKE THE FIRE. CAN YOU<br>STOP IT?
04f2 | 1: Sword 1 | X: 18 Y: 07 | 01 01 02 22 | THE MONSTER RAID
057c | 1: Sword 1 | X: 28 Y: 11 | 00 01 02 0a | LET THEM OUT AND FACE THEM
0517 | 1: Sword 1 | X: 23 Y: 08 | 00 01 02 18 | BEWARE THE GUARDIANS
0a70 | 2: Sword 2 | X: 16 Y: 19 | 01 01 02 0c | WHEN IS A WELL NOT A WELL
09f3 | 2: Sword 2 | X: 19 Y: 15 | 01 01 02 30 | RUN AWAY!!!
08f7 | 2: Sword 2 | X: 23 Y: 07 | 01 01 02 14 | BREAD TRAILS ARE HANDY
0911 | 2: Sword 2 | X: 17 Y: 08 | 00 01 02 23 | BEWARE THE TRAP
0959 | 2: Sword 2 | X: 25 Y: 10 | 00 01 02 2a | LOTS OF WORK
09c3 | 2: Sword 2 | X: 03 Y: 14 | 01 01 02 21 | REMEMBER TO HIDE
0863 | 2: Sword 2 | X: 03 Y: 03 | 00 01 02 28 | HUNT FOR GOLD
0de6 | 3: Sword 3 | X: 06 Y: 15 | 00 01 02 12 | WELCOME TO THE DUNGEON
0dbf | 3: Sword 3 | X: 31 Y: 13 | 00 02 02 33 | WELL DONE<br>THE SWORD IS YOURS
1441 | 5: Shield  | X: 01 Y: 02 | 00 02 02 0f | YOUR QUEST FOR THE SHIELD<br>HAS BEGUN
1443 | 5: Shield  | X: 03 Y: 02 | 00 01 02 0a | THE SPRIG OF LIFE AND DEATH
1481 | 5: Shield  | X: 01 Y: 04 | 00 01 02 18 | DO NOT PLAY WITH FIRE
142f | 5: Shield  | X: 15 Y: 01 | 00 01 02 1e | KEEP THE PAD DOWN
1587 | 5: Shield  | X: 07 Y: 12 | 00 01 02 22 | SEWER ENTERANCE
143d | 5: Shield  | X: 29 Y: 01 | 00 01 02 18 | THE HOME OF MR CHILD
1637 | 5: Shield  | X: 23 Y: 17 | 01 01 02 31 | THE PRISON
17dc | 5: Shield  | X: 28 Y: 30 | 00 02 02 32 | WELL DONE!<br>QUEST ONE COMPLETED
114a | 4: Cup     | X: 10 Y: 10 | 00 01 02 2e | MYSTIC DOOR
1183 | 4: Cup     | X: 03 Y: 12 | 00 01 02 1a | THE MYSTIC CHAMBERS
1161 | 4: Cup     | X: 01 Y: 11 | 01 01 02 1e | NOBODY MUST ENTER
10a5 | 4: Cup     | X: 05 Y: 05 | 01 01 02 31 | HOLY MOLY
1009 | 4: Cup     | X: 09 Y: 00 | 00 01 02 12 | YOUR EYES TELL YOU LIES
1304 | 4: Cup     | X: 04 Y: 24 | 01 01 02 22 | THE CUP GARDIANS
117e | 4: Cup     | X: 30 Y: 11 | 00 01 02 2c | GOLEMS LAND
12db | 4: Cup     | X: 27 Y: 22 | 01 01 02 19 | ENTER THE WARP MAZE
0442 | 1: Sword 1 | X: 02 Y: 02 | 00 01 02 18 | THE CONVEYOR OF LIFE
1622 | 5: Shield  | X: 02 Y: 17 | 00 01 02 25 | TARGET PRACTICE
1b9e | 6: Crown 2 | X: 30 Y: 28 | 00 02 02 0d | THE ROUTE TO THE CASTLE OF<br>FEAR
1c40 | 7: Crown 1 | X: 00 Y: 02 | 00 01 02 1f | THE TRAINING ROOMS
096f | 2: Sword 2 | X: 15 Y: 11 | 01 01 02 1c | THE SOUNDS OF HELL
1a05 | 6: Crown 2 | X: 05 Y: 16 | 00 01 02 11 | RETURN THE THREE STAFFS
0688 | 1: Sword 1 | X: 08 Y: 20 | 00 01 02 2a | GOLDEY LOCKS
0fa7 | 3: Sword 3 | X: 07 Y: 29 | 00 01 02 1c | RESET IN THE CENTRE
0e60 | 3: Sword 3 | X: 00 Y: 19 | 01 01 02 34 | ALPHABET
0f66 | 3: Sword 3 | X: 06 Y: 27 | 01 01 02 33 | BREACHES
0e87 | 3: Sword 3 | X: 07 Y: 20 | 01 01 02 38 | CLOSED
0f60 | 3: Sword 3 | X: 00 Y: 27 | 01 01 02 3b | DOORS
0f47 | 3: Sword 3 | X: 07 Y: 26 | 00 01 02 0e | THE ROOMS OF THE HEALERS
0f89 | 3: Sword 3 | X: 09 Y: 28 | 00 02 02 12 | TAKE YOUR TIME AND STAND<br>BACK
06d1 | 1: Sword 1 | X: 17 Y: 22 | 00 01 02 2d | DIZZY ROOMS
073d | 1: Sword 1 | X: 29 Y: 25 | 00 01 02 25 | EVIL TO THE END
0a36 | 2: Sword 2 | X: 22 Y: 17 | 01 02 02 0f | YOU ARE DESTINED TO DIE OF<br>OLD AGE
031b | 0: Forest  | X: 27 Y: 24 | 01 01 02 25 | ENJOY YOUR RIDE
011b | 0: Forest  | X: 27 Y: 08 | 00 01 02 1f | BEWARE THE SWAMP
17db | 5: Shield  | X: 27 Y: 30 | 0a 02 02 37 | KEEP OUT<br>PRIVATE POOLS
07db | 1: Sword 1 | X: 27 Y: 30 | 01 01 02 20 | HUNT FOR A SWITCH
03bf | 0: Forest  | X: 31 Y: 29 | 00 01 02 15 | RETURN THE CROWN HERE

### $28da: Teleporters

73 4-byte locations of teleporter doors. Two bytes each for the map coordinates.

Addr | Map        | Coords      |Target| Map        | Coords      
-----|------------|-------------|------|------------|-------------
035f | 0: Forest  | X: 31 Y: 26 | 0340 | 0: Forest  | X: 00 Y: 26 
0320 | 0: Forest  | X: 00 Y: 25 | 035e | 0: Forest  | X: 30 Y: 26 
033b | 0: Forest  | X: 27 Y: 25 | 002e | 0: Forest  | X: 14 Y: 01 
000d | 0: Forest  | X: 13 Y: 00 | 033a | 0: Forest  | X: 26 Y: 25 
0158 | 0: Forest  | X: 24 Y: 10 | 0404 | 1: Sword 1 | X: 04 Y: 00 
0160 | 0: Forest  | X: 00 Y: 11 | 122e | 4: Cup     | X: 14 Y: 17 
0331 | 0: Forest  | X: 17 Y: 25 | 1401 | 5: Shield  | X: 01 Y: 00 
00c0 | 0: Forest  | X: 00 Y: 06 | 1c61 | 7: Crown 1 | X: 01 Y: 03 
0a43 | 2: Sword 2 | X: 03 Y: 18 | 0a00 | 2: Sword 2 | X: 00 Y: 16 
0a44 | 2: Sword 2 | X: 04 Y: 18 | 09e9 | 2: Sword 2 | X: 09 Y: 15 
08b8 | 2: Sword 2 | X: 24 Y: 05 | 08b9 | 2: Sword 2 | X: 25 Y: 05 
08ba | 2: Sword 2 | X: 26 Y: 05 | 089a | 2: Sword 2 | X: 26 Y: 04 
087a | 2: Sword 2 | X: 26 Y: 03 | 085a | 2: Sword 2 | X: 26 Y: 02 
0859 | 2: Sword 2 | X: 25 Y: 02 | 083b | 2: Sword 2 | X: 27 Y: 01 
083c | 2: Sword 2 | X: 28 Y: 01 | 083d | 2: Sword 2 | X: 29 Y: 01 
085d | 2: Sword 2 | X: 29 Y: 02 | 087d | 2: Sword 2 | X: 29 Y: 03 
087c | 2: Sword 2 | X: 28 Y: 03 | 08de | 2: Sword 2 | X: 30 Y: 06 
08be | 2: Sword 2 | X: 30 Y: 05 | 08bf | 2: Sword 2 | X: 31 Y: 05 
0897 | 2: Sword 2 | X: 23 Y: 04 | 0918 | 2: Sword 2 | X: 24 Y: 08 
08d9 | 2: Sword 2 | X: 25 Y: 06 | 08dc | 2: Sword 2 | X: 28 Y: 06 
0899 | 2: Sword 2 | X: 25 Y: 04 | 08dc | 2: Sword 2 | X: 28 Y: 06 
0819 | 2: Sword 2 | X: 25 Y: 00 | 08dc | 2: Sword 2 | X: 28 Y: 06 
083a | 2: Sword 2 | X: 26 Y: 01 | 08b7 | 2: Sword 2 | X: 23 Y: 05 
08db | 2: Sword 2 | X: 27 Y: 06 | 08b7 | 2: Sword 2 | X: 23 Y: 05 
089b | 2: Sword 2 | X: 27 Y: 04 | 081a | 2: Sword 2 | X: 26 Y: 00 
085b | 2: Sword 2 | X: 27 Y: 02 | 0839 | 2: Sword 2 | X: 25 Y: 01 
081b | 2: Sword 2 | X: 27 Y: 00 | 08d8 | 2: Sword 2 | X: 24 Y: 06 
08bc | 2: Sword 2 | X: 28 Y: 05 | 081a | 2: Sword 2 | X: 26 Y: 00 
08dd | 2: Sword 2 | X: 29 Y: 06 | 08b7 | 2: Sword 2 | X: 23 Y: 05 
089d | 2: Sword 2 | X: 29 Y: 04 | 081a | 2: Sword 2 | X: 26 Y: 00 
081d | 2: Sword 2 | X: 29 Y: 00 | 0879 | 2: Sword 2 | X: 25 Y: 03 
08df | 2: Sword 2 | X: 31 Y: 06 | 08da | 2: Sword 2 | X: 26 Y: 06 
0c0a | 3: Sword 3 | X: 10 Y: 00 | 0a50 | 2: Sword 2 | X: 16 Y: 18 
0c00 | 3: Sword 3 | X: 00 Y: 00 | 0de5 | 3: Sword 3 | X: 05 Y: 15 
0dc6 | 3: Sword 3 | X: 06 Y: 14 | 0dc8 | 3: Sword 3 | X: 08 Y: 14 
0dc7 | 3: Sword 3 | X: 07 Y: 14 | 0dc5 | 3: Sword 3 | X: 05 Y: 14 
0e5d | 3: Sword 3 | X: 29 Y: 18 | 0dbe | 3: Sword 3 | X: 30 Y: 13 
0ddf | 3: Sword 3 | X: 31 Y: 14 | 002e | 0: Forest  | X: 14 Y: 01 
1400 | 5: Shield  | X: 00 Y: 00 | 0312 | 0: Forest  | X: 18 Y: 24 
145c | 5: Shield  | X: 28 Y: 02 | 15f6 | 5: Shield  | X: 22 Y: 15 
15f5 | 5: Shield  | X: 21 Y: 15 | 143b | 5: Shield  | X: 27 Y: 01 
116c | 4: Cup     | X: 12 Y: 11 | 110e | 4: Cup     | X: 14 Y: 08 
119e | 4: Cup     | X: 30 Y: 12 | 1150 | 4: Cup     | X: 16 Y: 10 
1176 | 4: Cup     | X: 22 Y: 11 | 116a | 4: Cup     | X: 10 Y: 11 
1100 | 4: Cup     | X: 00 Y: 08 | 11c1 | 4: Cup     | X: 01 Y: 14 
11e1 | 4: Cup     | X: 01 Y: 15 | 1100 | 4: Cup     | X: 00 Y: 08 
12a0 | 4: Cup     | X: 00 Y: 21 | 00ee | 0: Forest  | X: 14 Y: 07 
0480 | 1: Sword 1 | X: 00 Y: 04 | 0421 | 1: Sword 1 | X: 01 Y: 01 
04c2 | 1: Sword 1 | X: 02 Y: 06 | 0421 | 1: Sword 1 | X: 01 Y: 01 
0500 | 1: Sword 1 | X: 00 Y: 08 | 0421 | 1: Sword 1 | X: 01 Y: 01 
1e9f | 7: Crown 1 | X: 31 Y: 20 | 1ebb | 7: Crown 1 | X: 27 Y: 21 
1dc5 | 7: Crown 1 | X: 05 Y: 14 | 1f57 | 7: Crown 1 | X: 23 Y: 26 
1a4f | 6: Crown 2 | X: 15 Y: 18 | 1821 | 6: Crown 2 | X: 01 Y: 01 
1801 | 6: Crown 2 | X: 01 Y: 00 | 1a30 | 6: Crown 2 | X: 16 Y: 17 
189d | 6: Crown 2 | X: 29 Y: 04 | 187f | 6: Crown 2 | X: 31 Y: 03 
189f | 6: Crown 2 | X: 31 Y: 04 | 18bc | 6: Crown 2 | X: 28 Y: 05 
18dd | 6: Crown 2 | X: 29 Y: 06 | 1996 | 6: Crown 2 | X: 22 Y: 12 
1997 | 6: Crown 2 | X: 23 Y: 12 | 18bc | 6: Crown 2 | X: 28 Y: 05 
18bd | 6: Crown 2 | X: 29 Y: 05 | 1811 | 6: Crown 2 | X: 17 Y: 00 
1812 | 6: Crown 2 | X: 18 Y: 00 | 18bc | 6: Crown 2 | X: 28 Y: 05 
191f | 6: Crown 2 | X: 31 Y: 08 | 1a1f | 6: Crown 2 | X: 31 Y: 16 
1978 | 6: Crown 2 | X: 24 Y: 11 | 1811 | 6: Crown 2 | X: 17 Y: 00 
1b58 | 6: Crown 2 | X: 24 Y: 26 | 1bbe | 6: Crown 2 | X: 30 Y: 29 
1ab3 | 6: Crown 2 | X: 19 Y: 21 | 1abb | 6: Crown 2 | X: 27 Y: 21 
1ce9 | 7: Crown 1 | X: 09 Y: 07 | 00c1 | 0: Forest  | X: 01 Y: 06 
1bfb | 6: Crown 2 | X: 27 Y: 31 | 1985 | 6: Crown 2 | X: 05 Y: 12 
0fe0 | 3: Sword 3 | X: 00 Y: 31 | 0f0f | 3: Sword 3 | X: 15 Y: 24 
0f0f | 3: Sword 3 | X: 15 Y: 24 | 0fe0 | 3: Sword 3 | X: 00 Y: 31 
0714 | 1: Sword 1 | X: 20 Y: 24 | 06bc | 1: Sword 1 | X: 28 Y: 21 
0b43 | 2: Sword 2 | X: 03 Y: 26 | 0ac3 | 2: Sword 2 | X: 03 Y: 22 
0b9f | 2: Sword 2 | X: 31 Y: 28 | 0bb9 | 2: Sword 2 | X: 25 Y: 29 
0bc9 | 2: Sword 2 | X: 09 Y: 30 | 0bbf | 2: Sword 2 | X: 31 Y: 29 
0000 |            |             | 0000 |            |             

### $29fe: Pits

80 6-byte entries, plus a 4-byte footer. Two bytes source map address, two bytes
destination address, two other bytes.

Addr | Map        | Coords      |Target| Map        | Coords      | ?  | ?
-----|------------|-------------|------|------------|-------------|----|----
0a30 | 2: Sword 2 | X: 16 Y: 17 | 0630 | 1: Sword 1 | X: 16 Y: 17 | a0 | 60 
0a02 | 2: Sword 2 | X: 02 Y: 16 | 0a42 | 2: Sword 2 | X: 02 Y: 18 | a0 | a0 
09e3 | 2: Sword 2 | X: 03 Y: 15 | 0a24 | 2: Sword 2 | X: 04 Y: 17 | a0 | a0 
0a51 | 2: Sword 2 | X: 17 Y: 18 | 0c61 | 3: Sword 3 | X: 01 Y: 03 | a0 | 03 
17de | 5: Shield  | X: 30 Y: 30 | 012d | 0: Forest  | X: 13 Y: 09 | 03 | 03 
110e | 4: Cup     | X: 14 Y: 08 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
10a6 | 4: Cup     | X: 06 Y: 05 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
10c7 | 4: Cup     | X: 07 Y: 06 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
10e7 | 4: Cup     | X: 07 Y: 07 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
10e8 | 4: Cup     | X: 08 Y: 07 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
1106 | 4: Cup     | X: 06 Y: 08 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
1108 | 4: Cup     | X: 08 Y: 08 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
1126 | 4: Cup     | X: 06 Y: 09 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
1127 | 4: Cup     | X: 07 Y: 09 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
1128 | 4: Cup     | X: 08 Y: 09 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
1147 | 4: Cup     | X: 07 Y: 10 | 1156 | 4: Cup     | X: 22 Y: 10 | 22 | 22 
12a5 | 4: Cup     | X: 05 Y: 21 | 1305 | 4: Cup     | X: 05 Y: 24 | a0 | 22 
1c17 | 7: Crown 1 | X: 23 Y: 00 | 0401 | 1: Sword 1 | X: 01 Y: 00 | 90 | 84 
1caa | 7: Crown 1 | X: 10 Y: 05 | 1cc8 | 7: Crown 1 | X: 08 Y: 06 | 88 | 90 
1620 | 5: Shield  | X: 00 Y: 17 | 1cba | 7: Crown 1 | X: 26 Y: 05 | 28 | 90 
1e55 | 7: Crown 1 | X: 21 Y: 18 | 1d9c | 7: Crown 1 | X: 28 Y: 12 | 90 | 44 
1ef9 | 7: Crown 1 | X: 25 Y: 23 | 1ebf | 7: Crown 1 | X: 31 Y: 21 | 44 | 44 
1efc | 7: Crown 1 | X: 28 Y: 23 | 1ebf | 7: Crown 1 | X: 31 Y: 21 | 44 | 44 
1f38 | 7: Crown 1 | X: 24 Y: 25 | 1de5 | 7: Crown 1 | X: 05 Y: 15 | 44 | 44 
1f58 | 7: Crown 1 | X: 24 Y: 26 | 1e05 | 7: Crown 1 | X: 05 Y: 16 | 44 | 44 
1f78 | 7: Crown 1 | X: 24 Y: 27 | 1e25 | 7: Crown 1 | X: 05 Y: 17 | 44 | 44 
1da8 | 7: Crown 1 | X: 08 Y: 13 | 11e4 | 4: Cup     | X: 04 Y: 15 | 90 | 88 
1d67 | 7: Crown 1 | X: 07 Y: 11 | 1bff | 6: Crown 2 | X: 31 Y: 31 | 90 | 44 
1842 | 6: Crown 2 | X: 02 Y: 02 | 18ab | 6: Crown 2 | X: 11 Y: 05 | 44 | 44 
1908 | 6: Crown 2 | X: 08 Y: 08 | 1971 | 6: Crown 2 | X: 17 Y: 11 | 44 | 44 
190e | 6: Crown 2 | X: 14 Y: 08 | 18b7 | 6: Crown 2 | X: 23 Y: 05 | 44 | 44 
181c | 6: Crown 2 | X: 28 Y: 00 | 1818 | 6: Crown 2 | X: 24 Y: 00 | 44 | 44 
1816 | 6: Crown 2 | X: 22 Y: 00 | 1806 | 6: Crown 2 | X: 06 Y: 00 | 44 | 44 
1804 | 6: Crown 2 | X: 04 Y: 00 | 1880 | 6: Crown 2 | X: 00 Y: 04 | 44 | 44 
18c0 | 6: Crown 2 | X: 00 Y: 06 | 18df | 6: Crown 2 | X: 31 Y: 06 | 44 | 44 
182d | 6: Crown 2 | X: 13 Y: 01 | 1956 | 6: Crown 2 | X: 22 Y: 10 | 44 | 44 
182f | 6: Crown 2 | X: 15 Y: 01 | 1958 | 6: Crown 2 | X: 24 Y: 10 | 44 | 44 
1b9d | 6: Crown 2 | X: 29 Y: 28 | 1b37 | 6: Crown 2 | X: 23 Y: 25 | 44 | 44 
1aba | 6: Crown 2 | X: 26 Y: 21 | 1af5 | 6: Crown 2 | X: 21 Y: 23 | 44 | 44 
1987 | 6: Crown 2 | X: 07 Y: 12 | 0648 | 1: Sword 1 | X: 08 Y: 18 | 44 | 05 
1981 | 6: Crown 2 | X: 01 Y: 12 | 1bda | 6: Crown 2 | X: 26 Y: 30 | 44 | 44 
19a1 | 6: Crown 2 | X: 01 Y: 13 | 1bfa | 6: Crown 2 | X: 26 Y: 31 | 44 | 44 
064a | 1: Sword 1 | X: 10 Y: 18 | 06c3 | 1: Sword 1 | X: 03 Y: 22 | 60 | 06 
0641 | 1: Sword 1 | X: 01 Y: 18 | 064b | 1: Sword 1 | X: 11 Y: 18 | 60 | 05 
068e | 1: Sword 1 | X: 14 Y: 20 | 06ef | 1: Sword 1 | X: 15 Y: 23 | 05 | 24 
0eef | 3: Sword 3 | X: 15 Y: 23 | 19a5 | 6: Crown 2 | X: 05 Y: 13 | 09 | 44 
0ed2 | 3: Sword 3 | X: 18 Y: 22 | 0cf3 | 3: Sword 3 | X: 19 Y: 07 | 09 | 09 
067a | 1: Sword 1 | X: 26 Y: 19 | 0713 | 1: Sword 1 | X: 19 Y: 24 | 24 | 24 
065b | 1: Sword 1 | X: 27 Y: 18 | 06f4 | 1: Sword 1 | X: 20 Y: 23 | 24 | 24 
065d | 1: Sword 1 | X: 29 Y: 18 | 06f4 | 1: Sword 1 | X: 20 Y: 23 | 24 | 24 
069b | 1: Sword 1 | X: 27 Y: 20 | 06f4 | 1: Sword 1 | X: 20 Y: 23 | 24 | 24 
067e | 1: Sword 1 | X: 30 Y: 19 | 0715 | 1: Sword 1 | X: 21 Y: 24 | 24 | 24 
06ba | 1: Sword 1 | X: 26 Y: 21 | 0713 | 1: Sword 1 | X: 19 Y: 24 | 24 | 24 
06da | 1: Sword 1 | X: 26 Y: 22 | 0713 | 1: Sword 1 | X: 19 Y: 24 | 24 | 24 
06dc | 1: Sword 1 | X: 28 Y: 22 | 0734 | 1: Sword 1 | X: 20 Y: 25 | 24 | 24 
06de | 1: Sword 1 | X: 30 Y: 22 | 0715 | 1: Sword 1 | X: 21 Y: 24 | 24 | 24 
02f9 | 0: Forest  | X: 25 Y: 23 | 0aff | 2: Sword 2 | X: 31 Y: 23 | 84 | 50 
0b3c | 2: Sword 2 | X: 28 Y: 25 | 0a9d | 2: Sword 2 | X: 29 Y: 20 | 50 | 50 
0018 | 0: Forest  | X: 24 Y: 00 | 0adb | 2: Sword 2 | X: 27 Y: 22 | 84 | 84 
0b1b | 2: Sword 2 | X: 27 Y: 24 | 0b39 | 2: Sword 2 | X: 25 Y: 25 | 84 | 50 
0a66 | 2: Sword 2 | X: 06 Y: 19 | 0b46 | 2: Sword 2 | X: 06 Y: 26 | a0 | 50 
0a86 | 2: Sword 2 | X: 06 Y: 20 | 0b66 | 2: Sword 2 | X: 06 Y: 27 | a0 | 50 
0aa6 | 2: Sword 2 | X: 06 Y: 21 | 0b86 | 2: Sword 2 | X: 06 Y: 28 | a0 | 50 
0ac6 | 2: Sword 2 | X: 06 Y: 22 | 0ba6 | 2: Sword 2 | X: 06 Y: 29 | a0 | 50 
0ae6 | 2: Sword 2 | X: 06 Y: 23 | 0bc6 | 2: Sword 2 | X: 06 Y: 30 | a0 | 50 
0b06 | 2: Sword 2 | X: 06 Y: 24 | 0be6 | 2: Sword 2 | X: 06 Y: 31 | a0 | 50 
0aa1 | 2: Sword 2 | X: 01 Y: 21 | 0b81 | 2: Sword 2 | X: 01 Y: 28 | a0 | 50 
0aa2 | 2: Sword 2 | X: 02 Y: 21 | 0b82 | 2: Sword 2 | X: 02 Y: 28 | a0 | 50 
0aa3 | 2: Sword 2 | X: 03 Y: 21 | 0b83 | 2: Sword 2 | X: 03 Y: 28 | a0 | 50 
0aa4 | 2: Sword 2 | X: 04 Y: 21 | 0b84 | 2: Sword 2 | X: 04 Y: 28 | a0 | 50 
0ae2 | 2: Sword 2 | X: 02 Y: 23 | 0bc2 | 2: Sword 2 | X: 02 Y: 30 | a0 | 50 
0ae4 | 2: Sword 2 | X: 04 Y: 23 | 0bc4 | 2: Sword 2 | X: 04 Y: 30 | a0 | 50 
031d | 0: Forest  | X: 29 Y: 24 | 1983 | 6: Crown 2 | X: 03 Y: 12 | 84 | 44 
1a64 | 6: Crown 2 | X: 04 Y: 19 | 17fc | 5: Shield  | X: 28 Y: 31 | 44 | 03 
19eb | 6: Crown 2 | X: 11 Y: 15 | 1a47 | 6: Crown 2 | X: 07 Y: 18 | 84 | 44 
1aa6 | 6: Crown 2 | X: 06 Y: 21 | 1aae | 6: Crown 2 | X: 14 Y: 21 | 84 | 84 
1af0 | 6: Crown 2 | X: 16 Y: 23 | 1acb | 6: Crown 2 | X: 11 Y: 22 | 84 | 84 
1b46 | 6: Crown 2 | X: 06 Y: 26 | 07dc | 1: Sword 1 | X: 28 Y: 30 | 84 | 84 
1ce3 | 7: Crown 1 | X: 03 Y: 07 | 1d01 | 7: Crown 1 | X: 01 Y: 08 | 44 | 84 
1e23 | 7: Crown 1 | X: 03 Y: 17 | 0bdf | 2: Sword 2 | X: 31 Y: 30 | 84 | c0 

### $2be2: Signs

For things like the Treguard signs. 307 4-byte rows: two bytes for map coords,
one byte unknown, one byte padding always 00.

Addr | Map        | Coords      |
-----|------------|-------------|-----
e33d | 0: Forest  | X: 29 Y: 25 | 10
0321 | 0: Forest  | X: 01 Y: 25 | 10
031a | 0: Forest  | X: 26 Y: 24 | 10
000e | 0: Forest  | X: 14 Y: 00 | 10
0332 | 0: Forest  | X: 18 Y: 25 | 10
0180 | 0: Forest  | X: 00 Y: 12 | 10
0138 | 0: Forest  | X: 24 Y: 09 | 10
00e0 | 0: Forest  | X: 00 Y: 07 | 10
04c4 | 1: Sword 1 | X: 04 Y: 06 | 10
04a7 | 1: Sword 1 | X: 07 Y: 05 | 10
0471 | 1: Sword 1 | X: 17 Y: 03 | 10
04ec | 1: Sword 1 | X: 12 Y: 07 | 10
050d | 1: Sword 1 | X: 13 Y: 08 | 10
04f2 | 1: Sword 1 | X: 18 Y: 07 | 10
057c | 1: Sword 1 | X: 28 Y: 11 | 10
0517 | 1: Sword 1 | X: 23 Y: 08 | 10
0a70 | 2: Sword 2 | X: 16 Y: 19 | 10
09f3 | 2: Sword 2 | X: 19 Y: 15 | 10
08f7 | 2: Sword 2 | X: 23 Y: 07 | 10
0911 | 2: Sword 2 | X: 17 Y: 08 | 10
0959 | 2: Sword 2 | X: 25 Y: 10 | 10
09c3 | 2: Sword 2 | X: 03 Y: 14 | 10
0863 | 2: Sword 2 | X: 03 Y: 03 | 10
0de6 | 3: Sword 3 | X: 06 Y: 15 | 10
0dbf | 3: Sword 3 | X: 31 Y: 13 | 10
1441 | 5: Shield  | X: 01 Y: 02 | 10
1443 | 5: Shield  | X: 03 Y: 02 | 10
1481 | 5: Shield  | X: 01 Y: 04 | 10
142f | 5: Shield  | X: 15 Y: 01 | 10
1587 | 5: Shield  | X: 07 Y: 12 | 10
143d | 5: Shield  | X: 29 Y: 01 | 10
1637 | 5: Shield  | X: 23 Y: 17 | 10
17dc | 5: Shield  | X: 28 Y: 30 | 10
114a | 4: Cup     | X: 10 Y: 10 | 10
1183 | 4: Cup     | X: 03 Y: 12 | 10
1161 | 4: Cup     | X: 01 Y: 11 | 10
10a5 | 4: Cup     | X: 05 Y: 05 | 10
1009 | 4: Cup     | X: 09 Y: 00 | 10
1304 | 4: Cup     | X: 04 Y: 24 | 10
117e | 4: Cup     | X: 30 Y: 11 | 10
12db | 4: Cup     | X: 27 Y: 22 | 10
0442 | 1: Sword 1 | X: 02 Y: 02 | 10
1622 | 5: Shield  | X: 02 Y: 17 | 10
1b9e | 6: Crown 2 | X: 30 Y: 28 | 10
1c40 | 7: Crown 1 | X: 00 Y: 02 | 10
096f | 2: Sword 2 | X: 15 Y: 11 | 10
1a05 | 6: Crown 2 | X: 05 Y: 16 | 10
0688 | 1: Sword 1 | X: 08 Y: 20 | 10
0fa7 | 3: Sword 3 | X: 07 Y: 29 | 10
0e60 | 3: Sword 3 | X: 00 Y: 19 | 10
0f66 | 3: Sword 3 | X: 06 Y: 27 | 10
0e87 | 3: Sword 3 | X: 07 Y: 20 | 10
0f60 | 3: Sword 3 | X: 00 Y: 27 | 10
0f47 | 3: Sword 3 | X: 07 Y: 26 | 10
0f89 | 3: Sword 3 | X: 09 Y: 28 | 10
06d1 | 1: Sword 1 | X: 17 Y: 22 | 10
073d | 1: Sword 1 | X: 29 Y: 25 | 10
0a36 | 2: Sword 2 | X: 22 Y: 17 | 10
031b | 0: Forest  | X: 27 Y: 24 | 10
011b | 0: Forest  | X: 27 Y: 08 | 10
17db | 5: Shield  | X: 27 Y: 30 | 10
07db | 1: Sword 1 | X: 27 Y: 30 | 10
03bf | 0: Forest  | X: 31 Y: 29 | 10
0319 | 0: Forest  | X: 25 Y: 24 | 17
0038 | 0: Forest  | X: 24 Y: 01 | 17
01d8 | 0: Forest  | X: 24 Y: 14 | 17
0258 | 0: Forest  | X: 24 Y: 18 | 17
0315 | 0: Forest  | X: 21 Y: 24 | 17
032b | 0: Forest  | X: 11 Y: 25 | 17
000f | 0: Forest  | X: 15 Y: 00 | 02
02d7 | 0: Forest  | X: 23 Y: 22 | 0a
0443 | 1: Sword 1 | X: 03 Y: 02 | 1c
0445 | 1: Sword 1 | X: 05 Y: 02 | 1c
04ad | 1: Sword 1 | X: 13 Y: 05 | 1b
05d3 | 1: Sword 1 | X: 19 Y: 14 | 11
05e4 | 1: Sword 1 | X: 04 Y: 15 | 1b
04b2 | 1: Sword 1 | X: 18 Y: 05 | 12
04b4 | 1: Sword 1 | X: 20 Y: 05 | 12
04a6 | 1: Sword 1 | X: 06 Y: 05 | 04
04a8 | 1: Sword 1 | X: 08 Y: 05 | 0b
042f | 1: Sword 1 | X: 15 Y: 01 | 0c
04aa | 1: Sword 1 | X: 10 Y: 05 | 0b
04b5 | 1: Sword 1 | X: 21 Y: 05 | 04
0507 | 1: Sword 1 | X: 07 Y: 08 | 04
0505 | 1: Sword 1 | X: 05 Y: 08 | 04
0584 | 1: Sword 1 | X: 04 Y: 12 | 04
05c4 | 1: Sword 1 | X: 04 Y: 14 | 04
050f | 1: Sword 1 | X: 15 Y: 08 | 04
0546 | 1: Sword 1 | X: 06 Y: 10 | 00
0586 | 1: Sword 1 | X: 06 Y: 12 | 00
05c6 | 1: Sword 1 | X: 06 Y: 14 | 00
0603 | 1: Sword 1 | X: 03 Y: 16 | 00
0628 | 1: Sword 1 | X: 08 Y: 17 | 00
062a | 1: Sword 1 | X: 10 Y: 17 | 00
062c | 1: Sword 1 | X: 12 Y: 17 | 00
062e | 1: Sword 1 | X: 14 Y: 17 | 00
050a | 1: Sword 1 | X: 10 Y: 08 | 04
0529 | 1: Sword 1 | X: 09 Y: 09 | 04
05e9 | 1: Sword 1 | X: 09 Y: 15 | 04
0567 | 1: Sword 1 | X: 07 Y: 11 | 04
057e | 1: Sword 1 | X: 30 Y: 11 | 04
05bd | 1: Sword 1 | X: 29 Y: 13 | 04
059a | 1: Sword 1 | X: 26 Y: 12 | 04
059f | 1: Sword 1 | X: 31 Y: 12 | 04
05d9 | 1: Sword 1 | X: 25 Y: 14 | 04
05fb | 1: Sword 1 | X: 27 Y: 15 | 04
05fe | 1: Sword 1 | X: 30 Y: 15 | 04
063d | 1: Sword 1 | X: 29 Y: 17 | 04
063a | 1: Sword 1 | X: 26 Y: 17 | 04
0515 | 1: Sword 1 | X: 21 Y: 08 | 0c
0551 | 1: Sword 1 | X: 17 Y: 10 | 0c
04ef | 1: Sword 1 | X: 15 Y: 07 | 0a
052f | 1: Sword 1 | X: 15 Y: 09 | 0a
056f | 1: Sword 1 | X: 15 Y: 11 | 0a
05b1 | 1: Sword 1 | X: 17 Y: 13 | 08
05af | 1: Sword 1 | X: 15 Y: 13 | 0a
0a0d | 2: Sword 2 | X: 13 Y: 16 | 04
0a32 | 2: Sword 2 | X: 18 Y: 17 | 00
089e | 2: Sword 2 | X: 30 Y: 04 | 0a
08d1 | 2: Sword 2 | X: 17 Y: 06 | 0e
08fc | 2: Sword 2 | X: 28 Y: 07 | 04
0999 | 2: Sword 2 | X: 25 Y: 12 | 04
09fd | 2: Sword 2 | X: 29 Y: 15 | 04
0855 | 2: Sword 2 | X: 21 Y: 02 | 0c
0894 | 2: Sword 2 | X: 20 Y: 04 | 0d
09cf | 2: Sword 2 | X: 15 Y: 14 | 0b
09cb | 2: Sword 2 | X: 11 Y: 14 | 0b
0802 | 2: Sword 2 | X: 02 Y: 00 | 00
0843 | 2: Sword 2 | X: 03 Y: 02 | 0b
0803 | 2: Sword 2 | X: 03 Y: 00 | 00
08cd | 2: Sword 2 | X: 13 Y: 06 | 04
0909 | 2: Sword 2 | X: 09 Y: 08 | 04
08e5 | 2: Sword 2 | X: 05 Y: 07 | 04
0dcc | 3: Sword 3 | X: 12 Y: 14 | 0c
0c72 | 3: Sword 3 | X: 18 Y: 03 | 04
0e26 | 3: Sword 3 | X: 06 Y: 17 | 04
0d8b | 3: Sword 3 | X: 11 Y: 12 | 04
0e11 | 3: Sword 3 | X: 17 Y: 16 | 0d
0d9f | 3: Sword 3 | X: 31 Y: 12 | 02
1404 | 5: Shield  | X: 04 Y: 00 | 17
146f | 5: Shield  | X: 15 Y: 03 | 17
143a | 5: Shield  | X: 26 Y: 01 | 17
1674 | 5: Shield  | X: 20 Y: 19 | 17
16d4 | 5: Shield  | X: 20 Y: 22 | 17
14a8 | 5: Shield  | X: 08 Y: 05 | 00
14a0 | 5: Shield  | X: 00 Y: 05 | 02
1429 | 5: Shield  | X: 09 Y: 01 | 0a
142c | 5: Shield  | X: 12 Y: 01 | 0a
1459 | 5: Shield  | X: 25 Y: 02 | 0a
152e | 5: Shield  | X: 14 Y: 09 | 02
152b | 5: Shield  | X: 11 Y: 09 | 0b
1505 | 5: Shield  | X: 05 Y: 08 | 04
15c7 | 5: Shield  | X: 07 Y: 14 | 0a
1594 | 5: Shield  | X: 20 Y: 12 | 06
151d | 5: Shield  | X: 29 Y: 08 | 04
1558 | 5: Shield  | X: 24 Y: 10 | 04
147d | 5: Shield  | X: 29 Y: 03 | 0c
15fe | 5: Shield  | X: 30 Y: 15 | 04
1618 | 5: Shield  | X: 24 Y: 16 | 0a
1104 | 4: Cup     | X: 04 Y: 08 | 08
10f2 | 4: Cup     | X: 18 Y: 07 | 00
10f3 | 4: Cup     | X: 19 Y: 07 | 00
10f4 | 4: Cup     | X: 20 Y: 07 | 00
10f5 | 4: Cup     | X: 21 Y: 07 | 0b
1119 | 4: Cup     | X: 25 Y: 08 | 0b
111a | 4: Cup     | X: 26 Y: 08 | 0b
111c | 4: Cup     | X: 28 Y: 08 | 0b
115e | 4: Cup     | X: 30 Y: 10 | 0b
1294 | 4: Cup     | X: 20 Y: 20 | 0d
1313 | 4: Cup     | X: 19 Y: 24 | 0c
13b3 | 4: Cup     | X: 19 Y: 29 | 0c
12e7 | 4: Cup     | X: 07 Y: 23 | 0b
1327 | 4: Cup     | X: 07 Y: 25 | 0b
1272 | 4: Cup     | X: 18 Y: 19 | 08
1346 | 4: Cup     | X: 06 Y: 26 | 02
1324 | 4: Cup     | X: 04 Y: 25 | 06
1322 | 4: Cup     | X: 02 Y: 25 | 06
1384 | 4: Cup     | X: 04 Y: 28 | 06
12c1 | 4: Cup     | X: 01 Y: 22 | 02
12e2 | 4: Cup     | X: 02 Y: 23 | 02
1382 | 4: Cup     | X: 02 Y: 28 | 04
13a1 | 4: Cup     | X: 01 Y: 29 | 02
09c8 | 2: Sword 2 | X: 08 Y: 14 | 1d
0a08 | 2: Sword 2 | X: 08 Y: 16 | 1d
0810 | 2: Sword 2 | X: 16 Y: 00 | 1d
08c1 | 2: Sword 2 | X: 01 Y: 06 | 14
09d5 | 2: Sword 2 | X: 21 Y: 14 | 18
0993 | 2: Sword 2 | X: 19 Y: 12 | 18
09bc | 2: Sword 2 | X: 28 Y: 13 | 15
0ab8 | 2: Sword 2 | X: 24 Y: 21 | 1d
098f | 2: Sword 2 | X: 15 Y: 12 | 1a
0983 | 2: Sword 2 | X: 03 Y: 12 | 1c
08cf | 2: Sword 2 | X: 15 Y: 06 | 15
0002 | 0: Forest  | X: 02 Y: 00 | 04
1c29 | 7: Crown 1 | X: 09 Y: 01 | 06
1cfb | 7: Crown 1 | X: 27 Y: 07 | 08
1d9d | 7: Crown 1 | X: 29 Y: 12 | 04
1694 | 5: Shield  | X: 20 Y: 20 | 06
1665 | 5: Shield  | X: 05 Y: 19 | 17
1666 | 5: Shield  | X: 06 Y: 19 | 17
1668 | 5: Shield  | X: 08 Y: 19 | 17
1669 | 5: Shield  | X: 09 Y: 19 | 17
166a | 5: Shield  | X: 10 Y: 19 | 17
166b | 5: Shield  | X: 11 Y: 19 | 17
1d59 | 7: Crown 1 | X: 25 Y: 10 | 0d
1d38 | 7: Crown 1 | X: 24 Y: 09 | 02
1e2f | 7: Crown 1 | X: 15 Y: 17 | 06
1e50 | 7: Crown 1 | X: 16 Y: 18 | 06
1e51 | 7: Crown 1 | X: 17 Y: 18 | 06
1e1a | 7: Crown 1 | X: 26 Y: 16 | 04
1f1f | 7: Crown 1 | X: 31 Y: 24 | 04
1daa | 7: Crown 1 | X: 10 Y: 13 | 0e
1f6a | 7: Crown 1 | X: 10 Y: 27 | 04
1f30 | 7: Crown 1 | X: 16 Y: 25 | 04
1d68 | 7: Crown 1 | X: 08 Y: 11 | 0d
1dc6 | 7: Crown 1 | X: 06 Y: 14 | 0d
1d27 | 7: Crown 1 | X: 07 Y: 09 | 0f
1b78 | 6: Crown 2 | X: 24 Y: 27 | 0c
1a5c | 6: Crown 2 | X: 28 Y: 18 | 0c
1a3b | 6: Crown 2 | X: 27 Y: 17 | 04
1a53 | 6: Crown 2 | X: 19 Y: 18 | 04
188d | 6: Crown 2 | X: 13 Y: 04 | 04
190c | 6: Crown 2 | X: 12 Y: 08 | 0c
18b5 | 6: Crown 2 | X: 21 Y: 05 | 0c
1a11 | 6: Crown 2 | X: 17 Y: 16 | 08
19be | 6: Crown 2 | X: 30 Y: 13 | 08
1926 | 6: Crown 2 | X: 06 Y: 09 | 0e
1d64 | 7: Crown 1 | X: 04 Y: 11 | 04
0660 | 1: Sword 1 | X: 00 Y: 19 | 04
0665 | 1: Sword 1 | X: 05 Y: 19 | 04
0707 | 1: Sword 1 | X: 07 Y: 24 | 04
0720 | 1: Sword 1 | X: 00 Y: 25 | 04
0661 | 1: Sword 1 | X: 01 Y: 19 | 04
06e0 | 1: Sword 1 | X: 00 Y: 23 | 04
0724 | 1: Sword 1 | X: 04 Y: 25 | 04
06a6 | 1: Sword 1 | X: 06 Y: 21 | 04
0fc7 | 3: Sword 3 | X: 07 Y: 30 | 0b
0c1e | 3: Sword 3 | X: 30 Y: 00 | 00
0cd9 | 3: Sword 3 | X: 25 Y: 06 | 00
0cf4 | 3: Sword 3 | X: 20 Y: 07 | 04
0cfa | 3: Sword 3 | X: 26 Y: 07 | 0a
06f3 | 1: Sword 1 | X: 19 Y: 23 | 0a
06f5 | 1: Sword 1 | X: 21 Y: 23 | 0b
0699 | 1: Sword 1 | X: 25 Y: 20 | 0c
073e | 1: Sword 1 | X: 30 Y: 25 | 0d
06fd | 1: Sword 1 | X: 29 Y: 23 | 0a
06f8 | 1: Sword 1 | X: 24 Y: 23 | 0a
073b | 1: Sword 1 | X: 27 Y: 25 | 0d
0638 | 1: Sword 1 | X: 24 Y: 17 | 04
063c | 1: Sword 1 | X: 28 Y: 17 | 04
069f | 1: Sword 1 | X: 31 Y: 20 | 04
06fc | 1: Sword 1 | X: 28 Y: 23 | 04
066f | 1: Sword 1 | X: 15 Y: 19 | 02
068f | 1: Sword 1 | X: 15 Y: 20 | 02
06af | 1: Sword 1 | X: 15 Y: 21 | 02
06cf | 1: Sword 1 | X: 15 Y: 22 | 02
071f | 1: Sword 1 | X: 31 Y: 24 | 04
077c | 1: Sword 1 | X: 28 Y: 27 | 02
075b | 1: Sword 1 | X: 27 Y: 26 | 02
0bc3 | 2: Sword 2 | X: 03 Y: 30 | 02
0ae8 | 2: Sword 2 | X: 08 Y: 23 | 04
0acb | 2: Sword 2 | X: 11 Y: 22 | 04
0a45 | 2: Sword 2 | X: 05 Y: 18 | 02
0add | 2: Sword 2 | X: 29 Y: 22 | 02
0b10 | 2: Sword 2 | X: 16 Y: 24 | 0a
0a6f | 2: Sword 2 | X: 15 Y: 19 | 0b
0a5a | 2: Sword 2 | X: 26 Y: 18 | 00
0a74 | 2: Sword 2 | X: 20 Y: 19 | 04
0a94 | 2: Sword 2 | X: 20 Y: 20 | 04
0a54 | 2: Sword 2 | X: 20 Y: 18 | 04
0a33 | 2: Sword 2 | X: 19 Y: 17 | 04
0a52 | 2: Sword 2 | X: 18 Y: 18 | 04
0a92 | 2: Sword 2 | X: 18 Y: 20 | 04
0a71 | 2: Sword 2 | X: 17 Y: 19 | 04
0a90 | 2: Sword 2 | X: 16 Y: 20 | 04
0aba | 2: Sword 2 | X: 26 Y: 21 | 0c
0a8a | 2: Sword 2 | X: 10 Y: 20 | 00
029b | 0: Forest  | X: 27 Y: 20 | 04
1a44 | 6: Crown 2 | X: 04 Y: 18 | 0a
17d8 | 5: Shield  | X: 24 Y: 30 | 06
17f3 | 5: Shield  | X: 19 Y: 31 | 06
17c7 | 5: Shield  | X: 07 Y: 30 | 06
17e1 | 5: Shield  | X: 01 Y: 31 | 06
16c4 | 5: Shield  | X: 04 Y: 22 | 06
07b1 | 1: Sword 1 | X: 17 Y: 29 | 0b
07ee | 1: Sword 1 | X: 14 Y: 31 | 06
07c7 | 1: Sword 1 | X: 07 Y: 30 | 08
0a9e | 2: Sword 2 | X: 30 Y: 20 | 06
0bca | 2: Sword 2 | X: 10 Y: 30 | 0f
1025 | 4: Cup     | X: 05 Y: 01 | 12
111b | 4: Cup     | X: 27 Y: 08 | 14
11b4 | 4: Cup     | X: 20 Y: 13 | 13
12f4 | 4: Cup     | X: 20 Y: 23 | 1a
1386 | 4: Cup     | X: 06 Y: 28 | 1d
13c6 | 4: Cup     | X: 06 Y: 30 | 1d
11c5 | 4: Cup     | X: 05 Y: 14 | 18
1310 | 4: Cup     | X: 16 Y: 24 | 1c
1223 | 4: Cup     | X: 03 Y: 17 | 1b
15a4 | 5: Shield  | X: 04 Y: 13 | 17
17a1 | 5: Shield  | X: 01 Y: 29 | 17
1865 | 6: Crown 2 | X: 05 Y: 03 | 16
18f7 | 6: Crown 2 | X: 23 Y: 07 | 15
19c9 | 6: Crown 2 | X: 09 Y: 14 | 1d
1a1e | 6: Crown 2 | X: 30 Y: 16 | 19
19b7 | 6: Crown 2 | X: 23 Y: 13 | 18
1d4c | 7: Crown 1 | X: 12 Y: 10 | 12
0000 |            |             | 00

### $30ae: ? (379 x 4)

 ? | ?  | ?  | ?
---|----|----|----
00 | 06 | 00 | ee
00 | 34 | 00 | 06
00 | f3 | 00 | 34
00 | 06 | 01 | 85
00 | 34 | 00 | 06
01 | 8e | 00 | 34
00 | 06 | 02 | 31
00 | 34 | 00 | 06
04 | 32 | 00 | 80
00 | 06 | 04 | 96
00 | 7f | 00 | 08
05 | 48 | 01 | 00
04 | 00 | 00 | 08
05 | 4a | 01 | 00
05 | 00 | 00 | 08
05 | 4c | 01 | 00
06 | 00 | 00 | 08
05 | 4e | 01 | 00
07 | 00 | 00 | 08
05 | 88 | 01 | 01
04 | 00 | 00 | 08
05 | 8a | 01 | 01
05 | 00 | 00 | 08
05 | 8c | 01 | 01
06 | 00 | 00 | 08
05 | 8e | 01 | 01
07 | 00 | 00 | 08
05 | c8 | 01 | 02
04 | 00 | 00 | 08
05 | ca | 01 | 02
05 | 00 | 00 | 08
05 | cc | 01 | 02
06 | 00 | 00 | 08
05 | ce | 01 | 02
07 | 00 | 00 | 08
06 | 08 | 01 | 03
04 | 00 | 00 | 08
06 | 0a | 01 | 03
05 | 00 | 00 | 08
06 | 0c | 01 | 03
06 | 00 | 00 | 08
06 | 0e | 01 | 03
07 | 00 | 00 | 0a
05 | 34 | 83 | 08
09 | 0a | 0b | 00
00 | 06 | 09 | e4
90 | 1b | 00 | 06
09 | f0 | 00 | 0c
00 | 06 | 09 | f2
00 | 0d | 00 | 0e
08 | 92 | 88 | 16
0e | 0f | 10 | 11
12 | 13 | 14 | 15
00 | 06 | 08 | 62
80 | 17 | 00 | 06
08 | a2 | 80 | 17
00 | 06 | 08 | e2
80 | 17 | 00 | 06
09 | 22 | 80 | 17
00 | 06 | 09 | 62
80 | 17 | 00 | 06
09 | a2 | 80 | 17
00 | 06 | 09 | 04
40 | 18 | 00 | 06
0d | 92 | 00 | 9a
00 | 06 | 0d | ce
00 | 1a | 00 | 06
0d | ae | 00 | 1a
00 | 06 | 0d | 8f
00 | 1a | 00 | 06
0d | b0 | 00 | 1a
00 | 06 | 0d | be
00 | 82 | 00 | 06
14 | 0f | 90 | 1c
00 | 06 | 14 | ce
00 | 1d | 00 | 08
15 | ad | 02 | 1e
1f | 20 | 00 | 08
15 | b0 | 02 | 21
22 | 23 | 00 | 06
17 | be | 00 | 81
00 | 06 | 10 | ee
80 | 25 | 00 | 06
11 | 0f | 80 | 24
00 | 06 | 11 | 2e
80 | 26 | 00 | 06
11 | 20 | 00 | 36
00 | 06 | 11 | 46
80 | 2b | 00 | 06
10 | 86 | 00 | 2c
00 | 06 | 10 | 87
00 | 2c | 00 | 06
10 | 88 | 00 | 2d
00 | 06 | 10 | a8
00 | 2d | 00 | 06
10 | a7 | 00 | 2e
00 | 06 | 10 | c8
00 | 2d | 00 | 06
11 | c1 | 80 | 27
00 | 08 | 11 | 32
82 | 28 | 29 | 2a
00 | 06 | 11 | 52
80 | 28 | 00 | 06
11 | 72 | 80 | 28
00 | 06 | 11 | 33
80 | 29 | 00 | 06
11 | 53 | 80 | 29
00 | 08 | 11 | 73
82 | 29 | 2a | 28
00 | 06 | 11 | 34
80 | 2a | 00 | 08
11 | 54 | 82 | 2a
28 | 29 | 00 | 06
11 | 74 | 80 | 2a
00 | 06 | 11 | 7f
00 | 84 | 00 | 06
12 | b3 | 40 | 85
00 | 06 | 12 | f7
00 | 86 | 00 | 06
13 | 3a | 00 | 87
00 | 06 | 13 | 39
00 | 89 | 00 | 06
13 | 7d | 00 | 86
00 | 06 | 13 | bb
00 | 89 | 00 | 06
13 | ba | 00 | 88
00 | 06 | 13 | f9
00 | 86 | 00 | 06
13 | 57 | 00 | 87
00 | 08 | 13 | 7b
01 | 8a | 8b | 00
00 | 06 | 12 | ea
00 | 8c | 00 | 06
13 | 2a | 00 | 8d
00 | 06 | 12 | 4c
00 | 8e | 00 | 06
12 | 4a | 00 | 8f
00 | 06 | 12 | 8b
00 | 90 | 00 | 06
12 | cc | 00 | 91
00 | 06 | 12 | aa
00 | 92 | 00 | 06
12 | ed | 00 | 93
00 | 06 | 13 | a2
00 | 83 | 00 | 0a
1c | 69 | 03 | 94
95 | 96 | 97 | 00
00 | 06 | 04 | 41
80 | 48 | 00 | 06
04 | 61 | 80 | 49
00 | 06 | 04 | 81
80 | 4a | 00 | 06
04 | a1 | 80 | 4b
00 | 06 | 04 | c1
80 | 4c | 00 | 06
04 | e1 | 80 | 4d
00 | 06 | 05 | 01
80 | 4d | 00 | 06
05 | 21 | 80 | 4d
00 | 06 | 05 | 41
80 | 4d | 00 | 08
05 | 61 | c0 | 4e
44 | 00 | 00 | 06
1c | cc | 00 | 98
00 | 0a | 1c | 7b
04 | 99 | 9b | 9c
9d | 9e | 00 | 08
1d | 19 | 01 | 9f
a0 | 00 | 00 | 06
16 | 45 | 80 | a1
00 | 06 | 16 | 46
80 | a1 | 00 | 06
16 | 47 | 80 | a1
00 | 06 | 16 | 48
80 | a1 | 00 | 06
16 | 49 | 80 | a1
00 | 06 | 16 | 4b
80 | a1 | 00 | 06
16 | 4a | 80 | a2
00 | 08 | 16 | 44
01 | a3 | a8 | 00
00 | 0a | 16 | ac
03 | a4 | a5 | a6
a7 | 00 | 00 | 08
1d | 7a | 01 | a9
aa | 00 | 00 | 06
1e | 72 | 00 | ab
00 | 08 | 1e | f7
81 | ac | ad | 00
00 | 06 | 1e | ff
80 | ae | 00 | 06
1f | 3f | 80 | af
00 | 06 | 1e | f5
00 | ab | 00 | 08
1f | ce | 02 | b0
b1 | b2 | 00 | 08
1f | d2 | 02 | b3
b4 | b5 | 00 | 08
1f | d6 | 02 | b6
b7 | b8 | 00 | 08
1f | da | 02 | b9
ba | bb | 00 | 06
12 | 42 | 00 | 4f
00 | 06 | 12 | 62
40 | 50 | 00 | 06
12 | 82 | 40 | 51
00 | 06 | 12 | 63
40 | 52 | 00 | 06
12 | 02 | 00 | 5f
00 | 06 | 12 | 00
00 | 5f | 00 | 0a
11 | e7 | 03 | 57
5c | 5d | 5e | 00
00 | 0a | 12 | 27
04 | 58 | 53 | 54
55 | 56 | 00 | 0a
12 | c7 | 03 | 59
5a | 5b | 57 | 00
00 | 08 | 1e | 27
01 | bc | bd | 00
00 | 08 | 1f | e9
01 | be | bf | 00
00 | 08 | 1e | 68
c0 | bd | 25 | 00
00 | 08 | 1e | 66
c0 | bd | 25 | 00
00 | 08 | 1e | 64
c0 | bd | 25 | 00
00 | 08 | 1e | 62
c0 | bd | 25 | 00
00 | 08 | 1e | 60
c0 | bd | 25 | 00
00 | 08 | 1e | a2
c0 | bd | 25 | 00
00 | 08 | 1e | a6
c0 | bd | 25 | 00
00 | 08 | 1e | e4
c0 | bd | 25 | 00
00 | 08 | 1e | a4
c0 | c0 | 47 | 00
00 | 08 | 1f | 61
01 | c1 | c2 | 00
00 | 08 | 1f | 63
01 | c1 | c2 | 00
00 | 0a | 1b | bb
03 | c3 | c4 | c6
c7 | 00 | 00 | 08
1b | 3d | 81 | c8
c9 | 00 | 00 | 08
1a | 78 | 01 | ca
cb | 00 | 00 | 08
1b | 9f | 41 | ca
cb | 00 | 00 | 06
1a | 72 | 00 | c5
00 | 08 | 1a | 12
02 | ce | cf | d0
00 | 08 | 18 | 29
01 | d1 | d2 | 00
00 | 08 | 18 | 85
01 | d3 | d4 | 00
00 | 06 | 18 | 92
00 | d5 | 00 | 0a
18 | bc | 03 | d6
d7 | d8 | d9 | 00
00 | 08 | 19 | 1d
01 | da | db | 00
00 | 08 | 19 | e4
c0 | e4 | 43 | 00
00 | 08 | 19 | e5
c0 | e5 | 43 | 00
00 | 08 | 19 | e6
c0 | e6 | 43 | 00
00 | 08 | 06 | 6d
c1 | 69 | 60 | 5b
00 | 08 | 06 | ed
c1 | 69 | 61 | 5b
00 | 08 | 06 | e9
c1 | 69 | 62 | 5b
00 | 08 | 06 | 69
c1 | 69 | 63 | 5b
00 | 06 | 06 | a4
80 | 67 | 00 | 06
06 | e4 | 80 | 66
00 | 06 | 06 | e2
80 | 65 | 00 | 06
06 | a2 | 80 | 64
00 | 06 | 06 | ae
00 | 68 | 00 | 08
0e | c3 | 01 | 6a
6b | 00 | 00 | 08
0e | e2 | 01 | 6a
6d | 00 | 00 | 08
0f | 03 | 01 | 6c
6d | 00 | 00 | 08
0e | e4 | 01 | 6b
6c | 00 | 00 | 12
0e | e3 | 0b | 76
77 | 78 | 71 | 72
73 | 74 | 75 | 6a
6b | 6c | 6d | 00
00 | 0a | 0f | 46
04 | 72 | 79 | 70
7c | 75 | 00 | 0a
0f | 40 | 03 | 7b
7a | 71 | 75 | 00
00 | 08 | 0e | 80
02 | 6e | 7a | 73
00 | 0c | 0e | 86
05 | 72 | 74 | 73
79 | 6f | 7d | 00
00 | 06 | 0e | 3e
00 | 7e | 00 | 06
0f | c8 | 00 | cc
00 | 06 | 0f | e9
00 | cc | 00 | 08
0f | ab | 01 | dc
df | 00 | 00 | 08
0f | cb | 01 | dd
df | 00 | 00 | 08
0f | eb | 01 | de
df | 00 | 00 | 08
0f | d3 | 01 | e0
e1 | 00 | 00 | 06
0f | 70 | 00 | e2
00 | 06 | 0e | ea
00 | cd | 00 | 06
0d | 33 | 00 | 19
00 | 08 | 0c | 19
02 | 2f | 30 | 31
00 | 06 | 0c | b8
00 | 32 | 00 | 06
0c | 3f | 80 | 33
00 | 08 | 0d | 1f
01 | 35 | 37 | 00
00 | 06 | 06 | 5e
00 | 38 | 00 | 08
06 | 5a | 81 | 3b
3c | 00 | 00 | 0a
06 | 7c | 03 | 3d
3e | 3f | 40 | 00
00 | 0a | 06 | 9d
03 | 3f | 40 | 3a
39 | 00 | 00 | 06
0a | fb | 00 | e3
00 | 06 | 0a | 84
80 | 43 | 00 | 08
0b | 05 | 01 | 41
42 | 00 | 00 | 06
0a | f9 | 00 | 44
00 | 06 | 0a | 5c
00 | 45 | 00 | 06
0a | 5e | 00 | 45
00 | 06 | 0a | 7f
00 | 45 | 00 | 08
0a | 8c | 01 | 46
47 | 00 | 00 | 08
17 | fa | 01 | e7
e8 | 00 | 00 | 06
16 | ed | 40 | e9
00 | 06 | 1a | 0c
00 | ea | 00 | 06
1a | 8a | 00 | ed
00 | 06 | 1a | cf
00 | eb | 00 | 06
1b | 51 | 00 | ec
00 | 06 | 07 | 7a
00 | ee | 00 | 06
07 | 70 | 00 | ef
00 | 06 | 07 | e6
00 | f0 | 00 | 06
07 | e4 | 00 | f1
00 | 06 | 07 | e1
00 | f2 | 00 | 06
07 | a6 | 00 | f3
00 | 08 | 03 | be
c0 | f5 | 08 | 00
00 | 06 | 03 | ff
00 | f4 | 00 | 00
00 | 00 | 00 | 00

### $369a: Traps

A series of four-byte entries for things like fireball traps. Two bytes for the
map address, and two more bytes to define the trap.

Addr | Map        | Coords      | ?  | ?
-----|------------|-------------|----|----
0546 | 1: Sword 1 | X: 06 Y: 10 | 07 | c0 
0586 | 1: Sword 1 | X: 06 Y: 12 | 07 | c0 
05c6 | 1: Sword 1 | X: 06 Y: 14 | 07 | c0 
0603 | 1: Sword 1 | X: 03 Y: 16 | 07 | c0 
0628 | 1: Sword 1 | X: 08 Y: 17 | 07 | 00 
062a | 1: Sword 1 | X: 10 Y: 17 | 07 | 00 
062c | 1: Sword 1 | X: 12 Y: 17 | 07 | 00 
062e | 1: Sword 1 | X: 14 Y: 17 | 07 | 00 
0554 | 1: Sword 1 | X: 20 Y: 10 | 83 | 00 
0575 | 1: Sword 1 | X: 21 Y: 11 | 83 | 00 
0576 | 1: Sword 1 | X: 22 Y: 11 | 83 | 00 
0557 | 1: Sword 1 | X: 23 Y: 10 | 83 | 00 
0a10 | 2: Sword 2 | X: 16 Y: 16 | 84 | 00 
0a32 | 2: Sword 2 | X: 18 Y: 17 | 07 | 00 
0915 | 2: Sword 2 | X: 21 Y: 08 | 83 | 00 
0914 | 2: Sword 2 | X: 20 Y: 08 | 83 | 00 
08f3 | 2: Sword 2 | X: 19 Y: 07 | 83 | 00 
0956 | 2: Sword 2 | X: 22 Y: 10 | 83 | 00 
0955 | 2: Sword 2 | X: 21 Y: 10 | 83 | 00 
0954 | 2: Sword 2 | X: 20 Y: 10 | 83 | 00 
0953 | 2: Sword 2 | X: 19 Y: 10 | 83 | 00 
0932 | 2: Sword 2 | X: 18 Y: 09 | 83 | 00 
0916 | 2: Sword 2 | X: 22 Y: 08 | 83 | 00 
0802 | 2: Sword 2 | X: 02 Y: 00 | 07 | 80 
0803 | 2: Sword 2 | X: 03 Y: 00 | 07 | c0 
0c5f | 3: Sword 3 | X: 31 Y: 02 | 83 | 00 
0d91 | 3: Sword 3 | X: 17 Y: 12 | 02 | 00 
09e3 | 2: Sword 2 | X: 03 Y: 15 | 0a | 20 
144f | 5: Shield  | X: 15 Y: 02 | 0a | 20 
1532 | 5: Shield  | X: 18 Y: 09 | 01 | 00 
158e | 5: Shield  | X: 14 Y: 12 | 83 | 00 
15ae | 5: Shield  | X: 14 Y: 13 | 83 | 00 
15ce | 5: Shield  | X: 14 Y: 14 | 83 | 00 
1591 | 5: Shield  | X: 17 Y: 12 | 83 | 00 
15b1 | 5: Shield  | X: 17 Y: 13 | 83 | 00 
15d1 | 5: Shield  | X: 17 Y: 14 | 83 | 00 
116f | 4: Cup     | X: 15 Y: 11 | 83 | 00 
112b | 4: Cup     | X: 11 Y: 09 | 83 | 00 
112a | 4: Cup     | X: 10 Y: 09 | 83 | 00 
1160 | 4: Cup     | X: 00 Y: 11 | 01 | 00 
10f2 | 4: Cup     | X: 18 Y: 07 | 07 | 80 
10f3 | 4: Cup     | X: 19 Y: 07 | 07 | 80 
10f4 | 4: Cup     | X: 20 Y: 07 | 07 | 80 
10a6 | 4: Cup     | X: 06 Y: 05 | 0a | 20 
10e8 | 4: Cup     | X: 08 Y: 07 | 0a | 20 
1108 | 4: Cup     | X: 08 Y: 08 | 0a | 20 
1106 | 4: Cup     | X: 06 Y: 08 | 0a | 20 
0c1e | 3: Sword 3 | X: 30 Y: 00 | 07 | 40 
0cd9 | 3: Sword 3 | X: 25 Y: 06 | 07 | 00 
0c18 | 3: Sword 3 | X: 24 Y: 00 | 01 | 00 
0cf8 | 3: Sword 3 | X: 24 Y: 07 | 84 | 00 
0c7b | 3: Sword 3 | X: 27 Y: 03 | 0b | 00 
0129 | 0: Forest  | X: 09 Y: 09 | 0b | 01 
0cb7 | 3: Sword 3 | X: 23 Y: 05 | 03 | 00 
1160 | 4: Cup     | X: 00 Y: 11 | 02 | 00 
0cd6 | 3: Sword 3 | X: 22 Y: 06 | 01 | 00 
06de | 1: Sword 1 | X: 30 Y: 22 | 0a | 20 
06bd | 1: Sword 1 | X: 29 Y: 21 | 0b | 02 
069e | 1: Sword 1 | X: 30 Y: 20 | 0b | 02 
065b | 1: Sword 1 | X: 27 Y: 18 | 0a | 20 
065d | 1: Sword 1 | X: 29 Y: 18 | 0a | 30 
065c | 1: Sword 1 | X: 28 Y: 18 | 0b | 02 
067b | 1: Sword 1 | X: 27 Y: 19 | 0b | 02 
067d | 1: Sword 1 | X: 29 Y: 19 | 0b | 02 
069c | 1: Sword 1 | X: 28 Y: 20 | 0b | 02 
0a83 | 2: Sword 2 | X: 03 Y: 20 | 03 | 00 
0aa0 | 2: Sword 2 | X: 00 Y: 21 | 03 | 00 
0b06 | 2: Sword 2 | X: 06 Y: 24 | 03 | 00 
0b19 | 2: Sword 2 | X: 25 Y: 24 | 84 | 00 
0a5a | 2: Sword 2 | X: 26 Y: 18 | 07 | c0 
0a8a | 2: Sword 2 | X: 10 Y: 20 | 07 | c0 
0a8e | 2: Sword 2 | X: 14 Y: 20 | 01 | 00 
0461 | 1: Sword 1 | X: 01 Y: 03 | 0d | 00 
0481 | 1: Sword 1 | X: 01 Y: 04 | 0d | 00 
04a1 | 1: Sword 1 | X: 01 Y: 05 | 0d | 00 
04c1 | 1: Sword 1 | X: 01 Y: 06 | 0d | 00 
04e1 | 1: Sword 1 | X: 01 Y: 07 | 0d | 00 
0481 | 1: Sword 1 | X: 01 Y: 04 | 0d | 00 
004d | 0: Forest  | X: 13 Y: 02 | 05 | 00 
1222 | 4: Cup     | X: 02 Y: 17 | 82 | 00 
12a2 | 4: Cup     | X: 02 Y: 21 | 81 | 00 
1264 | 4: Cup     | X: 04 Y: 19 | 81 | 00 
1261 | 4: Cup     | X: 01 Y: 19 | 81 | 00 
1247 | 4: Cup     | X: 07 Y: 18 | 0b | 03 
1267 | 4: Cup     | X: 07 Y: 19 | 0b | 03 
1287 | 4: Cup     | X: 07 Y: 20 | 0b | 03 
12a7 | 4: Cup     | X: 07 Y: 21 | 0b | 03 
1207 | 4: Cup     | X: 07 Y: 16 | 03 | 00 
1207 | 4: Cup     | X: 07 Y: 16 | 04 | 00 
1208 | 4: Cup     | X: 08 Y: 16 | 03 | 00 
1228 | 4: Cup     | X: 08 Y: 17 | 03 | 00 
1248 | 4: Cup     | X: 08 Y: 18 | 03 | 00 
1208 | 4: Cup     | X: 08 Y: 16 | 04 | 00 
1228 | 4: Cup     | X: 08 Y: 17 | 04 | 00 
1248 | 4: Cup     | X: 08 Y: 18 | 04 | 00 
1222 | 4: Cup     | X: 02 Y: 17 | 01 | 00 
066c | 1: Sword 1 | X: 12 Y: 19 | 81 | 00 
06cd | 1: Sword 1 | X: 13 Y: 22 | 81 | 00 
06ea | 1: Sword 1 | X: 10 Y: 23 | 81 | 00 
0689 | 1: Sword 1 | X: 09 Y: 20 | 81 | 00 
0663 | 1: Sword 1 | X: 03 Y: 19 | 0d | 00 
06e6 | 1: Sword 1 | X: 06 Y: 23 | 0d | 00 
0722 | 1: Sword 1 | X: 02 Y: 25 | 0d | 00 
06a0 | 1: Sword 1 | X: 00 Y: 21 | 0d | 00 
06eb | 1: Sword 1 | X: 11 Y: 23 | 83 | 00 
06ab | 1: Sword 1 | X: 11 Y: 21 | 0c | 00 
0ec2 | 3: Sword 3 | X: 02 Y: 22 | 0b | 04 
0ec4 | 3: Sword 3 | X: 04 Y: 22 | 0b | 04 
0f04 | 3: Sword 3 | X: 04 Y: 24 | 0b | 04 
0f02 | 3: Sword 3 | X: 02 Y: 24 | 0b | 04 
0ec8 | 3: Sword 3 | X: 08 Y: 22 | 03 | 00 
0ee8 | 3: Sword 3 | X: 08 Y: 23 | 03 | 00 
0ee7 | 3: Sword 3 | X: 07 Y: 23 | 03 | 00 
0ee6 | 3: Sword 3 | X: 06 Y: 23 | 03 | 00 
0e83 | 3: Sword 3 | X: 03 Y: 20 | 03 | 00 
0ee0 | 3: Sword 3 | X: 00 Y: 23 | 03 | 00 
0f42 | 3: Sword 3 | X: 02 Y: 26 | 03 | 00 
0f43 | 3: Sword 3 | X: 03 Y: 26 | 03 | 00 
0ec8 | 3: Sword 3 | X: 08 Y: 22 | 04 | 00 
0ee8 | 3: Sword 3 | X: 08 Y: 23 | 04 | 00 
0ee7 | 3: Sword 3 | X: 07 Y: 23 | 04 | 00 
0ee6 | 3: Sword 3 | X: 06 Y: 23 | 04 | 00 
0e83 | 3: Sword 3 | X: 03 Y: 20 | 04 | 00 
0ee0 | 3: Sword 3 | X: 00 Y: 23 | 04 | 00 
0f42 | 3: Sword 3 | X: 02 Y: 26 | 04 | 00 
0f43 | 3: Sword 3 | X: 03 Y: 26 | 04 | 00 
0eb2 | 3: Sword 3 | X: 18 Y: 21 | 83 | 00 
041c | 1: Sword 1 | X: 28 Y: 00 | 01 | 00 
0418 | 1: Sword 1 | X: 24 Y: 00 | 01 | 00 
0002 | 0: Forest  | X: 02 Y: 00 | 0f | 00 
0003 | 0: Forest  | X: 03 Y: 00 | 0f | 00 
0000 | 0: Forest  | X: 00 Y: 00 | 0f | 00 
1293 | 4: Cup     | X: 19 Y: 20 | 03 | 00 
11bc | 4: Cup     | X: 28 Y: 13 | 0b | 05 
12fe | 4: Cup     | X: 30 Y: 23 | 0d | 00 
137f | 4: Cup     | X: 31 Y: 27 | 0d | 00 
12fc | 4: Cup     | X: 28 Y: 23 | 0d | 00 
133c | 4: Cup     | X: 28 Y: 25 | 0d | 00 
1334 | 4: Cup     | X: 20 Y: 25 | 03 | 00 
13d4 | 4: Cup     | X: 20 Y: 30 | 03 | 00 
12e9 | 4: Cup     | X: 09 Y: 23 | 01 | 00 
1329 | 4: Cup     | X: 09 Y: 25 | 01 | 00 
124b | 4: Cup     | X: 11 Y: 18 | 0b | 05 
126a | 4: Cup     | X: 10 Y: 19 | 0b | 05 
128c | 4: Cup     | X: 12 Y: 20 | 0b | 05 
12cb | 4: Cup     | X: 11 Y: 22 | 0b | 05 
12ca | 4: Cup     | X: 10 Y: 22 | 0b | 05 
130e | 4: Cup     | X: 14 Y: 24 | 0b | 05 
1c03 | 7: Crown 1 | X: 03 Y: 00 | 0b | 06 
1c27 | 7: Crown 1 | X: 07 Y: 01 | 0b | 07 
1ca1 | 7: Crown 1 | X: 01 Y: 05 | 0b | 08 
1ca4 | 7: Crown 1 | X: 04 Y: 05 | 0b | 09 
1d11 | 7: Crown 1 | X: 17 Y: 08 | 0b | 0a 
1c3e | 7: Crown 1 | X: 30 Y: 01 | 0b | 0b 
0c01 | 3: Sword 3 | X: 01 Y: 00 | 03 | 00 
1c5e | 7: Crown 1 | X: 30 Y: 02 | 0b | 0b 
1c7e | 7: Crown 1 | X: 30 Y: 03 | 0b | 0b 
1c9e | 7: Crown 1 | X: 30 Y: 04 | 0b | 0b 
1cbe | 7: Crown 1 | X: 30 Y: 05 | 0b | 0b 
1cf9 | 7: Crown 1 | X: 25 Y: 07 | 03 | 00 
1cfa | 7: Crown 1 | X: 26 Y: 07 | 03 | 00 
15e2 | 5: Shield  | X: 02 Y: 15 | 0b | 0c 
160c | 5: Shield  | X: 12 Y: 16 | 03 | 00 
1ce7 | 7: Crown 1 | X: 07 Y: 07 | 03 | 00 
160f | 5: Shield  | X: 15 Y: 16 | 01 | 00 
164f | 5: Shield  | X: 15 Y: 18 | 01 | 00 
168f | 5: Shield  | X: 15 Y: 20 | 01 | 00 
16cf | 5: Shield  | X: 15 Y: 22 | 01 | 00 
1cd8 | 7: Crown 1 | X: 24 Y: 06 | 03 | 00 
1c47 | 7: Crown 1 | X: 07 Y: 02 | 01 | 00 
1c81 | 7: Crown 1 | X: 01 Y: 04 | 01 | 00 
1e58 | 7: Crown 1 | X: 24 Y: 18 | 0b | 0d 
1efd | 7: Crown 1 | X: 29 Y: 23 | 03 | 00 
1ef9 | 7: Crown 1 | X: 25 Y: 23 | 03 | 00 
1efc | 7: Crown 1 | X: 28 Y: 23 | 03 | 00 
1f58 | 7: Crown 1 | X: 24 Y: 26 | 03 | 00 
1faf | 7: Crown 1 | X: 15 Y: 29 | 03 | 00 
1fcf | 7: Crown 1 | X: 15 Y: 30 | 03 | 00 
1fef | 7: Crown 1 | X: 15 Y: 31 | 03 | 00 
1fb3 | 7: Crown 1 | X: 19 Y: 29 | 03 | 00 
1fd3 | 7: Crown 1 | X: 19 Y: 30 | 03 | 00 
1ff3 | 7: Crown 1 | X: 19 Y: 31 | 03 | 00 
1fb7 | 7: Crown 1 | X: 23 Y: 29 | 03 | 00 
1fd7 | 7: Crown 1 | X: 23 Y: 30 | 03 | 00 
1ff7 | 7: Crown 1 | X: 23 Y: 31 | 03 | 00 
1fbb | 7: Crown 1 | X: 27 Y: 29 | 03 | 00 
1fdb | 7: Crown 1 | X: 27 Y: 30 | 03 | 00 
1ffb | 7: Crown 1 | X: 27 Y: 31 | 03 | 00 
1e47 | 7: Crown 1 | X: 07 Y: 18 | 03 | 00 
1e07 | 7: Crown 1 | X: 07 Y: 16 | 84 | 00 
1e07 | 7: Crown 1 | X: 07 Y: 16 | 03 | 00 
1d26 | 7: Crown 1 | X: 06 Y: 09 | 03 | 00 
1ea1 | 7: Crown 1 | X: 01 Y: 21 | 03 | 00 
1fc1 | 7: Crown 1 | X: 01 Y: 30 | 0c | 00 
1fc3 | 7: Crown 1 | X: 03 Y: 30 | 0c | 00 
1b9b | 6: Crown 2 | X: 27 Y: 28 | 03 | 00 
1bbc | 6: Crown 2 | X: 28 Y: 29 | 84 | 00 
1bbc | 6: Crown 2 | X: 28 Y: 29 | 03 | 00 
1b7f | 6: Crown 2 | X: 31 Y: 27 | 03 | 00 
1b5d | 6: Crown 2 | X: 29 Y: 26 | 03 | 00 
1b1d | 6: Crown 2 | X: 29 Y: 24 | 03 | 00 
1b9d | 6: Crown 2 | X: 29 Y: 28 | 03 | 00 
19f7 | 6: Crown 2 | X: 23 Y: 15 | 0b | 0e 
1a71 | 6: Crown 2 | X: 17 Y: 19 | 0b | 0e 
0fe8 | 3: Sword 3 | X: 08 Y: 31 | 84 | 00 
0fe8 | 3: Sword 3 | X: 08 Y: 31 | 03 | 00 
1a55 | 6: Crown 2 | X: 21 Y: 18 | 81 | 00 
1a76 | 6: Crown 2 | X: 22 Y: 19 | 81 | 00 
1a73 | 6: Crown 2 | X: 19 Y: 19 | 81 | 00 
1888 | 6: Crown 2 | X: 08 Y: 04 | 03 | 00 
18a2 | 6: Crown 2 | X: 02 Y: 05 | 03 | 00 
1931 | 6: Crown 2 | X: 17 Y: 09 | 03 | 00 
196f | 6: Crown 2 | X: 15 Y: 11 | 03 | 00 
1877 | 6: Crown 2 | X: 23 Y: 03 | 03 | 00 
1853 | 6: Crown 2 | X: 19 Y: 02 | 03 | 00 
185b | 6: Crown 2 | X: 27 Y: 02 | 03 | 00 
1913 | 6: Crown 2 | X: 19 Y: 08 | 03 | 00 
191b | 6: Crown 2 | X: 27 Y: 08 | 03 | 00 
191e | 6: Crown 2 | X: 30 Y: 08 | 03 | 00 
193f | 6: Crown 2 | X: 31 Y: 09 | 03 | 00 
0fac | 3: Sword 3 | X: 12 Y: 29 | 03 | 00 
0fcc | 3: Sword 3 | X: 12 Y: 30 | 03 | 00 
0fec | 3: Sword 3 | X: 12 Y: 31 | 03 | 00 
0fd1 | 3: Sword 3 | X: 17 Y: 30 | 03 | 00 
0fb3 | 3: Sword 3 | X: 19 Y: 29 | 03 | 00 
0f71 | 3: Sword 3 | X: 17 Y: 27 | 03 | 00 
0f6f | 3: Sword 3 | X: 15 Y: 27 | 03 | 00 
02fa | 0: Forest  | X: 26 Y: 23 | 03 | 00 
1961 | 6: Crown 2 | X: 01 Y: 11 | 83 | 00 
1980 | 6: Crown 2 | X: 00 Y: 12 | 83 | 00 
19a0 | 6: Crown 2 | X: 00 Y: 13 | 83 | 00 
17fb | 5: Shield  | X: 27 Y: 31 | 84 | 00 
170d | 5: Shield  | X: 13 Y: 24 | 03 | 00 
16a9 | 5: Shield  | X: 09 Y: 21 | 0b | 0f 
1b11 | 6: Crown 2 | X: 17 Y: 24 | 0d | 00 
19ca | 6: Crown 2 | X: 10 Y: 14 | 0d | 00 
1a4c | 6: Crown 2 | X: 12 Y: 18 | 0d | 00 
1b8f | 6: Crown 2 | X: 15 Y: 28 | 0d | 00 
0774 | 1: Sword 1 | X: 20 Y: 27 | 0d | 00 
0766 | 1: Sword 1 | X: 06 Y: 27 | 0d | 00 
07e0 | 1: Sword 1 | X: 00 Y: 31 | 0d | 00 
07fa | 1: Sword 1 | X: 26 Y: 31 | 0d | 00 
07f7 | 1: Sword 1 | X: 23 Y: 31 | 0d | 00 
07ca | 1: Sword 1 | X: 10 Y: 30 | 0b | 0b 
0001 | 0: Forest  | X: 01 Y: 00 | 0f | 00 
03de | 0: Forest  | X: 30 Y: 30 | 03 | 00 
0000 |            |             | 00 | 00 

### $3a76: Locked doors

62 6-byte entries. Two bytes for the map address of the door, two bytes for the
address of the keyhole, one byte unknown, and one byte which is always zero.

Addr | Map        | Coords      |Target| Map        | Coords      | Data
-----|------------|-------------|------|------------|-------------|----
02d7 | 0: Forest  | X: 23 Y: 22 | 02d6 | 0: Forest  | X: 22 Y: 22 | 81 
04a8 | 1: Sword 1 | X: 08 Y: 05 | 0489 | 1: Sword 1 | X: 09 Y: 04 | 81 
042f | 1: Sword 1 | X: 15 Y: 01 | 044f | 1: Sword 1 | X: 15 Y: 02 | 81 
04aa | 1: Sword 1 | X: 10 Y: 05 | 048b | 1: Sword 1 | X: 11 Y: 04 | 81 
0515 | 1: Sword 1 | X: 21 Y: 08 | 0516 | 1: Sword 1 | X: 22 Y: 08 | 81 
0551 | 1: Sword 1 | X: 17 Y: 10 | 0532 | 1: Sword 1 | X: 18 Y: 09 | 81 
04ef | 1: Sword 1 | X: 15 Y: 07 | 0510 | 1: Sword 1 | X: 16 Y: 08 | 81 
052f | 1: Sword 1 | X: 15 Y: 09 | 0550 | 1: Sword 1 | X: 16 Y: 10 | 81 
056f | 1: Sword 1 | X: 15 Y: 11 | 0590 | 1: Sword 1 | X: 16 Y: 12 | 81 
05af | 1: Sword 1 | X: 15 Y: 13 | 05d0 | 1: Sword 1 | X: 16 Y: 14 | 81 
089e | 2: Sword 2 | X: 30 Y: 04 | 087f | 2: Sword 2 | X: 31 Y: 03 | 81 
08d1 | 2: Sword 2 | X: 17 Y: 06 | 08b2 | 2: Sword 2 | X: 18 Y: 05 | 81 
0855 | 2: Sword 2 | X: 21 Y: 02 | 0835 | 2: Sword 2 | X: 21 Y: 01 | 81 
0894 | 2: Sword 2 | X: 20 Y: 04 | 0875 | 2: Sword 2 | X: 21 Y: 03 | 81 
09cf | 2: Sword 2 | X: 15 Y: 14 | 09ef | 2: Sword 2 | X: 15 Y: 15 | 81 
09cb | 2: Sword 2 | X: 11 Y: 14 | 09ea | 2: Sword 2 | X: 10 Y: 15 | 81 
0843 | 2: Sword 2 | X: 03 Y: 02 | 0823 | 2: Sword 2 | X: 03 Y: 01 | 81 
0dcc | 3: Sword 3 | X: 12 Y: 14 | 0dec | 3: Sword 3 | X: 12 Y: 15 | 81 
0e11 | 3: Sword 3 | X: 17 Y: 16 | 0d7d | 3: Sword 3 | X: 29 Y: 11 | 81 
1429 | 5: Shield  | X: 09 Y: 01 | 144a | 5: Shield  | X: 10 Y: 02 | 81 
142c | 5: Shield  | X: 12 Y: 01 | 140d | 5: Shield  | X: 13 Y: 00 | 81 
1459 | 5: Shield  | X: 25 Y: 02 | 1458 | 5: Shield  | X: 24 Y: 02 | 81 
152b | 5: Shield  | X: 11 Y: 09 | 154b | 5: Shield  | X: 11 Y: 10 | 81 
15c7 | 5: Shield  | X: 07 Y: 14 | 15a8 | 5: Shield  | X: 08 Y: 13 | 81 
147d | 5: Shield  | X: 29 Y: 03 | 145d | 5: Shield  | X: 29 Y: 02 | 81 
1618 | 5: Shield  | X: 24 Y: 16 | 163f | 5: Shield  | X: 31 Y: 17 | 83 
10f5 | 4: Cup     | X: 21 Y: 07 | 1116 | 4: Cup     | X: 22 Y: 08 | 81 
1119 | 4: Cup     | X: 25 Y: 08 | 1139 | 4: Cup     | X: 25 Y: 09 | 81 
111a | 4: Cup     | X: 26 Y: 08 | 113b | 4: Cup     | X: 27 Y: 09 | 81 
111c | 4: Cup     | X: 28 Y: 08 | 113d | 4: Cup     | X: 29 Y: 09 | 81 
115e | 4: Cup     | X: 30 Y: 10 | 115f | 4: Cup     | X: 31 Y: 10 | 81 
1294 | 4: Cup     | X: 20 Y: 20 | 12dc | 4: Cup     | X: 28 Y: 22 | 81 
1313 | 4: Cup     | X: 19 Y: 24 | 1332 | 4: Cup     | X: 18 Y: 25 | 81 
13b3 | 4: Cup     | X: 19 Y: 29 | 13d2 | 4: Cup     | X: 18 Y: 30 | 81 
12e7 | 4: Cup     | X: 07 Y: 23 | 1307 | 4: Cup     | X: 07 Y: 24 | 81 
1327 | 4: Cup     | X: 07 Y: 25 | 1306 | 4: Cup     | X: 06 Y: 24 | 81 
1d59 | 7: Crown 1 | X: 25 Y: 10 | 1d5a | 7: Crown 1 | X: 26 Y: 10 | 81 
1daa | 7: Crown 1 | X: 10 Y: 13 | 1dca | 7: Crown 1 | X: 10 Y: 14 | 81 
1d68 | 7: Crown 1 | X: 08 Y: 11 | 1d69 | 7: Crown 1 | X: 09 Y: 11 | 83 
1dc6 | 7: Crown 1 | X: 06 Y: 14 | 1de7 | 7: Crown 1 | X: 07 Y: 15 | 83 
1d27 | 7: Crown 1 | X: 07 Y: 09 | 1d46 | 7: Crown 1 | X: 06 Y: 10 | 81 
1b78 | 6: Crown 2 | X: 24 Y: 27 | 1ab8 | 6: Crown 2 | X: 24 Y: 21 | 81 
1a5c | 6: Crown 2 | X: 28 Y: 18 | 1a50 | 6: Crown 2 | X: 16 Y: 18 | 83 
190c | 6: Crown 2 | X: 12 Y: 08 | 192c | 6: Crown 2 | X: 12 Y: 09 | 83 
18b5 | 6: Crown 2 | X: 21 Y: 05 | 18bb | 6: Crown 2 | X: 27 Y: 05 | 81 
1926 | 6: Crown 2 | X: 06 Y: 09 | 1925 | 6: Crown 2 | X: 05 Y: 09 | 81 
0fc7 | 3: Sword 3 | X: 07 Y: 30 | 0fe7 | 3: Sword 3 | X: 07 Y: 31 | 81 
0cfa | 3: Sword 3 | X: 26 Y: 07 | 0d1a | 3: Sword 3 | X: 26 Y: 08 | 81 
06f3 | 1: Sword 1 | X: 19 Y: 23 | 0615 | 1: Sword 1 | X: 21 Y: 16 | 81 
06f5 | 1: Sword 1 | X: 21 Y: 23 | 0657 | 1: Sword 1 | X: 23 Y: 18 | 81 
0699 | 1: Sword 1 | X: 25 Y: 20 | 06b8 | 1: Sword 1 | X: 24 Y: 21 | 81 
073e | 1: Sword 1 | X: 30 Y: 25 | 071d | 1: Sword 1 | X: 29 Y: 24 | 81 
06fd | 1: Sword 1 | X: 29 Y: 23 | 06fe | 1: Sword 1 | X: 30 Y: 23 | 81 
06f8 | 1: Sword 1 | X: 24 Y: 23 | 06f7 | 1: Sword 1 | X: 23 Y: 23 | 81 
073b | 1: Sword 1 | X: 27 Y: 25 | 071b | 1: Sword 1 | X: 27 Y: 24 | 81 
0b10 | 2: Sword 2 | X: 16 Y: 24 | 0b0e | 2: Sword 2 | X: 14 Y: 24 | 81 
0a6f | 2: Sword 2 | X: 15 Y: 19 | 0a8c | 2: Sword 2 | X: 12 Y: 20 | 89 
0aba | 2: Sword 2 | X: 26 Y: 21 | 0a9a | 2: Sword 2 | X: 26 Y: 20 | 81 
1a44 | 6: Crown 2 | X: 04 Y: 18 | 1a24 | 6: Crown 2 | X: 04 Y: 17 | 81 
07b1 | 1: Sword 1 | X: 17 Y: 29 | 07cc | 1: Sword 1 | X: 12 Y: 30 | 81 
0bca | 2: Sword 2 | X: 10 Y: 30 | 0bea | 2: Sword 2 | X: 10 Y: 31 | 81 
0000 |            |             | 0000 |            |             | 00 

The only three data bytes used are `81`, `83`, and `89`. The number does not
appear to relate to the type of key required.

### $3bea: Switches

112 6-byte entries, consisting of two bytes each for a map address, target
block, and effect.

Addr | Map        | Coords    |Target| Target map | T. coords | Effect
-----|------------|-----------|------|------------|-----------|--------
000f | 0: Forest  | X:15 Y:00 | 00ae | 0: Forest  | X:14 Y:05 | 8a 20
04a6 | 1: Sword 1 | X:06 Y:05 | 0486 | 1: Sword 1 | X:06 Y:04 | 83 00
04b5 | 1: Sword 1 | X:21 Y:05 | 0413 | 1: Sword 1 | X:19 Y:00 | 83 00
0507 | 1: Sword 1 | X:07 Y:08 | 04c7 | 1: Sword 1 | X:07 Y:06 | 83 00
0505 | 1: Sword 1 | X:05 Y:08 | 0506 | 1: Sword 1 | X:06 Y:08 | 83 00
0584 | 1: Sword 1 | X:04 Y:12 | 0564 | 1: Sword 1 | X:04 Y:11 | 83 00
05c4 | 1: Sword 1 | X:04 Y:14 | 0543 | 1: Sword 1 | X:03 Y:10 | 83 00
050f | 1: Sword 1 | X:15 Y:08 | 052e | 1: Sword 1 | X:14 Y:09 | 81 00
050a | 1: Sword 1 | X:10 Y:08 | 0000 |            |           | 85 00
0529 | 1: Sword 1 | X:09 Y:09 | 0005 |            |           | 85 00 
05e9 | 1: Sword 1 | X:09 Y:15 | 0004 |            |           | 85 00
0567 | 1: Sword 1 | X:07 Y:11 | 0003 |            |           | 85 00
057e | 1: Sword 1 | X:30 Y:11 | 057d | 1: Sword 1 | X:29 Y:11 | 81 00
05bd | 1: Sword 1 | X:29 Y:13 | 059c | 1: Sword 1 | X:28 Y:12 | 81 00
059a | 1: Sword 1 | X:26 Y:12 | 05bb | 1: Sword 1 | X:27 Y:13 | 81 00
059f | 1: Sword 1 | X:31 Y:12 | 05be | 1: Sword 1 | X:30 Y:13 | 01 00
05d9 | 1: Sword 1 | X:25 Y:14 | 05fa | 1: Sword 1 | X:26 Y:15 | 81 00
05fb | 1: Sword 1 | X:27 Y:15 | 05dc | 1: Sword 1 | X:28 Y:14 | 81 00
05fe | 1: Sword 1 | X:30 Y:15 | 05fd | 1: Sword 1 | X:29 Y:15 | 81 00
063d | 1: Sword 1 | X:29 Y:17 | 061c | 1: Sword 1 | X:28 Y:16 | 81 00
063a | 1: Sword 1 | X:26 Y:17 | 0619 | 1: Sword 1 | X:25 Y:16 | 81 00
05b1 | 1: Sword 1 | X:17 Y:13 | 041f | 1: Sword 1 | X:31 Y:00 | 03 00
0a0d | 2: Sword 2 | X:13 Y:16 | 0a10 | 2: Sword 2 | X:16 Y:16 | 83 00
08fc | 2: Sword 2 | X:28 Y:07 | 091d | 2: Sword 2 | X:29 Y:08 | 81 00
0999 | 2: Sword 2 | X:25 Y:12 | 09ba | 2: Sword 2 | X:26 Y:13 | 81 00
09fd | 2: Sword 2 | X:29 Y:15 | 09de | 2: Sword 2 | X:30 Y:14 | 81 00
08cd | 2: Sword 2 | X:13 Y:06 | 0925 | 2: Sword 2 | X:05 Y:09 | 83 00
0909 | 2: Sword 2 | X:09 Y:08 | 08ee | 2: Sword 2 | X:14 Y:07 | 83 00
08e5 | 2: Sword 2 | X:05 Y:07 | 0949 | 2: Sword 2 | X:09 Y:10 | 83 00
0c72 | 3: Sword 3 | X:18 Y:03 | 0c92 | 3: Sword 3 | X:18 Y:04 | 83 00
0e26 | 3: Sword 3 | X:06 Y:17 | 0e20 | 3: Sword 3 | X:00 Y:17 | 83 00
0d8b | 3: Sword 3 | X:11 Y:12 | 0d91 | 3: Sword 3 | X:17 Y:12 | 01 00
0d9f | 3: Sword 3 | X:31 Y:12 | 0e5c | 3: Sword 3 | X:28 Y:18 | 83 00
14a0 | 5: Shield  | X:00 Y:05 | 14a8 | 5: Shield  | X:08 Y:05 | 07 40
152e | 5: Shield  | X:14 Y:09 | 154e | 5: Shield  | X:14 Y:10 | 83 00
1505 | 5: Shield  | X:05 Y:08 | 14c6 | 5: Shield  | X:06 Y:06 | 83 00
1594 | 5: Shield  | X:20 Y:12 | 14dc | 5: Shield  | X:28 Y:06 | 83 00
151d | 5: Shield  | X:29 Y:08 | 1554 | 5: Shield  | X:20 Y:10 | 83 00
1558 | 5: Shield  | X:24 Y:10 | 14f9 | 5: Shield  | X:25 Y:07 | 83 00
15fe | 5: Shield  | X:30 Y:15 | 1677 | 5: Shield  | X:23 Y:19 | 81 00
1104 | 4: Cup     | X:04 Y:08 | 1128 | 4: Cup     | X:08 Y:09 | 83 00
1272 | 4: Cup     | X:18 Y:19 | 126f | 4: Cup     | X:15 Y:19 | 0c 00
1346 | 4: Cup     | X:06 Y:26 | 134a | 4: Cup     | X:10 Y:26 | 03 00
1324 | 4: Cup     | X:04 Y:25 | 1341 | 4: Cup     | X:01 Y:26 | 03 00
1322 | 4: Cup     | X:02 Y:25 | 1385 | 4: Cup     | X:05 Y:28 | 03 00
1384 | 4: Cup     | X:04 Y:28 | 12e1 | 4: Cup     | X:01 Y:23 | 03 00
12c1 | 4: Cup     | X:01 Y:22 | 1380 | 4: Cup     | X:00 Y:28 | 03 00
12e2 | 4: Cup     | X:02 Y:23 | 13c5 | 4: Cup     | X:05 Y:30 | 03 00
1382 | 4: Cup     | X:02 Y:28 | 12e0 | 4: Cup     | X:00 Y:23 | 03 00
13a1 | 4: Cup     | X:01 Y:29 | 13a4 | 4: Cup     | X:04 Y:29 | 01 00
0002 | 0: Forest  | X:02 Y:00 | 0000 | 0: Forest  | X:00 Y:00 | 0e 00
1c29 | 7: Crown 1 | X:09 Y:01 | 1c35 | 7: Crown 1 | X:21 Y:01 | 0c 00
1cfb | 7: Crown 1 | X:27 Y:07 | 1d3d | 7: Crown 1 | X:29 Y:09 | 0c 00
1d9d | 7: Crown 1 | X:29 Y:12 | 1d3b | 7: Crown 1 | X:27 Y:09 | 03 00
1694 | 5: Shield  | X:20 Y:20 | 164c | 5: Shield  | X:12 Y:18 | 03 00
1d38 | 7: Crown 1 | X:24 Y:09 | 1d28 | 7: Crown 1 | X:08 Y:09 | 03 00
1e2f | 7: Crown 1 | X:15 Y:17 | 1df2 | 7: Crown 1 | X:18 Y:15 | 0c 00
1e50 | 7: Crown 1 | X:16 Y:18 | 1dee | 7: Crown 1 | X:14 Y:15 | 0c 00
1e51 | 7: Crown 1 | X:17 Y:18 | 1e6e | 7: Crown 1 | X:14 Y:19 | 0c 00
1e1a | 7: Crown 1 | X:26 Y:16 | 1edb | 7: Crown 1 | X:27 Y:22 | 03 00
1f1f | 7: Crown 1 | X:31 Y:24 | 1ed7 | 7: Crown 1 | X:23 Y:22 | 03 00
1f6a | 7: Crown 1 | X:10 Y:27 | 1f10 | 7: Crown 1 | X:16 Y:24 | 03 00
1f30 | 7: Crown 1 | X:16 Y:25 | 1f8c | 7: Crown 1 | X:12 Y:28 | 03 00
1a3b | 6: Crown 2 | X:27 Y:17 | 1a34 | 6: Crown 2 | X:20 Y:17 | 01 00
1a53 | 6: Crown 2 | X:19 Y:18 | 1a9f | 6: Crown 2 | X:31 Y:20 | 03 00
188d | 6: Crown 2 | X:13 Y:04 | 18c2 | 6: Crown 2 | X:02 Y:06 | 03 00
1a11 | 6: Crown 2 | X:17 Y:16 | 18ff | 6: Crown 2 | X:31 Y:07 | 03 00
19be | 6: Crown 2 | X:30 Y:13 | 182e | 6: Crown 2 | X:14 Y:01 | 03 00
1d64 | 7: Crown 1 | X:04 Y:11 | 1d09 | 7: Crown 1 | X:09 Y:08 | 03 00
0660 | 1: Sword 1 | X:00 Y:19 | 0643 | 1: Sword 1 | X:03 Y:18 | 83 00
0665 | 1: Sword 1 | X:05 Y:19 | 0623 | 1: Sword 1 | X:03 Y:17 | 83 00
0707 | 1: Sword 1 | X:07 Y:24 | 0622 | 1: Sword 1 | X:02 Y:17 | 83 00
0720 | 1: Sword 1 | X:00 Y:25 | 0602 | 1: Sword 1 | X:02 Y:16 | 83 00
0661 | 1: Sword 1 | X:01 Y:19 | 0683 | 1: Sword 1 | X:03 Y:20 | 01 00
06e0 | 1: Sword 1 | X:00 Y:23 | 06c1 | 1: Sword 1 | X:01 Y:22 | 01 00
0724 | 1: Sword 1 | X:04 Y:25 | 0703 | 1: Sword 1 | X:03 Y:24 | 01 00
06a6 | 1: Sword 1 | X:06 Y:21 | 06c5 | 1: Sword 1 | X:05 Y:22 | 01 00
0cf4 | 3: Sword 3 | X:20 Y:07 | 0cf8 | 3: Sword 3 | X:24 Y:07 | 03 00
0638 | 1: Sword 1 | X:24 Y:17 | 0659 | 1: Sword 1 | X:25 Y:18 | 0a 20
063c | 1: Sword 1 | X:28 Y:17 | 0659 | 1: Sword 1 | X:25 Y:18 | 0a 00
069f | 1: Sword 1 | X:31 Y:20 | 06dc | 1: Sword 1 | X:28 Y:22 | 0a 20
06fc | 1: Sword 1 | X:28 Y:23 | 06ba | 1: Sword 1 | X:26 Y:21 | 0a 20
066f | 1: Sword 1 | X:15 Y:19 | 0653 | 1: Sword 1 | X:19 Y:18 | 0c 00
068f | 1: Sword 1 | X:15 Y:20 | 0655 | 1: Sword 1 | X:21 Y:18 | 0c 00
06af | 1: Sword 1 | X:15 Y:21 | 0693 | 1: Sword 1 | X:19 Y:20 | 0c 00
06cf | 1: Sword 1 | X:15 Y:22 | 0695 | 1: Sword 1 | X:21 Y:20 | 0c 00
071f | 1: Sword 1 | X:31 Y:24 | 065b | 1: Sword 1 | X:27 Y:18 | 0a 20
077c | 1: Sword 1 | X:28 Y:27 | 079e | 1: Sword 1 | X:30 Y:28 | 0c 00
075b | 1: Sword 1 | X:27 Y:26 | 07de | 1: Sword 1 | X:30 Y:30 | 0c 00
0bc3 | 2: Sword 2 | X:03 Y:30 | 0b63 | 2: Sword 2 | X:03 Y:27 | 03 00
0ae8 | 2: Sword 2 | X:08 Y:23 | 0aa9 | 2: Sword 2 | X:09 Y:21 | 0c 00
0acb | 2: Sword 2 | X:11 Y:22 | 0aa9 | 2: Sword 2 | X:09 Y:21 | 0c 00
0a45 | 2: Sword 2 | X:05 Y:18 | 0a58 | 2: Sword 2 | X:24 Y:18 | 03 00
0add | 2: Sword 2 | X:29 Y:22 | 0b19 | 2: Sword 2 | X:25 Y:24 | 03 00
0a74 | 2: Sword 2 | X:20 Y:19 | 0ad3 | 2: Sword 2 | X:19 Y:22 | 0c 00
0a94 | 2: Sword 2 | X:20 Y:20 | 0ad5 | 2: Sword 2 | X:21 Y:22 | 0c 00
0a54 | 2: Sword 2 | X:20 Y:18 | 0ad5 | 2: Sword 2 | X:21 Y:22 | 0c 00
0a33 | 2: Sword 2 | X:19 Y:17 | 0ad3 | 2: Sword 2 | X:19 Y:22 | 0c 00
0a52 | 2: Sword 2 | X:18 Y:18 | 0ad1 | 2: Sword 2 | X:17 Y:22 | 0c 00
0a92 | 2: Sword 2 | X:18 Y:20 | 0ad3 | 2: Sword 2 | X:19 Y:22 | 0c 00
0a71 | 2: Sword 2 | X:17 Y:19 | 0ad1 | 2: Sword 2 | X:17 Y:22 | 0c 00
0a90 | 2: Sword 2 | X:16 Y:20 | 0acf | 2: Sword 2 | X:15 Y:22 | 0c 00
029b | 0: Forest  | X:27 Y:20 | 025b | 0: Forest  | X:27 Y:18 | 03 00
17d8 | 5: Shield  | X:24 Y:30 | 17b5 | 5: Shield  | X:21 Y:29 | 0c 00
17f3 | 5: Shield  | X:19 Y:31 | 176e | 5: Shield  | X:14 Y:27 | 0c 00
17c7 | 5: Shield  | X:07 Y:30 | 1728 | 5: Shield  | X:08 Y:25 | 0c 00
17e1 | 5: Shield  | X:01 Y:31 | 1721 | 5: Shield  | X:01 Y:25 | 03 00
16c4 | 5: Shield  | X:04 Y:22 | 17fb | 5: Shield  | X:27 Y:31 | 03 00
07ee | 1: Sword 1 | X:14 Y:31 | 1d05 | 7: Crown 1 | X:05 Y:08 | 03 00
07c7 | 1: Sword 1 | X:07 Y:30 | 00f2 |            |           | 05 00
0a9e | 2: Sword 2 | X:30 Y:20 | 02fa | 0: Forest  | X:26 Y:23 | 03 00
0000 |            |           | 0000 |            |           | 00 00

The switch effects are as follows:

01 / 81
: ? There are 8 `01`s and 13 `81`s in the game.

03 / 83
: ? Most common type. There are 29 `03`s and 23 `83`s.

05 / 85
: ? The address field is not used here as an address, but for something else,
probably an offset. There are 4 `85` and one `05` in the game.

07
: Fireball trap. The second byte is `40` in the only instance where this appears
in the first dungeon.

0a
: ? There are 5 `0a` and 1 `8a`.

0a / 8a
: Change the target square to another tile type. The second byte determines what
it's replaced with. In all instances it's either `00`, solid block, or `20`,
empty space. There are only 5 `0a` and one `8a` in the game.

0c
: ? There are 25 of these.

0e
: There is only one of these, and it's set on the top leftmost accessible block
in the game, targeting the very top-left block.

The entries can have an 8 for the first digit, which at a guess stores the
current activation condition of the switch.

The last entry is all zeroes. It may be that the design of the top-left corner,
which is blocked off from the rest of the map, is intentional to isolate block
0,0.

### $3e8a: Text strings, NPCs

Text for NPCs, beginning with a four byte header. Each entry begins with a ten
bytes of header: two bytes for the length of the string (including its header),
a unique ID number, seven more bytes (unknown), and the text in ASCII terminated
by null bytes.

 ID | Bytes    |Mon | Bytes    | Text
----|----------|----|----------|----------------------
 01 | 47 4b 00 | 08 | 01 02 1a | I HAVE LOST MY CHILD
 04 | 46 00 00 | 08 | 01 02 18 | I HAVE LOST MY COVER
 06 | 5d 00 00 | 08 | 01 02 14 | HAVE YOU SEEN MY CUP?
 07 | 39 00 00 | 08 | 01 02 15 | I HAVE LOST MY WEAPON
 0e | 00 53 00 | 01 | 01 02 2a | DO NOT HIT ME
 0f | 00 52 00 | 01 | 01 02 19 | WE WILL MAKE YOU ILL
 0d | 53 5b 01 | 05 | 01 02 28 | I WILL PAY YOU
 03 | 5b 00 00 | 03 | 01 02 38 | PAY ME
 05 | 6f 62 00 | 0a | 01 02 30 | LEAVE NOW
 02 | 00 00 00 | 09 | 01 02 22 | NO ONE WILL PASS
 09 | 00 60 00 | 00 | 01 02 3b | ARRGH
 0c | 00 5d 00 | 06 | 01 02 3c | HISSS
 0a | 00 62 00 | 06 | 01 02 26 | I SEE MY DINNER
 0b | 00 61 00 | 00 | 01 02 2d | YOU WILL DIE
 13 | 34 38 00 | 00 | 01 02 14 | LET ME SEE YOUR SWORD
 14 | 00 64 00 | 00 | 01 02 0e | LORDFEAR WILL DEFEAT YOU
 15 | 00 4d 00 | 00 | 01 02 26 | I WILL HEAL YOU
 16 | 00 62 00 | 04 | 01 02 1e | LEAVE ME IN PIECES
 17 | 00 63 00 | 00 | 01 02 13 | I KNOW WHERE THE KEY IS
 18 | 00 62 00 | ff | 01 00 08 | 
 19 | 00 62 00 | ff | 01 00 12 | 
 11 | 00 25 00 | 00 | 01 02 38 | SQWARK
 1a | 00 47 00 | 04 | 01 02 27 | ONE IS THE EXIT
 1b | 00 61 00 | 00 | 02 02 0a | I WANT TO BE THE GUARDIAN OF<br>THE CASTLE
 1c | 00 61 00 | 00 | 02 02 13 | I AM THE GUARDIAN OF THE<br>CASTLE
 1d | 00 61 00 | ff | 02 00 08 | 
 1e | 00 63 00 | ff | 02 00 14 | 
 12 | 00 5b 00 | 00 | 01 02 31 | STAY AWAY
 1f | 00 4e 00 | 00 | 01 02 26 | I WILL HEAL YOU
 21 | 00 4d 00 | 00 | 01 02 26 | I WILL HEAL YOU
 10 | 00 61 00 | 00 | 01 02 1c | HOW DARE YOU ENTER
 20 | 00 5f 00 | 0a | 01 02 26 | HE HE HE HE HE
 22 | 00 64 00 | 00 | 01 02 2d | HA HA HA HA
 23 | 00 08 00 | 00 | 01 02 21 | THE CROWN IS MINE

There is no text ID #8.

The monster field determines the picture which appears above the main play area:

00
: Treguard

01
: Pickle

02
: Red-eyed hooded figure (unused)

03
: Hooded figure

04
: Jester

05
: ? - says I WILL PAY YOU

06
: Cobra

07
: ? (unused)

08
: Tree

09
: Troll

0a
: Witch

ff
: Disabled

### $41fa: Monsters

Contains map offsets for monsters.

The first bytes are `1be8`, or 7,144, the section length, followed by `00 c8`,
200, the meaning of which is unknown. It ends with the end marker `88 82 88 81`
followed by padding with zeroes until the end of the section.

Each monster entry is of variable width. The first two bytes of each entry give
the map address for the monster's starting tile. The fourth byte is the length
of the entry, including this four byte header. Each entry follows with eight
bytes, then what at an educated guess may be one four-byte section per monster.
The significance of the bytes have not yet fully decoded.

Addr | Map        | Coords      | ?                       | Monsters
-----|------------|-------------|-------------------------|------------
03ac | 0: Forest  | X: 12 Y: 29 | 00 02 09 14 03 0e 46 05 | 00 46 14 00
0027 | 0: Forest  | X: 07 Y: 01 | 00 00 01 0a 00 0a 0a 04 | 00 0a 14 00
0034 | 0: Forest  | X: 20 Y: 01 | 00 01 0a 0c 08 0e 0a 05 | 00 0a 14 00
0097 | 0: Forest  | X: 23 Y: 04 | 00 01 0a 0b 00 0a 0a 06 | 00 14 14 00
00e4 | 0: Forest  | X: 04 Y: 07 | 00 02 05 09 00 0d 0a 07 | 00 0c 14 00
016a | 0: Forest  | X: 10 Y: 11 | 00 01 04 06 00 0a 0a 08 | 00 17 14 00
020c | 0: Forest  | X: 12 Y: 16 | 00 01 03 0c 00 0a 0a 09 | 00 32 14 00
02f1 | 0: Forest  | X: 17 Y: 23 | 81 03 38 ff 01 dc 01 01 | 03 e8 14 00
0162 | 0: Forest  | X: 02 Y: 11 | 84 02 0d ff 01 dc 01 01 | 03 e8 14 00
0156 | 0: Forest  | X: 22 Y: 10 | 86 03 2e ff 01 dc 01 01 | 03 e8 14 00
00c2 | 0: Forest  | X: 02 Y: 06 | 87 00 03 ff 01 e1 fa 01 | 07 d0 14 00
02f6 | 0: Forest  | X: 22 Y: 23 | 00 01 05 0a 03 22 5a 1e | 05 14 00 00<br>05 14 02 00
02f7 | 0: Forest  | X: 23 Y: 23 | 00 00 06 0a 03 22 5a 1e | 10 cc 00 00<br>10 cc 02 00
0446 | 1: Sword 1 | X: 06 Y: 02 | 00 03 0a 0c 14 01 81 14 | 01 a4 14 00
0427 | 1: Sword 1 | X: 07 Y: 01 | 00 00 06 0c 13 01 b3 1d | 01 86 14 00
0430 | 1: Sword 1 | X: 16 Y: 01 | 00 01 06 0e 15 0a 81 14 | 00 f0 00 00<br>00 f0 02 00<br>00 f0 06 00<br>00 f0 08 00
0450 | 1: Sword 1 | X: 16 Y: 02 | 00 00 00 08 15 0a 8b 14 | 00 fa 00 00<br>00 fa 02 00<br>00 fa 07 00
040c | 1: Sword 1 | X: 12 Y: 00 | 00 01 01 09 13 0a 81 19 | 00 fa 00 00<br>00 fa 02 00
041b | 1: Sword 1 | X: 27 Y: 00 | 00 02 0a 0f 12 3c 81 11 | 01 54 14 00
041d | 1: Sword 1 | X: 29 Y: 00 | 00 02 02 09 12 3c 81 11 | 01 54 14 00
04c6 | 1: Sword 1 | X: 06 Y: 06 | 00 00 01 09 12 0a 81 1b | 00 f0 00 00<br>00 f0 02 00
0525 | 1: Sword 1 | X: 05 Y: 09 | 00 01 08 0a 13 0a 6d 11 | 01 86 00 00<br>01 86 02 00
05a4 | 1: Sword 1 | X: 04 Y: 13 | 00 01 09 0b 14 0a 77 11 | 00 f0 00 00<br>00 f0 02 00
0503 | 1: Sword 1 | X: 03 Y: 08 | 00 02 0a 12 1b 32 c7 1f | 02 a8 14 00
0458 | 1: Sword 1 | X: 24 Y: 02 | 00 00 0a 0c 13 0a 63 1b | 01 22 00 00<br>01 22 02 00
045a | 1: Sword 1 | X: 26 Y: 02 | 00 01 04 09 13 0a 4f 1d | 03 7a 14 00
047d | 1: Sword 1 | X: 29 Y: 03 | 00 00 06 0a 14 0a 31 19 | 01 b8 00 00<br>01 b8 02 00<br>01 b8 07 00
049c | 1: Sword 1 | X: 28 Y: 04 | 00 00 02 09 14 0a 63 1a | 01 b8 00 00<br>01 b8 02 00
049e | 1: Sword 1 | X: 30 Y: 04 | 00 02 05 0c 15 5a 81 14 | 01 54 00 00<br>01 54 02 00<br>01 54 07 00
059b | 1: Sword 1 | X: 27 Y: 12 | 00 03 02 06 12 0a 59 11 | 00 f0 14 00
059e | 1: Sword 1 | X: 30 Y: 12 | 00 01 04 0b 12 0a 63 11 | 02 4e 00 00<br>02 4e 02 00
05da | 1: Sword 1 | X: 26 Y: 14 | 00 00 00 09 13 0a 77 11 | 01 54 14 00
05de | 1: Sword 1 | X: 30 Y: 14 | 00 01 01 0a 14 0a 59 11 | 01 b8 14 00
061d | 1: Sword 1 | X: 29 Y: 16 | 00 02 08 0b 14 0a 4f 11 | 02 80 00 00<br>02 80 02 00
061a | 1: Sword 1 | X: 26 Y: 16 | 00 02 0a 0b 14 0a 63 18 | 01 f4 00 00<br>01 f4 02 00<br>01 f4 07 00
0617 | 1: Sword 1 | X: 23 Y: 16 | 00 03 05 09 1b 3c 81 1b | 02 e4 14 00
05f8 | 1: Sword 1 | X: 24 Y: 15 | 00 02 06 0a 1b 3c 81 1b | 02 e4 14 00
0572 | 1: Sword 1 | X: 18 Y: 11 | 00 01 04 0c 1b 3c 81 11 | 02 e4 14 00
0593 | 1: Sword 1 | X: 19 Y: 12 | 00 03 03 09 1b 32 81 1d | 02 b2 00 00<br>02 b2 02 00
05b4 | 1: Sword 1 | X: 20 Y: 13 | 00 02 01 0c 1b 3c 81 15 | 02 44 00 00<br>02 44 02 00
05b6 | 1: Sword 1 | X: 22 Y: 13 | 00 03 03 0e 1b 46 81 1d | 02 94 00 00<br>02 94 02 00<br>02 94 07 00
05b7 | 1: Sword 1 | X: 23 Y: 13 | 00 00 0b 0f 1b 3c 81 15 | 02 f8 00 00<br>02 f8 02 00<br>02 f8 06 00<br>02 f8 08 00
0598 | 1: Sword 1 | X: 24 Y: 12 | 00 00 0a 0b 1b 32 81 17 | 01 fe 00 00<br>01 fe 02 00
0579 | 1: Sword 1 | X: 25 Y: 11 | 00 01 05 0a 1b 32 81 15 | 01 68 00 00<br>01 68 02 00<br>01 68 07 00
05f1 | 1: Sword 1 | X: 17 Y: 15 | 00 00 04 07 1c 0a 45 09 | 00 78 14 00
0958 | 2: Sword 2 | X: 24 Y: 10 | 00 01 01 0d 12 0a 6d 0e | 01 b8 14 00
0978 | 2: Sword 2 | X: 24 Y: 11 | 00 02 03 0e 13 0a 81 0f | 02 1c 14 00
083f | 2: Sword 2 | X: 31 Y: 01 | 00 01 01 0c 16 0a 81 07 | 01 b8 14 00
08d4 | 2: Sword 2 | X: 20 Y: 06 | 00 03 04 08 17 1e 81 07 | 01 04 14 00
08d5 | 2: Sword 2 | X: 21 Y: 06 | 00 02 03 05 17 28 4f 07 | 01 72 14 00
08f4 | 2: Sword 2 | X: 20 Y: 07 | 00 02 02 0b 17 1e 81 03 | 01 18 14 00
08f5 | 2: Sword 2 | X: 21 Y: 07 | 00 00 04 0a 17 14 81 03 | 01 cc 14 00
08f6 | 2: Sword 2 | X: 22 Y: 07 | 00 03 00 09 17 1e 63 07 | 01 72 14 00
0952 | 2: Sword 2 | X: 18 Y: 10 | 00 00 01 0b 17 28 81 07 | 01 18 14 00
0972 | 2: Sword 2 | X: 18 Y: 11 | 00 01 0a 0d 17 1e 4f 07 | 01 68 14 00
0973 | 2: Sword 2 | X: 19 Y: 11 | 00 01 06 07 17 14 81 07 | 01 c2 14 00
0974 | 2: Sword 2 | X: 20 Y: 11 | 00 02 05 09 17 14 3b 07 | 01 54 14 00
0976 | 2: Sword 2 | X: 22 Y: 11 | 00 01 04 07 1d 0a 31 0c | 01 18 00 00<br>01 18 02 00<br>01 18 07 00
093a | 2: Sword 2 | X: 26 Y: 09 | 00 00 0b 0c 12 0a 27 14 | 00 f0 00 00<br>00 f0 02 00<br>00 f0 06 00<br>00 f0 08 00
093c | 2: Sword 2 | X: 28 Y: 09 | 00 01 08 0e 13 0a 45 17 | 00 f0 00 00<br>00 f0 02 00<br>00 f0 07 00
091e | 2: Sword 2 | X: 30 Y: 08 | 00 00 08 0d 14 0a 63 1a | 01 54 00 00<br>01 54 02 00<br>01 54 06 00<br>01 54 08 00
097b | 2: Sword 2 | X: 27 Y: 11 | 00 00 02 06 1d 0a 59 0c | 00 5a 00 00<br>00 5a 02 00<br>00 5a 07 00
097c | 2: Sword 2 | X: 28 Y: 11 | 00 02 06 0d 15 0a 81 14 | 01 86 00 00<br>01 86 02 00
097f | 2: Sword 2 | X: 31 Y: 11 | 00 01 00 0b 16 0a 81 14 | 01 ea 00 00<br>01 ea 02 00
09da | 2: Sword 2 | X: 26 Y: 14 | 00 02 02 0a 16 1e 4f 16 | 01 36 00 00<br>01 36 02 00<br>01 36 07 00
09db | 2: Sword 2 | X: 27 Y: 14 | 00 03 09 0b 15 0a 77 1d | 01 90 00 00<br>01 90 02 00<br>01 90 06 00<br>01 90 08 00
09ff | 2: Sword 2 | X: 31 Y: 15 | 00 03 0c 0d 20 5a 70 1e | 03 48 14 00
0a1d | 2: Sword 2 | X: 29 Y: 16 | 0e 00 04 09 15 0a 63 14 | 00 f0 14 00
09f5 | 2: Sword 2 | X: 21 Y: 15 | 00 03 03 05 17 0a 6d 16 | 01 b8 14 00
09f7 | 2: Sword 2 | X: 23 Y: 15 | 00 03 03 0b 16 0a db 14 | 01 54 14 00
0a15 | 2: Sword 2 | X: 21 Y: 16 | 00 01 05 0c 1f 0a 77 1e | 03 48 14 00
0a18 | 2: Sword 2 | X: 24 Y: 16 | 0f 00 00 05 1d 0a 77 11 | 00 78 00 00<br>00 78 02 00<br>00 78 06 00<br>00 78 08 00
0832 | 2: Sword 2 | X: 18 Y: 01 | 00 02 0d 0f 17 50 59 11 | 01 b8 14 00
0836 | 2: Sword 2 | X: 22 Y: 01 | 00 03 0a 13 1f 1e d1 1b | 03 48 14 00
09eb | 2: Sword 2 | X: 11 Y: 15 | 00 02 02 04 1d 0a 81 13 | 00 78 00 00<br>00 78 02 00<br>00 78 06 00<br>00 78 08 00
0929 | 2: Sword 2 | X: 09 Y: 09 | 8d 01 03 0a 18 01 01 16 | 3e 80 14 00
0828 | 2: Sword 2 | X: 08 Y: 01 | 00 03 05 0f 1f 0a 63 11 | 01 86 14 00
086e | 2: Sword 2 | X: 14 Y: 03 | 00 02 00 0e 1f 0a 59 11 | 01 22 14 00
08ca | 2: Sword 2 | X: 10 Y: 06 | 00 03 02 0d 1f 0a 4f 1b | 00 be 14 00
0946 | 2: Sword 2 | X: 06 Y: 10 | 00 00 09 0c 1f 0a 45 1b | 01 22 14 00
096d | 2: Sword 2 | X: 13 Y: 11 | 00 00 0a 0b 1f 0a 3b 11 | 00 be 14 00
0985 | 2: Sword 2 | X: 05 Y: 12 | 00 01 05 0a 1f 0a 95 1b | 01 22 14 00
0989 | 2: Sword 2 | X: 09 Y: 12 | 00 00 06 09 1f 0a 8b 11 | 00 be 14 00
08ce | 2: Sword 2 | X: 14 Y: 06 | 00 00 03 0b 20 0a 81 15 | 02 e4 14 00
0924 | 2: Sword 2 | X: 04 Y: 09 | 00 02 04 0c 20 0a 81 1b | 03 48 14 00
0c49 | 3: Sword 3 | X: 09 Y: 02 | 83 01 15 c8 05 ff 01 01 | fd e8 14 00
0c2d | 3: Sword 3 | X: 13 Y: 01 | 00 02 00 05 21 01 ab 14 | 00 64 00 00<br>00 64 02 00
0c6e | 3: Sword 3 | X: 14 Y: 03 | 00 03 03 05 21 01 ab 14 | 00 82 00 00<br>00 82 02 00<br>00 82 07 00
0ca6 | 3: Sword 3 | X: 06 Y: 05 | 00 03 03 04 21 01 ab 14 | 00 c8 00 00<br>00 c8 02 00<br>00 c8 06 00<br>00 c8 08 00
0ce9 | 3: Sword 3 | X: 09 Y: 07 | 00 00 02 04 21 01 ab 0a | 00 c8 00 00<br>00 c8 02 00<br>00 c8 07 00
0d03 | 3: Sword 3 | X: 03 Y: 08 | 00 03 03 05 21 01 ab 0a | 00 c8 00 00<br>00 c8 02 00<br>00 c8 06 00<br>00 c8 08 00
0d61 | 3: Sword 3 | X: 01 Y: 11 | 00 03 01 04 21 01 ab 14 | 00 c8 00 00<br>00 c8 02 00<br>00 c8 07 00
0d83 | 3: Sword 3 | X: 03 Y: 12 | 00 01 01 04 21 01 ab 14 | 00 c8 00 00<br>00 c8 02 00<br>00 c8 06 00<br>00 c8 08 00
0c54 | 3: Sword 3 | X: 20 Y: 02 | 00 00 03 0f 03 14 45 11 | 01 22 14 00
0c74 | 3: Sword 3 | X: 20 Y: 03 | 00 01 14 19 0e 14 bd 25 | 02 44 00 00<br>02 44 02 00
0c94 | 3: Sword 3 | X: 20 Y: 04 | 00 01 14 17 0e 14 c7 1b | 01 cc 00 00<br>01 cc 02 00<br>01 cc 07 00
0c75 | 3: Sword 3 | X: 21 Y: 03 | 00 02 0c 15 0e 14 a9 1b | 02 b2 00 00<br>02 b2 02 00<br>02 b2 06 00<br>02 b2 08 00
0e40 | 3: Sword 3 | X: 00 Y: 18 | 00 01 06 09 0e 14 81 07 | 03 ac 14 00
0dc9 | 3: Sword 3 | X: 09 Y: 14 | 00 02 00 0c 06 14 c7 15 | 04 10 14 00
0def | 3: Sword 3 | X: 15 Y: 15 | 05 03 01 08 04 64 c8 20 | 06 4a 14 00
0d7c | 3: Sword 3 | X: 28 Y: 11 | 00 00 0b 0f 0e 14 81 1b | 01 b8 00 00<br>01 b8 02 00<br>01 b8 07 00
0db9 | 3: Sword 3 | X: 25 Y: 13 | 00 00 0c 0e 08 14 81 20 | 03 66 14 00
0dd8 | 3: Sword 3 | X: 24 Y: 14 | 00 01 07 0d 0e 14 c7 1b | 02 1c 14 00
0ddb | 3: Sword 3 | X: 27 Y: 14 | 00 00 08 0c 08 14 6d 25 | 03 48 14 00
0e19 | 3: Sword 3 | X: 25 Y: 16 | 00 00 03 0b 08 14 81 24 | 02 e4 14 00
0e33 | 3: Sword 3 | X: 19 Y: 17 | 00 02 04 0c 0e 14 63 15 | 02 44 00 00<br>02 44 02 00
0d7e | 3: Sword 3 | X: 30 Y: 11 | 00 01 00 0f 04 14 81 1b | 04 b0 14 00
14e1 | 5: Shield  | X: 01 Y: 07 | 00 00 08 0f 00 01 63 0a | 00 1e 14 00
1408 | 5: Shield  | X: 08 Y: 00 | 00 03 09 0d 00 01 63 0c | 00 28 14 00
1434 | 5: Shield  | X: 20 Y: 01 | 00 00 00 0f 00 01 8b 0a | 00 32 00 00<br>00 32 02 00
1438 | 5: Shield  | X: 24 Y: 01 | 00 01 02 0f 00 01 8b 0a | 00 32 00 00<br>00 32 02 00
145a | 5: Shield  | X: 26 Y: 02 | 00 02 11 14 03 01 3c 0f | 00 8c 14 00
14b1 | 5: Shield  | X: 17 Y: 05 | 00 02 0c 0d 00 01 77 0c | 00 32 00 00<br>00 32 02 00<br>00 32 07 00
14b0 | 5: Shield  | X: 16 Y: 05 | 00 03 07 0d 00 01 77 0d | 00 32 00 00<br>00 32 02 00<br>00 32 07 00
14ac | 5: Shield  | X: 12 Y: 05 | 00 00 06 0e 01 01 96 0f | 00 50 00 00<br>00 50 02 00<br>00 50 06 00<br>00 50 08 00
1551 | 5: Shield  | X: 17 Y: 10 | 00 03 00 08 00 01 81 0c | 00 32 00 00<br>00 32 02 00<br>00 32 07 00
1550 | 5: Shield  | X: 16 Y: 10 | 00 00 02 09 00 01 8b 0f | 00 3c 00 00<br>00 3c 02 00
1528 | 5: Shield  | X: 08 Y: 09 | 00 01 04 0c 00 01 b3 09 | 00 3c 00 00<br>00 3c 02 00<br>00 3c 06 00<br>00 3c 08 00
1569 | 5: Shield  | X: 09 Y: 11 | 00 03 05 0c 00 01 b3 0b | 00 3c 00 00<br>00 3c 02 00<br>00 3c 06 00<br>00 3c 08 00
1541 | 5: Shield  | X: 01 Y: 10 | 00 02 00 0c 00 01 b3 0a | 00 3c 00 00<br>00 3c 02 00<br>00 3c 06 00<br>00 3c 08 00
1562 | 5: Shield  | X: 02 Y: 11 | 00 03 02 0c 01 01 b3 0a | 00 5a 00 00<br>00 5a 02 00<br>00 5a 06 00<br>00 5a 08 00
1590 | 5: Shield  | X: 16 Y: 12 | 00 01 06 0c 00 01 4f 0f | 00 96 00 00<br>00 96 02 00<br>00 96 06 00<br>00 96 08 00
15b3 | 5: Shield  | X: 19 Y: 13 | 00 00 07 0b 00 01 77 0d | 00 8c 00 00<br>00 8c 02 00<br>00 8c 06 00<br>00 8c 08 00
15b4 | 5: Shield  | X: 20 Y: 13 | 00 00 03 0c 00 01 77 13 | 00 8c 00 00<br>00 8c 02 00<br>00 8c 06 00<br>00 8c 08 00
1574 | 5: Shield  | X: 20 Y: 11 | 00 02 03 08 03 01 95 10 | 00 c8 00 00<br>00 c8 02 00<br>00 c8 06 00<br>00 c8 08 00
1515 | 5: Shield  | X: 21 Y: 08 | 00 01 00 09 03 01 b3 10 | 00 c8 00 00<br>00 c8 02 00
14fa | 5: Shield  | X: 26 Y: 07 | 00 02 01 09 03 01 b3 10 | 00 c8 00 00<br>00 c8 02 00<br>00 c8 06 00<br>00 c8 08 00
151c | 5: Shield  | X: 28 Y: 08 | 00 03 06 09 03 01 b3 10 | 01 04 00 00<br>01 04 02 00<br>01 04 06 00<br>01 04 08 00
1517 | 5: Shield  | X: 23 Y: 08 | 00 03 03 04 21 01 77 0a | 00 46 14 00
1556 | 5: Shield  | X: 22 Y: 10 | 00 00 03 05 21 01 b3 0b | 00 50 14 00
151f | 5: Shield  | X: 31 Y: 08 | 00 03 04 06 21 01 77 0c | 00 50 00 00<br>00 50 02 00
149d | 5: Shield  | X: 29 Y: 04 | 00 03 01 05 21 01 77 10 | 00 3c 00 00<br>00 3c 02 00<br>00 3c 07 00
149f | 5: Shield  | X: 31 Y: 04 | 00 01 01 04 21 01 77 10 | 00 46 00 00<br>00 46 02 00
1658 | 5: Shield  | X: 24 Y: 18 | 00 02 0b 0c 00 01 81 10 | 00 4b 00 00<br>00 4b 02 00<br>00 4b 06 00<br>00 4b 08 00
1659 | 5: Shield  | X: 25 Y: 18 | 00 03 06 0d 00 01 8b 12 | 00 55 00 00<br>00 55 02 00<br>00 55 06 00<br>00 55 08 00
1679 | 5: Shield  | X: 25 Y: 19 | 00 02 09 0e 03 01 95 12 | 00 5f 00 00<br>00 5f 02 00<br>00 5f 06 00<br>00 5f 08 00
167a | 5: Shield  | X: 26 Y: 19 | 00 02 04 0f 00 01 9f 12 | 00 73 00 00<br>00 73 02 00<br>00 73 06 00<br>00 73 08 00
169a | 5: Shield  | X: 26 Y: 20 | 00 00 07 10 03 01 a9 12 | 00 7d 00 00<br>00 7d 02 00<br>00 7d 06 00<br>00 7d 08 00
163a | 5: Shield  | X: 26 Y: 17 | 00 03 00 0d 01 01 a9 15 | 00 e1 00 00<br>00 e1 02 00<br>00 e1 06 00<br>00 e1 08 00
1698 | 5: Shield  | X: 24 Y: 20 | 00 00 02 0d 01 01 a9 15 | 00 e1 00 00<br>00 e1 02 00<br>00 e1 06 00<br>00 e1 08 00
169f | 5: Shield  | X: 31 Y: 20 | 00 01 0b 0d 03 01 8b 13 | 00 87 00 00<br>00 87 02 00<br>00 87 06 00<br>00 87 08 00
16bf | 5: Shield  | X: 31 Y: 21 | 00 01 0b 0c 03 01 81 15 | 00 eb 00 00<br>00 eb 02 00<br>00 eb 06 00<br>00 eb 08 00
16ff | 5: Shield  | X: 31 Y: 23 | 00 02 04 08 06 01 95 19 | 03 a7 14 00
179e | 5: Shield  | X: 30 Y: 28 | 82 01 04 0f 03 9b cd 0a | 03 20 14 00
118f | 4: Cup     | X: 15 Y: 12 | 00 00 07 0c 12 01 4f 0a | 00 87 00 00<br>00 87 02 00<br>00 87 07 00
11af | 4: Cup     | X: 15 Y: 13 | 00 00 02 0a 13 01 4f 0d | 00 87 00 00<br>00 87 02 00
11ae | 4: Cup     | X: 14 Y: 13 | 00 02 04 09 14 01 81 11 | 00 d7 00 00<br>00 d7 02 00<br>00 d7 07 00
1124 | 4: Cup     | X: 04 Y: 09 | 00 01 00 09 15 01 8b 0e | 01 1d 14 00
10e4 | 4: Cup     | X: 04 Y: 07 | 00 02 02 0c 03 01 4f 0d | 00 87 00 00<br>00 87 02 00
10a4 | 4: Cup     | X: 04 Y: 05 | 00 03 09 0c 01 01 4f 0b | 00 b9 00 00<br>00 b9 02 00<br>00 b9 06 00<br>00 b9 08 00
1080 | 4: Cup     | X: 00 Y: 04 | 00 03 08 0c 03 01 4f 16 | 00 7d 00 00<br>00 7d 02 00
10a0 | 4: Cup     | X: 00 Y: 05 | 00 03 04 0c 03 01 4f 15 | 00 af 00 00<br>00 af 02 00<br>00 af 07 00
1028 | 4: Cup     | X: 08 Y: 01 | 00 01 04 0a 14 01 8b 11 | 00 69 00 00<br>00 69 02 00<br>00 69 06 00<br>00 69 08 00
10b1 | 4: Cup     | X: 17 Y: 05 | 00 00 00 06 17 01 63 16 | 01 63 14 00
1016 | 4: Cup     | X: 22 Y: 00 | 00 01 01 07 17 01 4f 16 | 01 63 14 00
101d | 4: Cup     | X: 29 Y: 00 | 00 02 09 0c 12 01 4f 0e | 00 c3 00 00<br>00 c3 02 00<br>00 c3 06 00<br>00 c3 08 00
103e | 4: Cup     | X: 30 Y: 01 | 00 02 0a 0c 13 01 81 0a | 00 b9 00 00<br>00 b9 02 00<br>00 b9 06 00<br>00 b9 08 00
109f | 4: Cup     | X: 31 Y: 04 | 09 03 04 09 14 01 81 10 | 00 eb 00 00<br>00 eb 02 00<br>00 eb 06 00<br>00 eb 08 00
117d | 4: Cup     | X: 29 Y: 11 | 00 02 03 09 03 01 77 19 | 00 cd 00 00<br>00 cd 02 00<br>00 cd 06 00<br>00 cd 08 00
11b9 | 4: Cup     | X: 25 Y: 13 | 00 00 03 07 17 01 4f 10 | 01 8b 14 00
1217 | 4: Cup     | X: 23 Y: 16 | 00 03 00 06 17 01 6d 12 | 01 3b 14 00
1212 | 4: Cup     | X: 18 Y: 16 | 00 00 00 06 17 01 81 13 | 01 45 14 00
1233 | 4: Cup     | X: 19 Y: 17 | 00 01 04 05 17 01 77 15 | 01 6d 14 00
1273 | 4: Cup     | X: 19 Y: 19 | 00 01 16 18 03 01 81 1a | 01 ef 14 00
129a | 4: Cup     | X: 26 Y: 20 | 00 02 04 08 15 01 4f 14 | 01 81 14 00
12bc | 4: Cup     | X: 28 Y: 21 | 00 01 06 09 15 01 9f 11 | 01 31 14 00
129f | 4: Cup     | X: 31 Y: 20 | 0a 01 01 05 17 01 a9 18 | 01 b3 14 00
137a | 4: Cup     | X: 26 Y: 27 | 0b 03 03 09 16 be a9 15 | 00 e1 14 00
120a | 4: Cup     | X: 10 Y: 16 | 09 02 00 05 17 01 b3 13 | 01 b3 14 00
1393 | 4: Cup     | X: 19 Y: 28 | 00 03 02 0c 01 64 4f 0f | 00 87 00 00<br>00 87 02 00<br>00 87 06 00<br>00 87 08 00
13f1 | 4: Cup     | X: 17 Y: 31 | 00 00 08 0b 01 64 77 11 | 00 9b 00 00<br>00 9b 02 00<br>00 9b 06 00<br>00 9b 08 00
13ec | 4: Cup     | X: 12 Y: 31 | 00 00 08 0a 01 64 81 13 | 00 a5 00 00<br>00 a5 02 00<br>00 a5 06 00<br>00 a5 08 00
136c | 4: Cup     | X: 12 Y: 27 | 00 01 05 09 01 64 8b 15 | 00 af 00 00<br>00 af 02 00<br>00 af 06 00<br>00 af 08 00
13a9 | 4: Cup     | X: 09 Y: 29 | 09 00 04 06 17 01 b3 14 | 01 b3 14 00
1362 | 4: Cup     | X: 02 Y: 27 | 00 00 01 04 1d 01 77 0e | 00 87 00 00<br>00 87 02 00
13a5 | 4: Cup     | X: 05 Y: 29 | 00 02 03 08 16 be c7 1b | 02 17 14 00
1301 | 4: Cup     | X: 01 Y: 24 | 00 01 00 09 15 96 b3 19 | 01 b3 14 00
13c0 | 4: Cup     | X: 00 Y: 30 | 00 02 00 04 1d 01 77 13 | 00 87 00 00<br>00 87 02 00<br>00 87 06 00<br>00 87 08 00
13e2 | 4: Cup     | X: 02 Y: 31 | 00 03 04 05 1d 01 81 14 | 00 87 00 00<br>00 87 02 00<br>00 87 06 00<br>00 87 08 00
13e4 | 4: Cup     | X: 04 Y: 31 | 00 03 03 04 1d 01 8b 16 | 00 87 00 00<br>00 87 02 00<br>00 87 06 00<br>00 87 08 00
13a3 | 4: Cup     | X: 03 Y: 29 | 0c 00 02 04 17 01 9f 15 | 02 17 14 00
12c0 | 4: Cup     | X: 00 Y: 22 | 00 03 0e 14 1f 01 c7 25 | 08 bb 14 00
1c8b | 7: Crown 1 | X: 11 Y: 04 | 00 03 05 0b 1f 6e 6d 14 | 03 a7 14 00
1c4d | 7: Crown 1 | X: 13 Y: 02 | 00 02 08 0b 1f 6e 81 14 | 03 a7 14 00
1c8f | 7: Crown 1 | X: 15 Y: 04 | 00 02 03 0b 1f 6e 95 1e | 04 6f 14 00
1c51 | 7: Crown 1 | X: 17 Y: 02 | 00 00 04 0b 1f 6e 6d 19 | 05 37 14 00
1c93 | 7: Crown 1 | X: 19 Y: 04 | 00 03 00 0b 1f 6e 8b 1e | 04 d3 14 00
1c35 | 7: Crown 1 | X: 21 Y: 01 | 00 00 01 09 1f 6e bd 14 | 03 a7 14 00
1c7a | 7: Crown 1 | X: 26 Y: 03 | 93 01 07 09 10 6e 6d 28 | 0d 6b 14 00
05c1 | 1: Sword 1 | X: 01 Y: 14 | 14 01 06 0a 07 be 64 1e | 05 dc 14 00
1d7d | 7: Crown 1 | X: 29 Y: 11 | 00 02 01 09 1f 6e bd 14 | 03 a7 14 00
1cd9 | 7: Crown 1 | X: 25 Y: 06 | 00 03 01 09 1f 6e bd 1e | 04 6f 14 00
1cda | 7: Crown 1 | X: 26 Y: 06 | 00 00 07 09 1f 6e bd 1e | 00 eb 14 00
1cb9 | 7: Crown 1 | X: 25 Y: 05 | 00 00 07 09 1f 6e bd 1e | 04 6f 14 00
1cdb | 7: Crown 1 | X: 27 Y: 06 | 00 01 04 09 1f 6e bd 1e | 04 6f 14 00
1610 | 5: Shield  | X: 16 Y: 16 | 95 00 03 0c 22 6e bd 28 | 00 87 14 00
1651 | 5: Shield  | X: 17 Y: 18 | 00 02 05 0b 0a 6f bd 1e | 05 ff 14 00
1691 | 5: Shield  | X: 17 Y: 20 | 00 01 00 0a 0a 8d bd 23 | 05 9b 14 00
16d1 | 5: Shield  | X: 17 Y: 22 | 00 02 01 09 0a 8d bd 24 | 05 ff 14 00
1cf2 | 7: Crown 1 | X: 18 Y: 07 | 16 03 09 0c 11 6e db 28 | 04 6f 14 00
1d71 | 7: Crown 1 | X: 17 Y: 11 | 00 03 0f 12 11 6e bd 1e | 03 a7 00 00<br>03 a7 02 00
1d72 | 7: Crown 1 | X: 18 Y: 11 | 00 00 0b 13 11 6e 9f 1e | 04 6f 00 00<br>04 6f 02 00
1d73 | 7: Crown 1 | X: 19 Y: 11 | 00 03 0c 14 11 6e c7 1e | 04 6f 00 00<br>04 6f 02 00<br>04 6f 07 00
1e30 | 7: Crown 1 | X: 16 Y: 17 | 00 03 03 0a 20 6e 01 1e | 02 df 00 00<br>02 df 02 00<br>02 df 07 00
1df0 | 7: Crown 1 | X: 16 Y: 15 | 00 01 04 0a 20 6e 01 1e | 02 df 00 00<br>02 df 02 00<br>02 df 07 00
1e2e | 7: Crown 1 | X: 14 Y: 17 | 00 00 01 0a 20 6e 01 1e | 02 df 00 00<br>02 df 02 00<br>02 df 07 00
1e73 | 7: Crown 1 | X: 19 Y: 19 | 00 01 03 18 11 6e d2 1e | 04 6f 00 00<br>04 6f 02 00<br>04 6f 07 00
1efa | 7: Crown 1 | X: 26 Y: 23 | 00 02 0d 0f 1b 96 6e 26 | 03 a7 00 00<br>03 a7 02 00<br>03 a7 07 00
1f3b | 7: Crown 1 | X: 27 Y: 25 | 00 03 09 0f 1b 96 64 26 | 02 17 00 00<br>02 17 02 00<br>02 17 07 00
1f5a | 7: Crown 1 | X: 26 Y: 26 | 00 02 09 0d 1b 96 78 26 | 03 43 00 00<br>03 43 02 00
1f7b | 7: Crown 1 | X: 27 Y: 27 | 00 02 04 0c 1b 96 5a 26 | 03 43 00 00<br>03 43 02 00<br>03 43 06 00<br>03 43 08 00
1f7e | 7: Crown 1 | X: 30 Y: 27 | 17 00 0c 1c 09 96 5a 50 | 0c a3 14 00
1ea8 | 7: Crown 1 | X: 08 Y: 21 | 00 03 01 16 11 be 5a 33 | 04 6f 00 00<br>04 6f 02 00
1f2e | 7: Crown 1 | X: 14 Y: 25 | 00 00 03 18 11 be 5a 3d | 04 6f 00 00<br>04 6f 02 00
1faa | 7: Crown 1 | X: 10 Y: 29 | 00 01 12 17 11 be 5a 33 | 04 6f 00 00<br>04 6f 02 00
1f52 | 7: Crown 1 | X: 18 Y: 26 | 00 01 15 18 10 d2 5a 4f | 08 57 14 00
1f9c | 7: Crown 1 | X: 28 Y: 28 | 00 02 07 0e 1f 46 6e 27 | 04 6f 14 00
1fbc | 7: Crown 1 | X: 28 Y: 29 | 00 01 09 0e 1f 46 6e 27 | 04 6f 14 00
1fbd | 7: Crown 1 | X: 29 Y: 29 | 00 01 04 0d 1f 64 78 1f | 04 0b 14 00
1fbe | 7: Crown 1 | X: 30 Y: 29 | 00 03 05 0c 1f 6e 82 24 | 02 7b 14 00
1fbf | 7: Crown 1 | X: 31 Y: 29 | 00 02 01 0b 1f 78 6e 1f | 02 df 14 00
1fdc | 7: Crown 1 | X: 28 Y: 30 | 00 03 01 09 1f 5a 6e 15 | 03 a7 14 00
1fdd | 7: Crown 1 | X: 29 Y: 30 | 00 00 06 08 1f 5a 6e 1f | 04 0b 14 00
1fde | 7: Crown 1 | X: 30 Y: 30 | 00 00 06 07 1f 5a 0a 15 | 03 43 14 00
1ffc | 7: Crown 1 | X: 28 Y: 31 | 00 01 03 06 1f 5a 6e 15 | 03 a7 14 00
1ffd | 7: Crown 1 | X: 29 Y: 31 | 00 00 03 05 1f 5a 5b 1f | 02 7b 14 00
1ffe | 7: Crown 1 | X: 30 Y: 31 | 00 00 01 04 1f 5a 5a 1f | 04 0b 14 00
1fff | 7: Crown 1 | X: 31 Y: 31 | 00 02 01 03 1f 64 5a 1f | 02 df 14 00
1fdf | 7: Crown 1 | X: 31 Y: 30 | 18 01 00 18 10 be be 1f | 0c 3f 14 00
1220 | 4: Cup     | X: 00 Y: 17 | 19 02 00 06 0a 7d 14 1f | 03 84 14 00
1265 | 4: Cup     | X: 05 Y: 19 | 00 03 07 09 0a 7d 14 1f | 03 20 14 00
12c2 | 4: Cup     | X: 02 Y: 22 | 00 03 06 07 0a 7d 14 1f | 02 bc 14 00
1e86 | 7: Crown 1 | X: 06 Y: 20 | 00 03 11 18 11 be 5a 27 | 05 37 14 00
1e82 | 7: Crown 1 | X: 02 Y: 20 | 1a 03 06 18 11 be 5a 27 | 05 9b 14 00
1f45 | 7: Crown 1 | X: 05 Y: 26 | 00 01 0a 18 10 be be 3d | 04 6f 14 00
1fa7 | 7: Crown 1 | X: 07 Y: 29 | 00 00 02 18 10 be be 3d | 04 6f 14 00
1fe7 | 7: Crown 1 | X: 07 Y: 31 | 00 01 05 18 10 be be 3d | 04 6f 14 00
1ad6 | 6: Crown 2 | X: 22 Y: 22 | 9b 01 10 13 07 64 be 45 | 07 f3 14 00
1add | 6: Crown 2 | X: 29 Y: 22 | 1c 02 07 0c 07 64 be 45 | 0d 6b 14 00
1afa | 6: Crown 2 | X: 26 Y: 23 | 00 01 0c 12 07 64 be 45 | 06 63 14 00
1b1f | 6: Crown 2 | X: 31 Y: 24 | 00 01 13 42 07 64 be 45 | 06 63 14 00
18a8 | 6: Crown 2 | X: 08 Y: 05 | 1d 03 04 0c 07 64 be 45 | 09 83 14 00
18d1 | 6: Crown 2 | X: 17 Y: 06 | 1d 02 01 0c 07 64 be 45 | 09 83 14 00
186d | 6: Crown 2 | X: 13 Y: 03 | 1e 03 00 07 07 64 be 45 | 08 57 14 00
1847 | 6: Crown 2 | X: 07 Y: 02 | 00 00 08 0a 1b 0a be 3b | 03 a7 00 00<br>03 a7 02 00<br>03 a7 07 00
1903 | 6: Crown 2 | X: 03 Y: 08 | 00 00 08 09 1b 0a be 31 | 03 a7 00 00<br>03 a7 02 00
18e8 | 6: Crown 2 | X: 08 Y: 07 | 00 01 06 0b 1b 0a be 3b | 03 a7 00 00<br>03 a7 02 00
1884 | 6: Crown 2 | X: 04 Y: 04 | 00 00 08 0c 1b 0a be 31 | 03 a7 00 00<br>03 a7 02 00
1886 | 6: Crown 2 | X: 06 Y: 04 | 00 00 03 0c 1b 0a be 31 | 03 a7 00 00<br>03 a7 02 00<br>03 a7 06 00<br>03 a7 08 00
188b | 6: Crown 2 | X: 11 Y: 04 | 00 02 06 12 09 01 d2 59 | 07 8f 14 00
18ac | 6: Crown 2 | X: 12 Y: 05 | 00 01 02 12 09 01 d2 59 | 07 8f 14 00
1911 | 6: Crown 2 | X: 17 Y: 08 | 00 02 03 12 09 01 d2 59 | 0b 77 14 00
194b | 6: Crown 2 | X: 11 Y: 10 | 00 03 0c 0e 09 01 d2 59 | 08 57 14 00
196b | 6: Crown 2 | X: 11 Y: 11 | 00 03 0a 0b 09 01 d2 59 | 07 8f 14 00
18ef | 6: Crown 2 | X: 15 Y: 07 | 00 00 06 0b 1c 01 64 27 | 03 a7 14 00
1833 | 6: Crown 2 | X: 19 Y: 01 | 00 03 06 09 07 64 dc 4f | 0c 3f 14 00
183b | 6: Crown 2 | X: 27 Y: 01 | 00 03 02 09 07 64 dc 4f | 0c 3f 14 00
1933 | 6: Crown 2 | X: 19 Y: 09 | 00 01 03 09 07 64 dc 4f | 0c 3f 14 00
193b | 6: Crown 2 | X: 27 Y: 09 | 00 00 00 09 07 64 dc 4f | 0c 3f 14 00
183e | 6: Crown 2 | X: 30 Y: 01 | 00 01 02 0a 1c 64 78 27 | 04 0b 14 00
1817 | 6: Crown 2 | X: 23 Y: 00 | 00 02 08 0a 1c 64 78 27 | 04 0b 14 00
1805 | 6: Crown 2 | X: 05 Y: 00 | 00 02 09 0a 1c 64 78 27 | 00 87 14 00
18a0 | 6: Crown 2 | X: 00 Y: 05 | 00 03 05 0a 1c 64 78 27 | 04 0b 14 00
197d | 6: Crown 2 | X: 29 Y: 11 | 00 02 04 06 19 01 c8 1d | 01 4f 14 00
199c | 6: Crown 2 | X: 28 Y: 12 | 00 02 02 07 19 01 c8 1d | 01 4f 14 00
19ae | 6: Crown 2 | X: 14 Y: 13 | 00 00 03 08 19 01 c8 1d | 00 23 14 00
19ce | 6: Crown 2 | X: 14 Y: 14 | 00 03 00 09 19 01 c8 1d | 00 23 14 00
19ef | 6: Crown 2 | X: 15 Y: 15 | 00 00 02 0a 09 01 c8 59 | 09 1f 14 00
180a | 6: Crown 2 | X: 10 Y: 00 | 00 01 06 08 19 01 c8 1d | 02 df 14 00
186f | 6: Crown 2 | X: 15 Y: 03 | 00 01 07 08 07 dd c8 45 | 00 eb 14 00
0685 | 1: Sword 1 | X: 05 Y: 20 | 12 01 06 13 09 55 64 79 | 05 14 14 00
0600 | 1: Sword 1 | X: 00 Y: 16 | 12 03 08 12 09 55 64 b5 | 05 dc 14 00
0719 | 1: Sword 1 | X: 25 Y: 24 | 12 02 01 11 09 55 64 b5 | 05 14 14 00
074e | 1: Sword 1 | X: 14 Y: 26 | 12 03 02 10 09 55 64 bf | 05 dc 14 00
0662 | 1: Sword 1 | X: 02 Y: 19 | 00 00 0a 0c 07 55 50 1f | 01 2c 14 00
0664 | 1: Sword 1 | X: 04 Y: 19 | 00 00 0b 0d 07 55 50 1f | 00 c8 14 00
06c6 | 1: Sword 1 | X: 06 Y: 22 | 00 01 07 0e 07 55 50 1f | 01 2c 14 00
06e7 | 1: Sword 1 | X: 07 Y: 23 | 00 00 0a 0f 07 55 50 1f | 00 c8 14 00
0723 | 1: Sword 1 | X: 03 Y: 25 | 00 00 04 0e 07 55 50 1f | 01 2c 14 00
0721 | 1: Sword 1 | X: 01 Y: 25 | 00 02 05 0d 07 55 50 1f | 01 2c 14 00
0680 | 1: Sword 1 | X: 00 Y: 20 | 00 01 01 0c 07 55 50 1f | 00 c8 14 00
06c0 | 1: Sword 1 | X: 00 Y: 22 | 00 02 02 0c 07 55 50 1f | 01 2c 14 00
0e1f | 3: Sword 3 | X: 31 Y: 16 | 00 03 0b 0c 0a 55 50 29 | 02 58 14 00
0fce | 3: Sword 3 | X: 14 Y: 30 | 00 03 0e 14 0b 01 f0 14 | 03 20 14 00
0faf | 3: Sword 3 | X: 15 Y: 29 | 00 03 06 14 0b 01 f0 1e | 03 20 14 00
0fd0 | 3: Sword 3 | X: 16 Y: 30 | 00 01 08 14 0b 01 f0 1e | 07 08 14 00
0f72 | 3: Sword 3 | X: 18 Y: 27 | 00 00 02 14 0b 01 f0 28 | 03 84 14 00
0f93 | 3: Sword 3 | X: 19 Y: 28 | 00 01 02 14 0b 01 f0 28 | 03 20 14 00
0f4e | 3: Sword 3 | X: 14 Y: 26 | 00 02 10 14 0b 01 f0 32 | 03 e8 14 00
0f6d | 3: Sword 3 | X: 13 Y: 27 | 00 02 13 14 0b 01 f0 32 | 04 4c 14 00
0f4c | 3: Sword 3 | X: 12 Y: 26 | 00 03 0b 14 0b 01 f0 32 | 04 b0 14 00
0f2d | 3: Sword 3 | X: 13 Y: 25 | 00 02 0c 14 0b 01 f0 32 | 05 14 14 00
0f0c | 3: Sword 3 | X: 12 Y: 24 | 00 02 07 14 0b 01 f0 32 | 04 b0 14 00
0f2a | 3: Sword 3 | X: 10 Y: 25 | a1 00 08 14 22 01 d2 32 | 00 64 14 00
0f6a | 3: Sword 3 | X: 10 Y: 27 | 9f 03 02 14 23 01 d2 32 | 00 64 14 00
0cf7 | 3: Sword 3 | X: 23 Y: 07 | 00 00 00 03 21 01 ab 19 | 00 c8 00 00<br>00 c8 02 00<br>00 c8 06 00<br>00 c8 08 00
0d18 | 3: Sword 3 | X: 24 Y: 08 | 00 01 03 04 21 01 ab 19 | 00 c8 00 00<br>00 c8 02 00<br>00 c8 06 00<br>00 c8 08 00
0d37 | 3: Sword 3 | X: 23 Y: 09 | 00 01 04 05 21 01 ab 19 | 00 c8 00 00<br>00 c8 02 00<br>00 c8 06 00<br>00 c8 08 00
0c37 | 3: Sword 3 | X: 23 Y: 01 | 00 03 05 0c 0d 01 db 1f | 01 a9 00 00<br>01 a9 02 00<br>01 a9 06 00<br>01 a9 08 00
0c7f | 3: Sword 3 | X: 31 Y: 03 | 11 02 02 14 0c 01 4f 33 | 02 ad 14 00
0cb6 | 3: Sword 3 | X: 22 Y: 05 | 11 03 02 0a 0c 01 db 29 | 03 20 14 00
05f5 | 1: Sword 1 | X: 21 Y: 15 | 00 03 03 04 17 5f 96 1f | 01 90 14 00
0698 | 1: Sword 1 | X: 24 Y: 20 | 00 03 02 03 17 5f 96 1f | 01 f4 14 00
06d7 | 1: Sword 1 | X: 23 Y: 22 | 00 00 02 05 17 5f 6e 1f | 01 90 14 00
0658 | 1: Sword 1 | X: 24 Y: 18 | 00 03 01 03 17 37 96 1f | 01 f4 14 00
06f2 | 1: Sword 1 | X: 18 Y: 23 | 00 03 01 04 17 37 96 1f | 02 58 14 00
0b40 | 2: Sword 2 | X: 00 Y: 26 | 10 02 04 05 10 55 64 47 | 05 dc 14 00
0b16 | 2: Sword 2 | X: 22 Y: 24 | 00 03 02 05 1b 55 aa 15 | 01 2c 00 00<br>01 2c 02 00
0b17 | 2: Sword 2 | X: 23 Y: 24 | 00 02 03 05 1b 55 aa 15 | 01 90 00 00<br>01 90 02 00
0a53 | 2: Sword 2 | X: 19 Y: 18 | 00 02 01 05 1b 55 c8 1f | 01 2c 00 00<br>01 2c 02 00<br>01 2c 07 00
0b13 | 2: Sword 2 | X: 19 Y: 24 | 00 00 01 05 1b 55 c8 29 | 01 2c 00 00<br>01 2c 02 00<br>01 2c 07 00
0a91 | 2: Sword 2 | X: 17 Y: 20 | 00 03 00 05 1b 55 c8 33 | 01 54 00 00<br>01 54 02 00<br>01 54 06 00<br>01 54 08 00
0b0d | 2: Sword 2 | X: 13 Y: 24 | 00 00 00 04 1b 55 d2 29 | 01 5e 00 00<br>01 5e 02 00<br>01 5e 06 00<br>01 5e 08 00
0a69 | 2: Sword 2 | X: 09 Y: 19 | 00 01 03 04 1b 55 d2 33 | 01 c2 00 00<br>01 c2 02 00<br>01 c2 06 00<br>01 c2 08 00
0a4c | 2: Sword 2 | X: 12 Y: 18 | 00 01 0e 0f 11 55 78 3d | 03 20 00 00<br>03 20 02 00<br>03 20 06 00<br>03 20 08 00
0a6c | 2: Sword 2 | X: 12 Y: 19 | 00 02 08 0f 11 55 78 3d | 02 bc 00 00<br>02 bc 02 00<br>02 bc 06 00<br>02 bc 08 00
0a60 | 2: Sword 2 | X: 00 Y: 19 | 00 01 06 09 19 55 5a 29 | 01 90 14 00
0a80 | 2: Sword 2 | X: 00 Y: 20 | 00 01 04 0c 11 55 78 1f | 02 58 00 00<br>02 58 02 00<br>02 58 06 00<br>02 58 08 00
0a7e | 2: Sword 2 | X: 30 Y: 19 | 00 03 02 05 1c 55 6e 0b | 01 18 14 00
0abd | 2: Sword 2 | X: 29 Y: 21 | 00 02 00 05 1c 55 78 0b | 01 18 14 00
0b60 | 2: Sword 2 | X: 00 Y: 27 | 00 03 01 08 1b 55 78 3d | 01 90 00 00<br>01 90 02 00<br>01 90 06 00<br>01 90 08 00
0b65 | 2: Sword 2 | X: 05 Y: 27 | 00 00 07 09 1b 55 78 3d | 01 90 00 00<br>01 90 02 00
0bc0 | 2: Sword 2 | X: 00 Y: 30 | 00 00 08 09 1b 55 78 3d | 01 90 00 00<br>01 90 02 00
0bc7 | 2: Sword 2 | X: 07 Y: 30 | 00 01 05 0b 11 55 78 47 | 04 b0 14 00
01fc | 0: Forest  | X: 28 Y: 15 | 00 00 07 0a 20 0a 64 1e | 01 2c 14 00
01b9 | 0: Forest  | X: 25 Y: 13 | 00 00 03 0a 20 0a 64 1e | 01 2c 14 00
019e | 0: Forest  | X: 30 Y: 12 | 00 02 04 0a 20 0a 64 1e | 01 2c 14 00
017c | 0: Forest  | X: 28 Y: 11 | 00 01 00 0a 20 0a 64 1e | 01 2c 14 00
007a | 0: Forest  | X: 26 Y: 03 | 00 03 0b 0c 20 01 77 1f | 00 eb 00 00<br>00 eb 02 00<br>00 eb 06 00<br>00 eb 08 00
001b | 0: Forest  | X: 27 Y: 00 | 00 00 06 0c 20 01 77 1f | 00 eb 00 00<br>00 eb 02 00<br>00 eb 06 00<br>00 eb 08 00
17d7 | 5: Shield  | X: 23 Y: 30 | 00 03 01 05 21 01 be 28 | 01 90 00 00<br>01 90 02 00<br>01 90 06 00<br>01 90 08 00
1771 | 5: Shield  | X: 17 Y: 27 | 00 01 02 05 21 01 be 28 | 01 90 00 00<br>01 90 02 00<br>01 90 06 00<br>01 90 08 00
174b | 5: Shield  | X: 11 Y: 26 | 00 00 00 05 21 01 be 28 | 01 f4 00 00<br>01 f4 02 00<br>01 f4 06 00<br>01 f4 08 00
172e | 5: Shield  | X: 14 Y: 25 | 00 01 03 0f 06 5a 6e 3c | 02 58 14 00
1735 | 5: Shield  | X: 21 Y: 25 | 00 02 0c 0f 06 5a 6e 3c | 02 58 14 00
16c0 | 5: Shield  | X: 00 Y: 22 | 00 02 08 0a 04 01 be 32 | 01 f4 14 00
1681 | 5: Shield  | X: 01 Y: 20 | 00 03 04 08 04 01 be 32 | 01 f4 14 00
1680 | 5: Shield  | X: 00 Y: 20 | 00 02 06 09 04 01 be 32 | 01 f4 14 00
1660 | 5: Shield  | X: 00 Y: 19 | 20 02 02 07 04 01 be 3c | 05 dc 14 00
1a48 | 6: Crown 2 | X: 08 Y: 18 | 00 00 07 11 19 01 dc 28 | 01 f4 00 00<br>01 f4 01 00<br>01 f4 02 00<br>01 f4 03 00<br>01 f4 04 00<br>01 f4 05 00<br>01 f4 06 00<br>01 f4 07 00<br>01 f4 08 00
1a8b | 6: Crown 2 | X: 11 Y: 20 | 00 03 00 09 1e c8 d2 50 | 03 e8 14 00
1b90 | 6: Crown 2 | X: 16 Y: 28 | 00 00 01 08 1e c8 d2 50 | 03 e8 14 00
1b2c | 6: Crown 2 | X: 12 Y: 25 | 00 01 04 06 1e c8 d2 50 | 04 b0 14 00
1ac9 | 6: Crown 2 | X: 09 Y: 22 | 00 01 05 06 1e c8 d2 50 | 05 14 14 00
1b08 | 6: Crown 2 | X: 08 Y: 24 | 00 02 03 06 1e c8 d2 50 | 05 78 14 00
078e | 1: Sword 1 | X: 14 Y: 28 | 00 01 04 06 1d 01 d2 28 | 01 90 00 00<br>01 90 02 00<br>01 90 06 00<br>01 90 08 00
1d41 | 7: Crown 1 | X: 01 Y: 10 | 00 03 02 06 1e c8 d2 50 | 01 90 14 00
1d42 | 7: Crown 1 | X: 02 Y: 10 | 00 02 00 06 1e c8 d2 50 | 01 2c 14 00
1d60 | 7: Crown 1 | X: 00 Y: 11 | 00 03 01 06 1e b4 d2 50 | 03 20 14 00
1d61 | 7: Crown 1 | X: 01 Y: 11 | 00 00 05 07 1e aa d2 50 | 03 20 14 00
1d62 | 7: Crown 1 | X: 02 Y: 11 | 00 00 05 06 1e aa d2 50 | 02 bc 14 00
1d81 | 7: Crown 1 | X: 01 Y: 12 | 00 01 03 06 1e aa d2 50 | 02 58 14 00
1d82 | 7: Crown 1 | X: 02 Y: 12 | 00 00 05 07 1e aa d2 50 | 02 bc 14 00
1da3 | 7: Crown 1 | X: 03 Y: 13 | 00 00 01 06 1e aa d2 50 | 02 58 14 00
1dc1 | 7: Crown 1 | X: 01 Y: 14 | 00 02 02 06 1e aa d2 50 | 01 f4 14 00
1dc2 | 7: Crown 1 | X: 02 Y: 14 | 00 01 00 07 1e c8 d2 50 | 01 f4 14 00
0bae | 2: Sword 2 | X: 14 Y: 29 | 22 02 00 05 1e d2 c8 64 | 08 34 14 00
0be9 | 2: Sword 2 | X: 09 Y: 31 | 23 03 06 08 1a dc dc c8 | 10 04 14 00

### $5de2: Items

A list of 637 four-byte entries. Each defines either a bundle of items at a
given location, or an item in that set.

Set | Locat | Map        | Coords    | Item name             | ID | Corner
----|-------|------------|-----------|-----------------------|----|---
  0 | $03ee | 0: Forest  | X:14 Y:31 |  SANDLES              | 1b | 1
  0 | $03ee | 0: Forest  | X:14 Y:31 |  SHORTS               | 13 | 1
  0 | $03ee | 0: Forest  | X:14 Y:31 |  SHORTS               | 13 | 1
  0 | $03ee | 0: Forest  | X:14 Y:31 |  SHORTS               | 13 | 1
  0 | $03ee | 0: Forest  | X:14 Y:31 |  T-SHIRT              | 09 | 1
  0 | $03ee | 0: Forest  | X:14 Y:31 |  T-SHIRT              | 09 | 2
  1 | $0024 | 0: Forest  | X:04 Y:01 |  SWEET                | 22 | 3
  2 | $00e9 | 0: Forest  | X:09 Y:07 |  SHORTS               | 13 | 2
  3 | $002f | 0: Forest  | X:15 Y:01 |  TWIG                 | 47 | 3
  4 | $0075 | 0: Forest  | X:21 Y:03 |  PENKNIFE             | 30 | 1
  5 | $0263 | 0: Forest  | X:03 Y:19 |  T-SHIRT              | 09 | 1
  6 | $0317 | 0: Forest  | X:23 Y:24 |  SPADE                | 48 | 0
  7 | $0241 | 0: Forest  | X:01 Y:18 |  SWEET                | 22 | 0
  8 | $0249 | 0: Forest  | X:09 Y:18 |  SANDLES              | 1b | 0
  9 | $00c6 | 0: Forest  | X:06 Y:06 |  SWEET                | 22 | 1
 10 | $01f5 | 0: Forest  | X:21 Y:15 |  SWEET                | 22 | 1
 11 | $026f | 0: Forest  | X:15 Y:19 |  SWEET                | 22 | 1
 12 | $02af | 0: Forest  | X:15 Y:21 |  PENKNIFE             | 30 | 2
 13 | $0301 | 0: Forest  | X:01 Y:24 |  IRON KEY             | 5f | 3
 14 | $0483 | 1: Sword 1 | X:03 Y:04 |  GRAPES               | 27 | 3
 15 | $04a3 | 1: Sword 1 | X:03 Y:05 |  WAND OF MAGIC        | 4b | 3
 15 | $04a3 | 1: Sword 1 | X:03 Y:05 |  BOILED EGG           | 28 | 1
 16 | $0408 | 1: Sword 1 | X:08 Y:00 |  SHEATH               | 35 | 2
 17 | $0406 | 1: Sword 1 | X:06 Y:00 |  GOLD KEY             | 60 | 3
 18 | $044e | 1: Sword 1 | X:14 Y:02 |  BOILED EGG           | 28 | 1
 18 | $044e | 1: Sword 1 | X:14 Y:02 |  BOILED EGG           | 28 | 1
 19 | $040b | 1: Sword 1 | X:11 Y:00 |  BRONZE KEY           | 61 | 1
 20 | $0410 | 1: Sword 1 | X:16 Y:00 |  GOLD KEY             | 60 | 2
 21 | $0412 | 1: Sword 1 | X:18 Y:00 |  DENIM                | 0c | 2
 22 | $041e | 1: Sword 1 | X:30 Y:00 |  IRON KEY             | 5f | 2
 23 | $05e3 | 1: Sword 1 | X:03 Y:15 |  BOOTS                | 1e | 1
 24 | $04e3 | 1: Sword 1 | X:03 Y:07 |  IRON KEY             | 5f | 3
 25 | $058d | 1: Sword 1 | X:13 Y:12 |  VEST +1              | 0e | 1
 25 | $058d | 1: Sword 1 | X:13 Y:12 |  CRASH HELMET +1      | 04 | 2
 26 | $05e6 | 1: Sword 1 | X:06 Y:15 |  APPLE                | 24 | 0
 26 | $05e6 | 1: Sword 1 | X:06 Y:15 |  APPLE                | 24 | 0
 27 | $05e5 | 1: Sword 1 | X:05 Y:15 |  IRON KEY             | 5f | 1
 28 | $043f | 1: Sword 1 | X:31 Y:01 |  BRONZE KEY           | 61 | 0
 29 | $049b | 1: Sword 1 | X:27 Y:04 |  LEATHER              | 17 | 3
 30 | $04de | 1: Sword 1 | X:30 Y:06 |  APPLE                | 24 | 3
 31 | $051b | 1: Sword 1 | X:27 Y:08 |  ROCK CAKE            | 26 | 2
 31 | $051b | 1: Sword 1 | X:27 Y:08 |  ROCK CAKE            | 26 | 2
 32 | $055b | 1: Sword 1 | X:27 Y:10 |  FENCER               | 36 | 1
 33 | $05f7 | 1: Sword 1 | X:23 Y:15 |  BRONZE KEY           | 61 | 3
 34 | $061e | 1: Sword 1 | X:30 Y:16 |  LEATHER              | 0d | 3
 34 | $061e | 1: Sword 1 | X:30 Y:16 |  JUMPER +2            | 0b | 1
 35 | $051a | 1: Sword 1 | X:26 Y:08 |  BIG BOW              | 55 | 0
 35 | $051a | 1: Sword 1 | X:26 Y:08 |  PENKNIFE             | 30 | 0
 36 | $0518 | 1: Sword 1 | X:24 Y:08 |  CROSS OF LIFE        | 4e | 0
 37 | $0559 | 1: Sword 1 | X:25 Y:10 |  SHIELD               | 45 | 2
 38 | $05b9 | 1: Sword 1 | X:25 Y:13 |  VEST                 | 0e | 0
 38 | $05b9 | 1: Sword 1 | X:25 Y:13 |  DENIM                | 0c | 0
 39 | $05b2 | 1: Sword 1 | X:18 Y:13 |  SANDLES              | 1b | 3
 40 | $0512 | 1: Sword 1 | X:18 Y:08 |  IRON KEY             | 5f | 1
 41 | $0971 | 2: Sword 2 | X:17 Y:11 |  WAND OF PAIN         | 4c | 0
 41 | $0971 | 2: Sword 2 | X:17 Y:11 |  STAFF OF OURA        | 50 | 0
 42 | $089a | 2: Sword 2 | X:26 Y:04 |  BOILED EGG           | 28 | 3
 42 | $089a | 2: Sword 2 | X:26 Y:04 |  BOILED EGG           | 28 | 3
 43 | $087d | 2: Sword 2 | X:29 Y:03 |  IRON KEY             | 5f | 1
 44 | $085f | 2: Sword 2 | X:31 Y:02 |  RUSTY KEY            | 63 | 0
 45 | $0878 | 2: Sword 2 | X:24 Y:03 |  SHORT SWORD          | 34 | 0
 45 | $0878 | 2: Sword 2 | X:24 Y:03 |  PEA                  | 40 | 0
 45 | $0878 | 2: Sword 2 | X:24 Y:03 |  PEA                  | 40 | 0
 45 | $0878 | 2: Sword 2 | X:24 Y:03 |  PEA                  | 40 | 0
 45 | $0878 | 2: Sword 2 | X:24 Y:03 |  PEA SHOOTER          | 57 | 0
 45 | $0878 | 2: Sword 2 | X:24 Y:03 |  DART                 | 3f | 0
 45 | $0878 | 2: Sword 2 | X:24 Y:03 |  DART                 | 3f | 0
 45 | $0878 | 2: Sword 2 | X:24 Y:03 |  DART                 | 3f | 0
 46 | $0934 | 2: Sword 2 | X:20 Y:09 |  APPLE                | 24 | 2
 47 | $0872 | 2: Sword 2 | X:18 Y:03 |  GEM KEY              | 62 | 1
 48 | $099a | 2: Sword 2 | X:26 Y:12 |  BOILED EGG           | 28 | 3
 48 | $099a | 2: Sword 2 | X:26 Y:12 |  ROCK CAKE            | 26 | 1
 49 | $09f4 | 2: Sword 2 | X:20 Y:15 |  BRONZE KEY           | 61 | 2
 50 | $0a14 | 2: Sword 2 | X:20 Y:16 |  GRAPES               | 27 | 0
 51 | $0a35 | 2: Sword 2 | X:21 Y:17 |  ARROW                | 3d | 0
 51 | $0a35 | 2: Sword 2 | X:21 Y:17 |  ARROW                | 3d | 0
 51 | $0a35 | 2: Sword 2 | X:21 Y:17 |  ARROW                | 3d | 0
 52 | $0837 | 2: Sword 2 | X:23 Y:01 |  GOLD KEY             | 60 | 3
 53 | $0876 | 2: Sword 2 | X:22 Y:03 |  GOLD KEY             | 60 | 0
 54 | $09c0 | 2: Sword 2 | X:00 Y:14 |  GRAPES               | 27 | 0
 54 | $09c0 | 2: Sword 2 | X:00 Y:14 |  GRAPES               | 27 | 0
 55 | $08e0 | 2: Sword 2 | X:00 Y:07 |  APPLE                | 24 | 2
 55 | $08e0 | 2: Sword 2 | X:00 Y:07 |  GRAPES               | 27 | 3
 56 | $0943 | 2: Sword 2 | X:03 Y:10 |  GOLD KEY             | 60 | 3
 57 | $0840 | 2: Sword 2 | X:00 Y:02 |  FENCER               | 36 | 0
 58 | $088b | 2: Sword 2 | X:11 Y:04 |  CRASH HELMET         | 04 | 0
 58 | $088b | 2: Sword 2 | X:11 Y:04 |  VEST                 | 0e | 0
 59 | $0a25 | 2: Sword 2 | X:05 Y:17 |  CHEST #0             | 5c | 1
 60 | $0cab | 3: Sword 3 | X:11 Y:05 |  BRONZE KEY           | 61 | 2
 61 | $0cb4 | 3: Sword 3 | X:20 Y:05 |  APPLE                | 24 | 0
 61 | $0cb4 | 3: Sword 3 | X:20 Y:05 |  APPLE                | 24 | 0
 61 | $0cb4 | 3: Sword 3 | X:20 Y:05 |  APPLE                | 24 | 0
 61 | $0cb4 | 3: Sword 3 | X:20 Y:05 |  GRAPES               | 27 | 0
 62 | $0c56 | 3: Sword 3 | X:22 Y:02 |  CHAIN MAIL           | 18 | 0
 62 | $0c56 | 3: Sword 3 | X:22 Y:02 |  CRASH HELMET +1      | 04 | 0
 63 | $0c36 | 3: Sword 3 | X:22 Y:01 |  BROADSWORD           | 38 | 0
 64 | $0c16 | 3: Sword 3 | X:22 Y:00 |  SPIDER'S LEG         | 25 | 0
 64 | $0c16 | 3: Sword 3 | X:22 Y:00 |  SPIDER'S LEG         | 25 | 0
 65 | $0c13 | 3: Sword 3 | X:19 Y:00 |  BALL                 | 41 | 0
 65 | $0c13 | 3: Sword 3 | X:19 Y:00 |  BALL                 | 41 | 0
 65 | $0c13 | 3: Sword 3 | X:19 Y:00 |  BALL                 | 41 | 0
 65 | $0c13 | 3: Sword 3 | X:19 Y:00 |  BALL                 | 41 | 0
 66 | $0e66 | 3: Sword 3 | X:06 Y:19 |  RACKET               | 59 | 0
 67 | $0e41 | 3: Sword 3 | X:01 Y:18 |  SHIELD               | 45 | 1
 68 | $0dc5 | 3: Sword 3 | X:05 Y:14 |  MILK BOTTLE          | 2a | 1
 68 | $0dc5 | 3: Sword 3 | X:05 Y:14 |  MILK BOTTLE          | 2a | 1
 69 | $0d7f | 3: Sword 3 | X:31 Y:11 |  SWORD OF FREEDOM     | 39 | 0
 70 | $1420 | 5: Shield  | X:00 Y:01 |  SWEET                | 22 | 0
 71 | $1480 | 5: Shield  | X:00 Y:04 |  IRON KEY             | 5f | 1
 72 | $1482 | 5: Shield  | X:02 Y:04 |  STAFF OF MYSTIC      | 4f | 0
 72 | $1482 | 5: Shield  | X:02 Y:04 |  CROSS OF AID         | 4d | 0
 73 | $1501 | 5: Shield  | X:01 Y:08 |  PENKNIFE             | 30 | 1
 74 | $1409 | 5: Shield  | X:09 Y:00 |  KNIFE                | 31 | 3
 75 | $1414 | 5: Shield  | X:20 Y:00 |  ROCK                 | 3b | 3
 75 | $1414 | 5: Shield  | X:20 Y:00 |  ROCK                 | 3b | 3
 76 | $147a | 5: Shield  | X:26 Y:03 |  KNIFE                | 31 | 3
 77 | $147b | 5: Shield  | X:27 Y:03 |  PEA                  | 40 | 3
 77 | $147b | 5: Shield  | X:27 Y:03 |  PEA                  | 40 | 3
 77 | $147b | 5: Shield  | X:27 Y:03 |  PEA                  | 40 | 3
 77 | $147b | 5: Shield  | X:27 Y:03 |  PEA                  | 40 | 3
 78 | $148b | 5: Shield  | X:11 Y:04 |  THREE PRONGED KNIFE  | 32 | 1
 79 | $14aa | 5: Shield  | X:10 Y:05 |  PEA SHOOTER          | 57 | 1
 80 | $154d | 5: Shield  | X:13 Y:10 |  DART                 | 3e | 3
 81 | $15a2 | 5: Shield  | X:02 Y:13 |  PENKNIFE             | 30 | 0
 82 | $1534 | 5: Shield  | X:20 Y:09 |  THREE PRONGED KNIFE  | 32 | 2
 83 | $15d0 | 5: Shield  | X:16 Y:14 |  BLOWPIPE             | 58 | 3
 84 | $1596 | 5: Shield  | X:22 Y:12 |  SPIDER'S LEG         | 25 | 0
 85 | $1632 | 5: Shield  | X:18 Y:17 |  DART                 | 3e | 3
 85 | $1632 | 5: Shield  | X:18 Y:17 |  DART                 | 3e | 3
 85 | $1632 | 5: Shield  | X:18 Y:17 |  DART                 | 3e | 3
 86 | $1633 | 5: Shield  | X:19 Y:17 |  PEA                  | 40 | 2
 86 | $1633 | 5: Shield  | X:19 Y:17 |  PEA                  | 40 | 2
 86 | $1633 | 5: Shield  | X:19 Y:17 |  PEA                  | 40 | 2
 87 | $16ba | 5: Shield  | X:26 Y:21 |  ROCK                 | 3b | 2
 88 | $173d | 5: Shield  | X:29 Y:25 |  KITCHEN KNIFE        | 33 | 2
 89 | $1401 | 5: Shield  | X:01 Y:00 |  SHORTS               | 13 | 1
 90 | $1421 | 5: Shield  | X:01 Y:01 |  T-SHIRT              | 09 | 1
 91 | $14a5 | 5: Shield  | X:05 Y:05 |  BLOUSE               | 0a | 1
 92 | $144b | 5: Shield  | X:11 Y:02 |  SLACKS               | 15 | 1
 93 | $148f | 5: Shield  | X:15 Y:04 |  SANDLES              | 1b | 0
 94 | $1431 | 5: Shield  | X:17 Y:01 |  SANDLES              | 1b | 3
 95 | $1494 | 5: Shield  | X:20 Y:04 |  SKIRT                | 14 | 0
 96 | $1496 | 5: Shield  | X:22 Y:04 |  T-SHIRT              | 09 | 0
 96 | $1496 | 5: Shield  | X:22 Y:04 |  SHORTS               | 13 | 0
 96 | $1496 | 5: Shield  | X:22 Y:04 |  BASEBALL HAT         | 02 | 0
 97 | $150e | 5: Shield  | X:14 Y:08 |  SKIRT +1             | 14 | 0
 98 | $14ca | 5: Shield  | X:10 Y:06 |  BLOUSE +1            | 0a | 0
 99 | $1502 | 5: Shield  | X:02 Y:08 |  IRON KEY             | 5f | 2
100 | $14e2 | 5: Shield  | X:02 Y:07 |  SWEET                | 22 | 2
101 | $140a | 5: Shield  | X:10 Y:00 |  SWEET                | 22 | 1
102 | $1411 | 5: Shield  | X:17 Y:00 |  IRON KEY             | 5f | 3
103 | $148c | 5: Shield  | X:12 Y:04 |  SKIRT +2             | 14 | 0
103 | $148c | 5: Shield  | X:12 Y:04 |  SWEET                | 22 | 1
104 | $152d | 5: Shield  | X:13 Y:09 |  SWEET                | 22 | 1
105 | $152c | 5: Shield  | X:12 Y:09 |  SWEET                | 22 | 3
106 | $14ae | 5: Shield  | X:14 Y:05 |  GOLD KEY             | 60 | 0
106 | $14ae | 5: Shield  | X:14 Y:05 |  JUMPER               | 0b | 0
107 | $1568 | 5: Shield  | X:08 Y:11 |  SWEET                | 22 | 2
108 | $1540 | 5: Shield  | X:00 Y:10 |  IRON KEY             | 5f | 1
109 | $143b | 5: Shield  | X:27 Y:01 |  SWEET                | 22 | 0
110 | $14db | 5: Shield  | X:27 Y:06 |  BRONZE KEY           | 61 | 0
111 | $1612 | 5: Shield  | X:18 Y:16 |  IRON KEY             | 5f | 3
112 | $17bf | 5: Shield  | X:31 Y:29 |  SHIELD OF JUSTICE    | 46 | 3
113 | $11cd | 4: Cup     | X:13 Y:14 |  MILK BOTTLE          | 2a | 3
113 | $11cd | 4: Cup     | X:13 Y:14 |  SLACKS               | 15 | 0
114 | $1165 | 4: Cup     | X:05 Y:11 |  DIE                  | 5a | 0
115 | $1121 | 4: Cup     | X:01 Y:09 |  SUNGLASSES           | 01 | 0
115 | $1121 | 4: Cup     | X:01 Y:09 |  SUNGLASSES           | 01 | 0
116 | $1149 | 4: Cup     | X:09 Y:10 |  CROSS OF AID         | 4d | 0
117 | $1043 | 4: Cup     | X:03 Y:02 |  SHORT SWORD          | 34 | 0
117 | $1043 | 4: Cup     | X:03 Y:02 |  KITCHEN KNIFE        | 33 | 0
118 | $11ce | 4: Cup     | X:14 Y:14 |  ARROW                | 3d | 0
118 | $11ce | 4: Cup     | X:14 Y:14 |  ARROW                | 3d | 0
118 | $11ce | 4: Cup     | X:14 Y:14 |  ARROW                | 3d | 0
119 | $1158 | 4: Cup     | X:24 Y:10 |  BOW                  | 54 | 0
120 | $1007 | 4: Cup     | X:07 Y:00 |  ARROW                | 3d | 0
120 | $1007 | 4: Cup     | X:07 Y:00 |  ARROW                | 3d | 0
120 | $1007 | 4: Cup     | X:07 Y:00 |  ARROW                | 3d | 0
121 | $1072 | 4: Cup     | X:18 Y:03 |  PEA                  | 40 | 0
121 | $1072 | 4: Cup     | X:18 Y:03 |  PEA                  | 40 | 0
121 | $1072 | 4: Cup     | X:18 Y:03 |  PEA                  | 40 | 0
121 | $1072 | 4: Cup     | X:18 Y:03 |  PEA                  | 40 | 0
121 | $1072 | 4: Cup     | X:18 Y:03 |  PEA                  | 40 | 0
121 | $1072 | 4: Cup     | X:18 Y:03 |  PEA                  | 40 | 0
121 | $1072 | 4: Cup     | X:18 Y:03 |  TRAINERS             | 1d | 0
121 | $1072 | 4: Cup     | X:18 Y:03 |  SHOES                | 1c | 0
122 | $101c | 4: Cup     | X:28 Y:00 |  THROWING STAR        | 3c | 0
122 | $101c | 4: Cup     | X:28 Y:00 |  DART                 | 3e | 0
122 | $101c | 4: Cup     | X:28 Y:00 |  DART                 | 3e | 0
122 | $101c | 4: Cup     | X:28 Y:00 |  DART                 | 3e | 0
122 | $101c | 4: Cup     | X:28 Y:00 |  DART                 | 3e | 0
122 | $101c | 4: Cup     | X:28 Y:00 |  DART                 | 3e | 0
123 | $1018 | 4: Cup     | X:24 Y:00 |  BLOWPIPE             | 58 | 0
124 | $1089 | 4: Cup     | X:09 Y:04 |  THROWING STAR        | 3c | 0
124 | $1089 | 4: Cup     | X:09 Y:04 |  THROWING STAR        | 3c | 0
125 | $10d9 | 4: Cup     | X:25 Y:06 |  THROWING STAR        | 3c | 0
125 | $10d9 | 4: Cup     | X:25 Y:06 |  CLUB                 | 42 | 0
126 | $103b | 4: Cup     | X:27 Y:01 |  SLACKS               | 15 | 0
126 | $103b | 4: Cup     | X:27 Y:01 |  SHIELD               | 45 | 0
127 | $111e | 4: Cup     | X:30 Y:08 |  BASEBALL HAT         | 02 | 0
127 | $111e | 4: Cup     | X:30 Y:08 |  TRILBY               | 03 | 0
128 | $1258 | 4: Cup     | X:24 Y:18 |  ARROW                | 3d | 0
128 | $1258 | 4: Cup     | X:24 Y:18 |  ARROW                | 3d | 0
128 | $1258 | 4: Cup     | X:24 Y:18 |  ARROW                | 3d | 0
128 | $1258 | 4: Cup     | X:24 Y:18 |  ARROW                | 3d | 0
129 | $11f8 | 4: Cup     | X:24 Y:15 |  BOW                  | 54 | 0
130 | $11ea | 4: Cup     | X:10 Y:15 |  SHEATH               | 35 | 0
131 | $10e5 | 4: Cup     | X:05 Y:07 |  BISCUIT              | 23 | 0
131 | $10e5 | 4: Cup     | X:05 Y:07 |  BISCUIT              | 23 | 0
131 | $10e5 | 4: Cup     | X:05 Y:07 |  BISCUIT              | 23 | 0
132 | $109d | 4: Cup     | X:29 Y:04 |  APPLE                | 24 | 0
132 | $109d | 4: Cup     | X:29 Y:04 |  APPLE                | 24 | 0
132 | $109d | 4: Cup     | X:29 Y:04 |  APPLE                | 24 | 0
132 | $109d | 4: Cup     | X:29 Y:04 |  APPLE                | 24 | 0
133 | $1210 | 4: Cup     | X:16 Y:16 |  ROCK CAKE            | 26 | 0
133 | $1210 | 4: Cup     | X:16 Y:16 |  ROCK CAKE            | 26 | 0
133 | $1210 | 4: Cup     | X:16 Y:16 |  ROCK CAKE            | 26 | 0
133 | $1210 | 4: Cup     | X:16 Y:16 |  APPLE                | 24 | 0
133 | $1210 | 4: Cup     | X:16 Y:16 |  APPLE                | 24 | 0
134 | $1275 | 4: Cup     | X:21 Y:19 |  SPIDER'S LEG         | 25 | 0
134 | $1275 | 4: Cup     | X:21 Y:19 |  SPIDER'S LEG         | 25 | 0
135 | $1284 | 4: Cup     | X:04 Y:20 |  TRAINERS             | 1d | 0
135 | $1284 | 4: Cup     | X:04 Y:20 |  BOOTS                | 1e | 0
135 | $1284 | 4: Cup     | X:04 Y:20 |  JUMPER +1            | 0b | 0
136 | $1348 | 4: Cup     | X:08 Y:26 |  DENIM                | 0c | 0
136 | $1348 | 4: Cup     | X:08 Y:26 |  JEANS                | 16 | 0
137 | $13c3 | 4: Cup     | X:03 Y:30 |  KITCHEN KNIFE        | 33 | 0
138 | $110a | 4: Cup     | X:10 Y:08 |  GOLD KEY             | 60 | 1
139 | $1148 | 4: Cup     | X:08 Y:10 |  GOLD KEY             | 60 | 2
140 | $1180 | 4: Cup     | X:00 Y:12 |  GOLD KEY             | 60 | 0
141 | $1111 | 4: Cup     | X:17 Y:08 |  GOLD KEY             | 60 | 0
142 | $1308 | 4: Cup     | X:08 Y:24 |  BRONZE KEY           | 61 | 0
143 | $1c8d | 7: Crown 1 | X:13 Y:04 |  SKULL OF STATUS      | 51 | 0
143 | $1c8d | 7: Crown 1 | X:13 Y:04 |  SPANNER              | 44 | 0
144 | $1fad | 7: Crown 1 | X:13 Y:29 |  GRAPES               | 27 | 1
145 | $1fcb | 7: Crown 1 | X:11 Y:30 |  GRAPES               | 27 | 2
146 | $1fd0 | 7: Crown 1 | X:16 Y:30 |  GRAPES               | 27 | 1
147 | $1fb4 | 7: Crown 1 | X:20 Y:29 |  GRAPES               | 27 | 0
148 | $1fd8 | 7: Crown 1 | X:24 Y:30 |  BOOTS                | 1f | 3
148 | $1fd8 | 7: Crown 1 | X:24 Y:30 |  CHAIN MAIL           | 18 | 1
148 | $1fd8 | 7: Crown 1 | X:24 Y:30 |  CHAIN MAIL           | 0f | 1
148 | $1fd8 | 7: Crown 1 | X:24 Y:30 |  VEST +1              | 0e | 3
149 | $1b13 | 6: Crown 2 | X:19 Y:24 |  MILK BOTTLE          | 2a | 1
149 | $1b13 | 6: Crown 2 | X:19 Y:24 |  MILK BOTTLE          | 2a | 1
149 | $1b13 | 6: Crown 2 | X:19 Y:24 |  CHEST #1             | 5c | 1
150 | $0ee9 | 3: Sword 3 | X:09 Y:23 |  STAFF                | 43 | 0
151 | $0c1b | 3: Sword 3 | X:27 Y:00 |  IRON KEY             | 5f | 3
152 | $0c57 | 3: Sword 3 | X:23 Y:02 |  GOLD KEY             | 60 | 0
153 | $0613 | 1: Sword 1 | X:19 Y:16 |  IRON KEY             | 5f | 1
154 | $05f4 | 1: Sword 1 | X:20 Y:15 |  GOLD KEY             | 60 | 0
155 | $06d3 | 1: Sword 1 | X:19 Y:22 |  BRONZE KEY           | 61 | 2
156 | $06d8 | 1: Sword 1 | X:24 Y:22 |  GEM KEY              | 62 | 0
157 | $069a | 1: Sword 1 | X:26 Y:20 |  IRON KEY             | 5f | 1
158 | $07de | 1: Sword 1 | X:30 Y:30 |  IRON KEY             | 5f | 0
159 | $0717 | 1: Sword 1 | X:23 Y:24 |  GEM KEY              | 62 | 0
160 | $0750 | 1: Sword 1 | X:16 Y:26 |  STAFF                | 43 | 0
161 | $0a37 | 2: Sword 2 | X:23 Y:17 |  STAFF                | 43 | 0
162 | $0b11 | 2: Sword 2 | X:17 Y:24 |  IRON KEY             | 5f | 3
163 | $0a29 | 2: Sword 2 | X:09 Y:17 |  GOLD KEY             | 60 | 2
164 | $00f9 | 0: Forest  | X:25 Y:07 |  APPLE                | 24 | 0
164 | $00f9 | 0: Forest  | X:25 Y:07 |  APPLE                | 24 | 0
165 | $003b | 0: Forest  | X:27 Y:01 |  APPLE                | 24 | 1
165 | $003b | 0: Forest  | X:27 Y:01 |  APPLE                | 24 | 1
166 | $07e2 | 1: Sword 1 | X:02 Y:31 |  GOLD KEY             | 60 | 3
167 | $0322 | 0: Forest  | X:02 Y:25 |  BALL                 | 41 | 0
167 | $0322 | 0: Forest  | X:02 Y:25 |  BALL                 | 41 | 0
167 | $0322 | 0: Forest  | X:02 Y:25 |  BALL                 | 41 | 0
168 | $1c06 | 7: Crown 1 | X:06 Y:00 |  PLATE                | 19 | 0
168 | $1c06 | 7: Crown 1 | X:06 Y:00 |  PLATE                | 20 | 0
169 | $1c2b | 7: Crown 1 | X:11 Y:01 |  HELM                 | 05 | 0
169 | $1c2b | 7: Crown 1 | X:11 Y:01 |  CRASH HELMET +2      | 04 | 0
170 | $1c2f | 7: Crown 1 | X:15 Y:01 |  HELM                 | 06 | 0
170 | $1c2f | 7: Crown 1 | X:15 Y:01 |  PLATE                | 10 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
171 | $1c2d | 7: Crown 1 | X:13 Y:01 |  GRAPES               | 27 | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  ARROW                | 3d | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  ARROW                | 3d | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  ARROW                | 3d | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  RACKET               | 59 | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  BALL                 | 41 | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  BALL                 | 41 | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  BALL                 | 41 | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  BALL                 | 41 | 0
172 | $1cdf | 7: Crown 1 | X:31 Y:06 |  BALL                 | 41 | 0
173 | $1cef | 7: Crown 1 | X:15 Y:07 |  CHAIN SAW            | 3a | 0
174 | $1da5 | 7: Crown 1 | X:05 Y:13 |  CHAIN SAW            | 3a | 0
175 | $1de3 | 7: Crown 1 | X:03 Y:15 |  AQUALUNG             | 12 | 0
176 | $1e77 | 7: Crown 1 | X:23 Y:19 |  CROSS BOW            | 56 | 0
176 | $1e77 | 7: Crown 1 | X:23 Y:19 |  ARROW                | 3d | 0
176 | $1e77 | 7: Crown 1 | X:23 Y:19 |  ARROW                | 3d | 0
176 | $1e77 | 7: Crown 1 | X:23 Y:19 |  ARROW                | 3d | 0
176 | $1e77 | 7: Crown 1 | X:23 Y:19 |  ARROW                | 3d | 0
177 | $1e2d | 7: Crown 1 | X:13 Y:17 |  ARROW                | 3d | 0
177 | $1e2d | 7: Crown 1 | X:13 Y:17 |  ARROW                | 3d | 0
177 | $1e2d | 7: Crown 1 | X:13 Y:17 |  ARROW                | 3d | 0
177 | $1e2d | 7: Crown 1 | X:13 Y:17 |  ARROW                | 3d | 0
177 | $1e2d | 7: Crown 1 | X:13 Y:17 |  ARROW                | 3d | 0
178 | $1e92 | 7: Crown 1 | X:18 Y:20 |  BOILED EGG           | 28 | 0
178 | $1e92 | 7: Crown 1 | X:18 Y:20 |  BOILED EGG           | 28 | 0
179 | $1f34 | 7: Crown 1 | X:20 Y:25 |  BOILED EGG           | 28 | 0
179 | $1f34 | 7: Crown 1 | X:20 Y:25 |  BOILED EGG           | 28 | 0
179 | $1f34 | 7: Crown 1 | X:20 Y:25 |  BOILED EGG           | 28 | 0
180 | $0c76 | 3: Sword 3 | X:22 Y:03 |  CHEST #2             | 5c | 3
181 | $10ef | 4: Cup     | X:15 Y:07 |  CHEST #3             | 5c | 1
182 | $1869 | 6: Crown 2 | X:09 Y:03 |  PLATE                | 19 | 0
183 | $1875 | 6: Crown 2 | X:21 Y:03 |  HELM +2              | 06 | 3
183 | $1875 | 6: Crown 2 | X:21 Y:03 |  HELM +1              | 06 | 2
184 | $18ae | 6: Crown 2 | X:14 Y:05 |  PLATE                | 20 | 3
184 | $18ae | 6: Crown 2 | X:14 Y:05 |  PLATE                | 10 | 0
185 | $19bb | 6: Crown 2 | X:27 Y:13 |  HELM +2              | 06 | 0
186 | $1aad | 6: Crown 2 | X:13 Y:21 |  PLATE                | 11 | 0
187 | $1ca9 | 7: Crown 1 | X:09 Y:05 |  STAFF OF OURA        | 50 | 0
187 | $1ca9 | 7: Crown 1 | X:09 Y:05 |  CROSS OF LIFE        | 4e | 0
187 | $1ca9 | 7: Crown 1 | X:09 Y:05 |  WAND OF PAIN         | 4c | 0
188 | $1dd7 | 7: Crown 1 | X:23 Y:14 |  THROWING STAR        | 3c | 0
188 | $1dd7 | 7: Crown 1 | X:23 Y:14 |  THROWING STAR        | 3c | 0
188 | $1dd7 | 7: Crown 1 | X:23 Y:14 |  THROWING STAR        | 3c | 0
188 | $1dd7 | 7: Crown 1 | X:23 Y:14 |  THROWING STAR        | 3c | 0
188 | $1dd7 | 7: Crown 1 | X:23 Y:14 |  THROWING STAR        | 3c | 0
189 | $0cb2 | 3: Sword 3 | X:18 Y:05 |  STAFF OF MYSTIC      | 4f | 0
189 | $0cb2 | 3: Sword 3 | X:18 Y:05 |  THROWING STAR        | 3c | 0
189 | $0cb2 | 3: Sword 3 | X:18 Y:05 |  THROWING STAR        | 3c | 0
189 | $0cb2 | 3: Sword 3 | X:18 Y:05 |  THROWING STAR        | 3c | 0
190 | $1f19 | 7: Crown 1 | X:25 Y:24 |  PLATE                | 11 | 0
190 | $1f19 | 7: Crown 1 | X:25 Y:24 |  HELM +2              | 06 | 0

The first entry is `09 f4 01 bc`; "bc" (188) might refer to the number of items
in the list, although there are slightly more entries than that. The last is `88
82 88 81`. These appear to be just start/stop markers. A set of `00 00 00 00`
empty entries appear after the stop marker.

A set header has the following four bytes:

- Two bytes for the address offset from the start of the map section.
- One byte padding, always `00`.
- One byte for the length of this item set, including its header.

An item entry has the following four bytes:

- Item ID
- Item variant. This is only used to uniquely distinguish chests, or alternate
  versions of armour items with higher stats (BLOUSE, CRASH HELMET, HELM 06,
  JUMPER, SKIRT, VEST). It's also used to determine the owner of a heart.
- Padding, `00`.
- Corner. A number from `80` to `83` determines which corner of this square the
  item is located in.

The following items do not appear in any map: 07 HELMET, 08 CROWN OF GLORY, 1a
PLATE, 21 PLATE, 29 RABBIT PIE, 2b MILK BOTTLE, 2c MILK BOTTLE, 2d STAMINA
POTION, 2e SIGHT POTION, 2f DECRIPPLING POTION, 37 SAMURAI, 49 BOMB, 4a SMASHED
BOTTLE, 52 HEALSTONE, 53 FUNNY STICK, 5b COIN, 5d CUP OF LIFE, 5e HEART OF
&gt;character&lt;, 64 STAR KEY, 65 EMPTY BOTTLE.
