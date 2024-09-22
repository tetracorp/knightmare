---
title: Experience and level
categories: analysis
---

### Gaining XP

Experience points can be gained in the following ways:

1. Making a melee attack. The attack doesn't have to hit, and you can even swing
at thin air. However, attacks from the back row marked "CAN'T REACH" don't
count.
2. Dealing damage with a melee attack.
3. Making a ranged attack.
4. Casting a spell.
5. Throwing an item by picking it up with the mouse and clicking on the screen.
The XP for this is granted to the character currently wearing the Helmet of
Justice. Throwing a heart at the Spring of Life does grant the XP for throwing
an item.
6. Taking damage from an enemy. Bumping into walls or the Sprig of Life doesn't
count. The character gains the XP even if the damage kills them.
7. Killing an enemy. All living party members receive XP.

Using an weapon or wand in hand grants XP to the class associated with that
item. A character can level up in all six classes this way. For example, it's
common for characters to gain Adventurer levels early on from unarmed punches
and throwing baseballs.

Clicking on the screen to throw an item grants XP to the Adventurer class
regardless of the item. All other sources of XP are granted to the character's
original base class.

### Level

Each character has a level in all six classes. Internally, this is stored as a
16-bit integer, with each named level (e.g. Novice, Tenderfoot) representing 4
level increases. However, you only gain stat increases at named levels. Each
character starts at level 4, Novice, for their original base class, and level 0
in all other classes.

XP is stored for each class as a 32-bit integer. Once XP goes above a threshold
equal to `2^((level/4)+6)` (i.e. 64 at level 0 and doubling at every fourth
level thereafter), the level increases by one and the XP resets. XP gained above
the threshold is not lost.

The following chart shows the internal level number, level title, amount of XP
to reach that level title from the previous level title, and the total amount of
XP necessary to reach that level title:

L#   | Level name | XP            | Total
-----|------------|---------------|---------------
 0   | (none)     |             0 |             0
 4   | NOVICE     |           256 |           256 
 8   | TENDERFOOT |           512 |           768 
12   | APPRENTICE |         1,024 |         1,792 
16   | ADEPT      |         2,048 |         3,840 
20   | VERSED     |         4,096 |         7,936 
24   | PROFICIENT |         8,192 |        16,128 
28   | ADVANCED   |        16,384 |        32,512 
32   | MAVEN      |        32,768 |        65,280 
36   | DOYEN      |        65,536 |       130,816 
40   | MASTER     |       131,072 |       261,888 
44   | MASTER 1   |       262,144 |       524,032 
48   | MASTER 2   |       524,288 |     1,048,320 
52   | MASTER 3   |     1,048,576 |     2,096,896 
56   | MASTER 4   |     2,097,152 |     4,194,048 
60   | MASTER 5   |     4,194,304 |     8,388,352 
64   | MASTER 6   |     8,388,608 |    16,776,960 
68   | MASTER 7   |    16,777,216 |    33,554,176 
72   | MASTER 8   |    33,554,432 |    67,108,608 
76   | MASTER 9   |    67,108,864 |   134,217,472 
80   | MASTER 10  |   134,217,728 |   268,435,200 
84   | MASTER 11  |   268,435,456 |   536,870,656 
88   | MASTER 12  |   536,870,912 | 1,073,741,568 
92   | MASTER 13  | 1,073,741,824 | 2,147,483,392 
96   | MASTER 14  | 2,147,483,648 | 4,294,967,040 
100+ | MASTER 15+ | 2,147,483,648 | 

Levels go from Novice to Master. For example, a total of 256 Adventurer XP will
grant the rank of Novice Adventurer. After Master, each additional level adds a
number after the title; e.g. Master Adventurer 1, Master Adventurer 2, etc.
After Master 14, the XP cost for each level is fixed.

### XP awards (WIP)

The amount of XP awarded is not fully explored yet. Early observed values:

- Attacking with a level 1 weapon attack or unarmed (e.g. punch, kick, penknife
  swing grants 3 XP.
- Attacking with a level 2 weapon attack (e.g. penknife stab) grants 4 XP.
- Melee damage seems to deal 17 XP.
- Casting a level 1 spell (e.g. Open, Confuse, Rem) grants 7 or 9 XP. The
  difference may be that it grants only 7 it fails, e.g. for running out of
  Magic or if the spell fizzles. A level 2 spell (e.g. Glow, Fitness) grants 9
  or 11 XP, and casting at increased level by clicking on the number at the top
  of the list will add another +2 to that. A successful level 6 Fitness grants
  13 or 21 depending on if it succeeds.
- Throwing a Ball gives between 8 and 11 XP.
- Taking a hit from the rebound of your own ball gives 2 or 3 XP.
- Throwing items by clicking in the main area grants different XP depending on
  the item, probably based on weight. Most grant 10 to 14, but throwing the
  rabbit pie gives 20 to 22 XP.
- Taking damage seems to grant an amount of XP which correlates with the amount
  of damage taken.
