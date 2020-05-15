# Mega Man Xtreme 2: Shared Health Upgrades
<b>!!!!Still a Work in progress project!!!!</b>
<br>Two patches for Mega Man Xtreme 2 to make the health upgrades shared over both player characters.
A detailed description can be found [on the RomHacking page of this hack](https://www.romhacking.net/hacks/5032/), as well as an alternative download link.
In this README you will find further information on how this patch was made and reference material.

## Tools
- [The BGB emulator](http://bgb.bircd.org/#downloads), for disassembly, hex editing, memory viewing, testing and even breakpoints.
- [LunarIPS](https://www.romhacking.net/utilities/240/), to create a patch file.
- [HxD](https://mh-nexus.de/en/hxd/), a clean hex editor used to study the save files with.

## Process
### begin and 1.0.0
A small handfull of things were already known about Xtreme 2. Some information on savings, health and how the game exactly treated the characters. On the latter an error was made by not reading up properly before starting the work. Take note to always use exsisting sources to the maximum.

How characters and their health worked had to be explored first. This was done by looking closely at the RAM values and what changed during switching and taking a health upgrade (tank). 7 variables were discovered: 2 health capacity and current value variables per character; the current health value; the current health backup; the current character value.
Knowing these breakpoints were utilised to see how they correspond to the save files and when and why they are changed during the game.
This was a lengthy and involved process. Meanwhile the meaning of the values had to be discovered as well. For instance, health capacity ran (again full savings were used to compare with fresh empty savings) from 0x10 to 0x20, but whenever damage was taken, 0x80 was added first, followed by the subtraction of the actual damage. 

With some shallow tests a few ROM banks responsible for dealing with these values were identified. Bank $28 apparently handled the resetting of the values. $D had a portion in charge of increasing the health after an upgrade. $2F contained most of the saving and loading code, as such also the saving and loading of characters health. 
To our advantage, this code was shared over the gamemodes and stages. From earlier NES hacking work, code was sometimes duplicated across multiple stages, making patching harder.

Most of the finding were informally compiled and plan was drawn up, where and which code must be changed to achieve a shared healthpool. This quickly turned into a complex patch. Especially as the exact intention was not compatible with the base game anymore (when comparing the save data).
This is what caused the two patches to be separate.
After a long implementation and test process the patch seemed to be working and complete...

### 1.0.1
After some feedback on RomHackingDotNet (RHDN) it occured that a logic flaw was present. The highest of the healths was picked to be shared, instead of adding up all upgrades.
Fixing this issue was a matter of going back into the useful patch file and changing it.

### 1.0.2
After more feedback a RHDN user by the name G3OFF pointed out that starting a fresh game using certain game modes completely broke saving and loading, effectively making them unplayable. 
This oversight was a result of shallow testing and poor conceptualizing. 
The fix however, proved rather easy, simply by building in some extra code into the saving mechanism. 

### 1.0.3
<b>!!!! Work in progress, process of a future release!!!!</b>
<br>G3OFF continued testing the 1.0.2 version and found even bigger issues. The game contained a progression system from one mode to the next. The patch however, did not take this into account. Furthermore the 'global' health sharing behaviour seemed to be inappropriate to be global for every mode. 

Precursory work was done to check how to address the issue properly.
There turned out to be a lot of unknowns and the patch would have to include a few exception cases to apply ONLY to the 'Xtreme' gamemode. 
After this roadblock it was decided to take a break from the project and approach it later with a fresh perspective .

[Patch making utility was created](https://github.com/ElwinBran/LSTPatchMaker), which made the editing process easier and enabled it to think of the code first instead of edits. A value in memory was found that represents the gamemode and both patches were going to be rewritten with a check to this value built in. Once the patches were completed testing was required.

## References
The same references as the other Xtreme projects:
- [A visual opcode map for the gameboy Z80.](http://pastraiser.com/cpu/gameboy/gameboy_opcodes.html)
- [Detailed descriptions for the opcodes, which were confusing to me without it.](https://raw.githubusercontent.com/gb-archive/salvage/master/txt-files/gb-instructions.txt)
- [The full gameboy memory map explanation.](http://gameboy.mongenel.com/dmg/asmmemmap.html)
