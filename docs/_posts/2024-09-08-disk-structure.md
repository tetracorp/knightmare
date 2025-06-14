---
title: Disk structure
categories: analysis
---

_Knightmare_ uses a non-standard file storage method based on the programmer's
own mini disk operating system, RATT-DOS. This makes the disk impossible to
copy by standard means. However, cracked versions of the game from shortly after
its release were able to convert it to a standard filesystem.

Each file entry is 24 bytes, and includes 12 bytes for the filename, a zero
byte, 3 bytes of information file's location on disk, 4 bytes for the filesize
when unpacked, and 4 bytes for the filesize when compressed.

### Disk 1

| Filename     |    |    |    | Unpacked|  Packed |
|--------------|----|----|----|--------:|--------:|
| fed_bootfile | 03 | 01 | 01 |   4,996 |   4,996 |
| fed_Demo     | 04 | 01 | 01 |   5,592 |   5,592 |
| min001       | 05 | 03 | 01 |  40,004 |  40,004 |
| min002       | 08 | 01 | 02 |  40,004 |  40,004 |
| min003       | 09 | 01 | 02 |  40,004 |  40,004 |
| title.mus    | 0a | 0e | 02 |  97,468 |  97,468 |
| seg5a        | 18 | 04 | 05 |  32,160 |  32,160 |
| seg5b        | 1c | 1c | 05 | 258,092 | 258,092 |
| min2001      | 38 | 01 | 05 |  40,004 |  40,004 |
| min2002      | 39 | 02 | 05 |  40,004 |  40,004 |
| min2003      | 3b | 02 | 04 |  40,004 |  40,004 |
| min2004      | 3d | 02 | 04 |  40,004 |  40,004 |
| min2005      | 3f | 01 | 04 |  40,004 |  40,004 |
| min2006      | 40 | 02 | 05 |  40,004 |  40,004 |
| min2007      | 42 | 02 | 05 |  40,004 |  40,004 |
| federation   | 44 | 0f | 05 |  92,172 |  92,172 |
| Fed_Sounds   | 54 | 08 | 04 |  49,048 |  44,024 |
| Fed_Sounds2  | 5c | 05 | 04 |  31,104 |  25,440 |
| xA3.mus      | 61 | 05 | 05 |  27,856 |  27,856 |
| seg0         | 66 | 01 | 05 |  12,200 |   3,464 |
| xA1.mus      | 67 | 04 | 00 |  20,712 |  20,712 |
| seg1         | 6b | 07 | 00 |  64,944 |  38,316 |
| xA2.mus      | 72 | 04 | 01 |  22,280 |  22,280 |
| seg2         | 76 | 02 | 01 |  23,512 |  11,908 |
| xA4.mus      | 78 | 04 | 02 |  21,320 |  21,320 |
| seg3         | 7c | 03 | 02 |  19,079 |  13,120 |
| EndDEMO      | 7f | 01 | 05 |   4,904 |   4,904 |
| Victory.MUS  | 80 | 05 | 05 |  27,240 |  27,240 |
| nox001       | 85 | 04 | 04 |  40,004 |  40,004 |
| nox002       | 89 | 04 | 04 |  40,004 |  40,004 |

### Disk 2

| Filename     |    |    |    | Unpacked|  Packed |
|--------------|----|----|----|--------:|--------:|
| bl_maps      | 03 | 05 | 01 |  26,584 |  26,584 |
| kmroof.pic   | 08 | 03 | 01 |  40,004 |  16,600 |
| Fed4.pic     | 0b | 05 | 02 |  40,004 |  26,036 |
| Fed5.pic     | 10 | 06 | 02 |  40,000 |  40,000 |
| Fed8.pic     | 16 | 07 | 05 |  40,000 |  40,000 |
| KMShop-A.pic | 1d | 06 | 04 |  40,000 |  40,000 |
| KMShop-B.pic | 23 | 07 | 04 |  40,000 |  40,000 |
| KMDoor-A.pic | 2a | 07 | 05 |  40,000 |  40,000 |
| KMDoor-B.pic | 31 | 07 | 05 |  40,000 |  40,000 |
| KMAln-A.pic  | 38 | 07 | 00 |  40,000 |  40,000 |
| KMAln-B.pic  | 3f | 05 | 00 |  40,004 |  25,184 |
| KMAln-C.pic  | 44 | 04 | 01 |  40,004 |  23,740 |
| KMAln-D.pic  | 48 | 04 | 01 |  40,004 |  24,280 |
| KMAln-E.pic  | 4c | 05 | 02 |  40,004 |  26,684 |
| KMAln-F.pic  | 52 | 07 | 02 |  40,000 |  40,000 |
| kmaln-g.pic  | 59 | 04 | 05 |  40,004 |  22,696 |
| kmaln-h.pic  | 5d | 07 | 05 |  40,000 |  40,000 |
| KMWall-A.pic | 64 | 06 | 04 |  40,000 |  40,000 |
| KMWall-B.pic | 6a | 06 | 04 |  40,000 |  40,000 |
| KMWall-C.pic | 70 | 06 | 05 |  40,000 |  40,000 |
| KMWall-D.pic | 76 | 06 | 05 |  40,000 |  40,000 |
| KMWall-E.pic | 7c | 06 | 05 |  40,000 |  40,000 |
| SEG4         | 82 | 0f | 00 | 165,180 |  91,792 |
| xDeath.MUS   | 91 | 05 | 00 |  32,112 |  32,112 |

### Compression

A number of files are compressed on disk. The files are compressed using
PRO-PACK, by Rob Northen Computing. I got an error message when attempting to
unpack them using PRO-PACK v2.08, which was released as freeware on Aminet in
1999, but xfdmaster was able to unpack them.

The PRO-PACK compressed files are:

- On disk 1: Fed_Sounds, Fed_Sounds2, seg0, seg1, seg2, seg3.
- On disk 2: Fed4.pic, KMAln-B.pic, KMAln-C.pic, KMAln-D.pic, KMAln-E.pic,
kmaln-g.pic, kmroof.pic, SEG4.

All of these files are packed using method 2, the fast method.

The total of the file sizes for disk 1 is 1,231,112 packed, 1,294,727 unpacked;
for disk 2 is 875,708 packed, 1,063,904 unpacked. The total file size of disk 1
exceeds the capacity of an 800KB Amiga disk, which can be explained by the use
of PowerPacker to compress data, which is seen on pirate releases that restore
the standard DOS disk index.

This suggests that the use of PowerPacker was original to the commercial
release, and not something added later by pirates to fit their own cracktro and
documentation on disk. Accounting for PowerPacker, the file size total for disk
1 is 663,564 bytes, and for disk 2 is 827,524 bytes, accounting for the file
indices which are 908 bytes each.

### File analysis

The immediately interesting thing about the filenames here is the term "fed",
short for Federation. "Federation Wars" was the original working title for
Crowther's previous game, Captive. The 
[Captive directory listing](https://captive.atari.org/Technical/Directory/Directory.php)
for the Atari ST release shows that this prefix "fed" was used in that game too.
Internally, the Knightmare game code refers to the disks as "FED1:" and "FED2:".
although the game disks don't have those names.

The files `EndDemo`, `fed_Demo`, `fed_bootfile` and `federation` are Amiga
executables. `federation` is the main game program, weighing in at 90 KB. The
files `fed_Demo` and `EndDemo` are the intro and outro, respectively. They're
also small, as the graphics and music are stored in other files.

The various .mus files are music in 
[MMDC format](https://www.exotica.org.uk/wiki/MMDC), a format created by Tony
Crowther and used in Knightmare and Captain Planet. The files `xA1.mus` through
`xA4.mus` are prefixed with executable code, whereas `title.mus` and
`Victory.MUS` are not. A copy can be found
[at the ExoticA wiki](https://www.exotica.org.uk/wiki/Knightmare).

A number of 40,004 byte files on disk 1 consist of full screen graphics used in
the intro or outro. These are quiet inefficiently stored; e.g. the Knightmare
logo is re-used for multiple screens, and text are stored as full-screen
graphics instead of being stored as a bitmap font and drawn on the fly.

Various .pic files on disk 2, being 40,000 or 40,004 bytes, are spritesheets for
in-game graphics. Most are prefixed with "KM" for Knightmare, although a few use
the prefix Fed, "Federation".

The files beginning with "seg" are graphics. These are used in the opening
animation, aside from `SEG4` on disk 2.

Fed_Sounds and Fed_Sounds2 are certainly the game audio.

The file `bl_maps` on disk 2 appears to store the dungeon maps. Further analysis
is needed.

### Date analysis

Since both of the game disks use RATT-DOS instead of the standard Amiga
operating system disk functions, no file date is recorded for the game files.

On the SPS release, which likely reflects an unmodified original copy, disk 1
includes an executable file BootSys, viewable in standard OS, dated 17 September
1991 at 10:35:29. This is very plausibly an accurate file date. Two icon files,
.info and BootSys.info, are dated 27 November 1989. The disk created date for
both disks is 21 November 1989 at 06:47:56. The 1989 dates are unlikely to be
accurate.

Two main pirate releases were made, by Crystal and Phil Douglas. The Crystal
version was later modified by Mac of Katharsis to remove "clerical spells
protection". The Douglas version was probably later than the Crystal release,
boasting that it is a "100%" crack and also adds a trainer for various cheats.

The Crystal release dates most files with the erroneous date of 26 January 1959.
The reason for the 1959 date is unknown. New files added, such as the group's
cracktro, use the 1 January 1978 date which reflects a default date set by an
Amiga without a battery-backed clock.

The Douglas release also uses the 1959 date, but filenames are mostly correctly
capitalized and the trainer boasts that it was based on a working original copy,
rather than the Crystal release. The Douglas additions (a trainer,
startup-sequence, game manual in English and French, etc) are mostly dated 16
January 1992 between 1:04 am and 1:17 am, which is a plausible real date,
although some files use a a 1978 default date. The disk date on the Douglas
release is 28 January 1992 at 18:41:27 and 18:43:42, for disks 1 and 2
respectively.
