# Mod Name: Ruins

## Authors:

__Creator:__ Justin Bengtson (d00dv4d3r) 
 - updated to 1.1 by kolt666
 - kept and developed since Minecraft 1.2.3 by AtomicStryker

__Version:__ see changelog in mod file

__License:__ http://atomicstryker.net/about.php


### What It Does:

Adds procedurally-generated ruins randomly into the game world, which are created from user-defined template files.
This mod comes with some default template files, but if you would like to see a certain template in the game all you have to do is create it.
There is an ingame template parsing process, [a video on my website explains how to use it](https://www.youtube.com/watch?v=E9cNolY_LsQ).

### How It Does It:

This hooks into the chunk generation using the methods provided by Forge to create the ruins.
You can create anything you like procedurally using template files, even complete structures 
(I just provide extra rules for creating ruins...)

Templates are loaded for each biome, and a generic set of templates is kept that can be used in all biomes
(although spawning conditions for each template must still be met.)
The mod will first check to see if a biome-specific template will be used or whether it will use a generic template.  Chances for either are provided per biome.

You can alter global options for ruin spawning by editing ruins.txt using your favorite text editor.  
Options are provided for normal and nether.

Each template has a weight.  All of the template weights for a specific biome, as well as the generic templates,
are added together to get a total weight for that biome 
from which a random number is generated to determine which template to load.
For instance, if you have five templates for a specific biome with weights 
10, 10, 5, 5, and 1, the total of these is 31.

A template with a weight of 1 has a 1 in 31 chance of spawning under those weight conditions.

Please see the template_rules.txt file for more information on creating templates for this mod.  

The file is set up so that you can simply copy it to the mods/resources/ruins folder and being editing it.


## Server Commands Added By Ruins

`/parseruin TEMPLATENAME`

see the YouTube video [Ruins Mod Ingame Template Parser](https://www.youtube.com/watch?v=E9cNolY_LsQ) for a in-depth explanation

__tldw:__ 

 1. make a one-block plate the width and thickness of the template from one block 
which is not part of the template. 
 1. Build template structure on top. 
 1. use command `/parseruin filename`. 
 1. break one block of plate.

For advanced features such as basements consult template_rules.txt

`/testruin TEMPLATENAME [X Y Z [ROTATION]]`

[] parts are optional each. `TEMPLATENAME` is either a filename in the "templateparser" biome folder 
or a folder and a template filename e.g  `/testruin ocean/lighthouse`

`/testruin` can be called exactly once without any arguments after successfully parsing a template 
in order to immediatly test that template

`ROTATION` is 

- 0 for 'none'
- 1 for EAST 'one to the right'
- 2 for SOUTH 'two to the right'
- 3 for WEST 'three to the right'

`/testruin` supports minecraft relative coordinates, and can be called by command blocks.

`/testruin` also supports using '_' instead of a relative or absolute y coordinate, 
which will let ruins find a suitable y using its spawner algorithm for given x,z


## Biome Specific Features

In the mods/resources/ruins folder you will find a folder specific to each biome.
If you place a template in one of those folders it will only be available to spawn in that biome.
If you place a template inside the mods/resources/ruins/generic folder, it will be considered a generic template that can spawn in any biome, other conditions (such as surface blocks) permitting.

The ruins.txt file now has variables for each biome, giving the chance that a biome-specific template will be used as opposed to a generic template.
If a biome-specific template is not available when asked for, no template will spawn.

## How To Install

1. Install Forge
1. Copy everything found in this archive into your corresponding .minecraft/ folders
1. Enjoy.
1. Edit /mods/ruins.txt, and any template files for weight, to fine-tune your experience.

__________

## Template Basics

This file is designed to be copied into the mods/resources/ruins folder and be edited to create a new template.  
It is functional as-is; you can use it as a placeholder for future templates after renaming it.

Templates have a .tml extension and the mod will attempt to load any and
all files with that extension if they reside in the <minecraft root>/
mods/resources/ruins folder. Whether it does so successfully or not can be
checked in the log file `<minecraft root>/mods/ruins_log.txt`. The mod will also
load any templates found in the biome folders within
`<minecraft root>/mods/resources/ruins`.

These template files are a simple text file; simply open them in your
favorite text editor to make changes or create new ones.

There are three sections to each template file:
> * VARIABLES define how the template is generated.
> * BLOCK RULES define how the layers are generated.
> * LAYERS tells the mod how to place blocks using BLOCK RULES.

You can specify a comment by placing a "#" character at the beginning of the
line.  You technically /don't/ have to do this, since the mod only looks for
particular line starts when loading a template, but I consider it good form 
because it increases readability and you prevent any errant line reads.


## VARIABLES

    biomesToSpawnIn=<name1>,<name2>,<name3>...

Adds the Template being loaded (regardless which folder it is in) to a Ruins Biome
found in the Minecraft Biomelist so that <namex> matches the Biome's name.
If you don't know a Biome's name, find it's folder. The foldername equals the
Biome name. Case insensitive. A Template cannot be added to a Biome more than once.
The Template is still added to the corresponding folder it was loaded from (if not already in).

example: biomesToSpawnIn=forest,foresthills,taiga
Optional. You can use the folders as before, or use this to save space, or both.

    weight=<weight>

Weight is how much weight this template has during random generation.  
When the mod asks for a template to place it adds up the weights of all templates
that are currently loaded and generates a random number based off of that
total, then checks to see what template to load.  If the mod loads five
templates with weights 1, 5, 5, 10 and 10, the template with weight 10 has a
10 in 31 chance of being generated, while the template with weight 1 has a 1 in 31 chance 
of being generated.  A template only has weight in its biome. Defaults to 1.

    embed_into_distance=<number of layers>

Specifies how many layers to embed into the target blocks.  This is useful for creating basements or dungeons. Defaults to 1.

    acceptable_target_blocks=<blockID>,<blockID>,<etc...>
    
This is a comma-delimited list of block IDs that this template can spawn on.
Only the top layer of blocks where the template will spawn are checked.
Optional line. Defaults to accept any block.

    unacceptable_target_blocks=<blockID>,<blockID>,<etc...>

This is a comma-delimited list of block IDs that this template can NOT spawn on.
Only the top layer of blocks where the template will spawn are checked. Optional line. 
Defaults to accept any block.

    dimensions=<height>,<width>,<length>

Defines how big this template is. Height is the number of layers that are used, not the height that sticks out
above the ground.  Width is the north-south width, and length is the east-west width.  

For reference, north in the template is the top of the text file. All default to 0.

    allowable_overhang=<overhang>

Specifies the allowed number of blocks in the potential build site that do not have
a surface within a reasonable distance, leaving the template "hanging" over an edge.

Defaults to 0.

    max_leveling=<leveling>

Specifies the maximum surface noise / height difference, within the build site, that will be accepted when 
considering a potential build location.

The site will then be levelled prior to spawning the template. 
If there is overhang allowed, also specifies how much "support" blocks are put
under the template when building it overlapping a surface edge.

Defaults to 2.

    leveling_buffer=<lbuffer>

The distance around the build site that will also be levelled.  Values higher
than 5 will use 5, since the world would otherwise be mangled pretty badly.
Defaults to 0.

Ruins 12.8 introduces a setting of -1, which will prevent any(!) site leveling

    preserve_water=<yes/no>

If set to 1 all site checking rules will treat water as air so that the ruin can be generated beneath/in water.  
Any rules that replace a block with air will respect water and not replace it.  

If set to 0, water is treated like any other block. Defaults to 0/no

    preserve_lava=<yes/no>

If set to 1 all site checking rules will treat lava as air so that the ruincan be generated beneath/in lava.  
Any rules that replace a block with air will respect lava and not replace it.  

If set to 0, lava is treated like any other block. Defaults to 0

    preserve_plants=<yes/no>

If set to 1 all site checking rules will treat trees, leaves, and cactus 
as air so that the ruin can be generated in and around them.  Any rules that
replace a block with air will respect trees, leaves, and cactus and not
replace them.  If set to 0, trees, leaves, and cactus are treated like any other blocks.  

Cactus adjacent to other blocks will break down, so you may not actually see cactus in your structure.

Defaults to 0/no

    random_height_offset=<min>,<max>

Specifies block range in which the Ruin will be randomly shifted up or down
on an established valid location before actually being built.

Defaults to 0,0 / no random shifting

__NOTE:__ When using preserve_water and preserve_lava it is advised that you 
restrict the cut_in_buffer as much as possible, preferably to 0.  If it is
more than 0 you may get some unexpected fluid dynamics if the structure
does not spawn completely in water/lava.

___________

Ruins 13.8 adds optional variable `uniqueMinDistance` which overrides global templateInstances `MinDistance`

    uniqueMinDistance=1500

defaults to 0. Values of 0 are handled as if no value was specified at all.

Ruins 13.9 adds optional variable "preventRotation" which keeps a template in the specified direction

    preventRotation=1

Defaults to 0. Values of 0 are handled as if no value was specified at all.

Ruins 14.3 adds the optional repeatable variable "adjoining_template" which lets you spawn adjacent templatesthese will not be checked for minimal distances against ANY other ruins, make sure your spacings are big enoughthese will also not be checked for circular dependencies, you build an infinite loop, you wait it outthese will be checked against the rules the template itself containsRuins will use its spawning algorithm to determine a fitting y coordinate to the xz you provideadjoining templates will be randomly rotated, you can disable this in their templates if you wantsyntax: adjoining_template=<template>;<relativeX>;<allowedYdifference>;<relativeZ>[;<spawnchance>]<template> is the full filepath relative to the resources/ruins folder, so, working like /testruin<relativeX> and <relativeZ> are the relative coords from the host x,z spawnpoint to the adjoining x,z<allowedYdifference> states how much the computed adjoining template height may absolutely differ before aborting<spawnchance> is optional and does what the name implies, [0-100] values allowedExample: adjoining_template=generic/MoaiHead;-25;10;25;33

```
weight=5
embed_into_distance=1
acceptable_target_blocks=stone,grass,dirt,sand,gravel,clay
dimensions=4,7,5
allowable_overhang=0
max_leveling=2
leveling_buffer=0
preserve_water=0
preserve_lava=0
preserve_plants=0
```

## BLOCK RULES

Each rule must be formatted carefully:

    rule<number>=<condition>,<chance to appear>,<list of blocks>rule<number>
    
The mod does not care what you call these and will number the rules in the
order they are loaded (sequentially from 1), so long as the line start with"rule".  
I suggest using "rule#" as a mnemonic, such as "rule1", "rule2","rule12", etc...  
Once the templates are loaded, the first rule in the list
becomes rule #1 for the purposes of building a layer, the second becomes rule#2, and so on.

The mod uses a special rule, 0, which defines the Air block with a 100% spawnrate and no conditional.  
You can use this rule in the layers to "blank out"certain areas (providing space for mobs, for instance).

    <condition>

A conditional to the block being placed, aside from randomness.

> 0 = Normal, spawns first
> ±1 = Checks block under, spawns delayed
> ±2 = Checks block adjacent, spawns delayed
> ±3 = Checks block above, spawns last
> 7 = Spawns last

Where the negative cases for 1,2,3 denote NOT. 

In other words, 2 means the block will only spawn if there is a block next to it, 
while -2 will NOT spawn if there are any blocks next to it.

Adjacent in this case is not diagonal, only along the cardinal directions.
Adjacent blocks are processed after 0 and 1 conditionals have been placed,
and "under blocks" are processed after all other blocks have been placed.
Please note that these blocks are still processed in an order dependent on
the rotation of the site.  

For a normal, north-facing site, the block rules
are processed for each line from the left and from the bottom template to thetop.
_DO NOT CHAIN CONDITIONS._ That results in undefined behaviour.

    <chance to appear>

The chance that this block will appear, out of 100, depending on whether thecondition above is met.

    <list of blocks>

A comma-delimited set of minecraft blockIDs.  This will determine what block
will actually spawn in the location.  Each blockID is given the same weight,
so you can skew the odds of a certain blockID appearing by adding it multiple
times.  

You can specify block metadata by adding a `-<metadata>` after theblockID.  

For instance, to place a cobblestone stairs block ascending to the east, use `stone_stairs-3`.  

To place a wood half-block (slab), use `stone_slab-2`. You can find a list of blockIDs and 
commonly-used metadata via Google. There is also one included, `idmappings.txt`

#### Example 1: 

    rule1=0,100,0,cobblestone,mossy_cobblestone

This spawns either air, cobblestone, or a mossy cobblestone 100% of the time.

#### Example 2: 

    rule1=1,50,mossy_cobblestone

This spawns a mossy cobblestone 50% of the time but only if another blockis underneath it.

#### Example 3: 

    rule1=2,100,stone_slab-2,planks

This spawns either a stone slab or a plank block, but only if there is anadjacent block on the same level.

#### Example 4: 

    rule1=0,100,torch-5

This spawns a torch standing on the ground.

#### Example 5: 

    rule1=0,25,MediumChest

This spawns a Medium Chest (see above) 25% of the time.

#### Example 6: 

    rule1=0,50,MobSpawner:Villager
    
This spawns a Mob Spawner with Entity Id "Villager" 50% of the time

Entity Id's are case sensitive! Mod added Mobs are allowed. 
Use a tool like Simply Hax to get correct Entity Id's

#### Example 7: 

Ruins 9.0 exposes minecraft ChestGenHook for your meddling.

    rule1=0,100,ChestGenHook:dungeonChest:10

This spawns a chest and tells the Minecraft generator to fill it with "10" items of the "dungeonChest" preset.
This might include extremely powerful items added by mods.	

Here is the complete list (as of mc 1.4.7) of ChestGen presets:  

```
"mineshaftCorridor"; 
"pyramidDesertyChest"; 
"pyramidJungleChest"; 
"pyramidJungleDispenser"; 
"strongholdCorridor";  
"strongholdLibrary"; 
"strongholdCrossing"; 
"villageBlacksmith"; 
"bonusChest"; 
"dungeonChest";	
```

As of 10.6, all chest rules can also append a metadata/rotation int similar to a chest block ID	

Example: 

    rule1=0,100,ChestGenHook:dungeonChest:10-5	

for a chest filled with dungeon loot and rotated to meta 5.

#### Example 8:

Ruins 10.3 adds Command Block Support. Usage: 

    rule1=0,100,CommandBlock:command:sender
    
`command` being the Command string to be executed and `sender` being the Command sender 
(a player name, Rcon...) 

you can provide several alternative command blocks seperated by comma, however you may not 
mix command blocks and other blocks in one ruleRuins 

#### Example 9:

12.1 adds Standing Signs. Usage: 

rule1=0,100,StandingSign:a|b|c|d-4

`a`,`b`,`c`,`d` are the 4 Strings/lines you can write on a sign. The metadata/rotation of the sign is affixed as usual.

#### Example 10:

Ruins 12.2 adds IInventory block support. Usage: 

    IInventory;<blockName>;<itemList>-<blockrotation>

where `<blockName>` is the registry name of the Block that sports an inventory
where `<itemList>` is a list of elements seperated by '+', each element is a block or item registry name
optionally followed by #stacksize OR #stacksize#metadata OR #stacksize#metadata#inventory 
slot

    rule1=0,100,IInventory;chest;red_flower+arrow#10+wool#3#15-5

this spawns a chest rotated to 5, which will contain a single red flower, 10 arrows, and 3 wool of color red 
(meta 15)

#### Example 11: 

rule1=0,100,IInventory;dispenser;arrow#10+snowball#20+egg#3-1

This spawns a dispenser rotated to 1, with 10 arrows 20 snowballs and 3 eggs
you may use this for items added by mods. names not found in the registry will simply be skipped over.

You may also use this for container blocks added by mods. Invalid anythings should be skipped.

The Ruins Parser will do this for you when you parse an `IInventory` block. 
Empty chests get hooked with dungeon loot automatically. If you want to prevent this, 
either edit the template afterwards or place some dirt in the chest.

If you specify the same slot index multiple times, only the first `itemstack` gets put in.

#### Example 12:

Ruins 15.0 adds ChestGenHook (see above) as valid <itemList> entry for IInventory rules

    rule1=0,100,IInventory;dispenser;ChestGenHook:dungeonChest:5-1


#### Example 13: 

Ruins 14.3 adds the -addbonemeal flag to use on `IGrowable` blocks such as saplings or most plants
simply append `-addbonemeal` to a block which is an IGrowable (and only those!)

Note this overrides certain checks done before hand application of bonemeal, such as darkness checks 
for shrooms. Also note /undo does NOT take growing things into account when cleaning up your shenanigans.

Below example is for oak and dark oak saplings

    rule1=0,100,0,sapling-addbonemeal,sapling-5-addbonemeal

#### Example 14: 

Ruins 14.9 adds support for item NBT data. the json nbt string replaces 'stacksize' in the format, 
as nbt capable items are never stackable

    rule1=0,100,IInventory;minecraft:chest;minecraft:diamond_sword#{RepairCost:2,display:{Name:"Choppa"}}#0#0


#### Example 15:

Ruins 15.0 adds support for tile entity NBT data. This is best achieved by adding the blockname 
to the global `teblock` variable in ruins.txt.  Usage: 

    teBlock;<blockName>;<nbttag json>-<blockrotation>

i.e. 

    rule1=0,100,teBlock;minecraft:trapped_chest;{x:-156,y:65,z:72,Items:[0:{Slot:12b,id:5s,Count:1b,Damage:0s},1:{Slot:13b,id:24s,Count:1b,Damage:1s}],id:"Chest"}-2



## LAYERS

Each layer is a comma-delimited list of rules, one for each block.  There
must be as many layers as the height, and each layer must have "layer" before
the rules and end with "endlayer".  There are as many rows as the length, and
as many rules as the width.  If you want the block blanked out use 0, which
represents the Air-block rule.

```
rule1=0,100,preserveBlock
rule2=0,80,brick_block,brick_block,dirt,stone,gravel
rule3=0,100,brick_block

layer
2,2,2,2,2
2,1,1,1,2
2,1,1,1,2
2,1,1,1,2
2,1,1,1,2
2,1,1,1,2
2,2,2,2,2
endlayer

layer
3,3,3,3,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,3,3,3,3
endlayer

layer
3,3,3,3,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,3,3,3,3
endlayer

layer
3,3,3,3,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,0,0,0,3
3,3,3,3,3
endlayer
```
