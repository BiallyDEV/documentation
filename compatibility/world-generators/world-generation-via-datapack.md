---
description: 
cover: >-
  https://cdn.discordapp.com/attachments/896841738621177896/966827878706708560/unknown.png
coverY: 0
---

# Datapack 

You can naturally world generate noteblocks and string blocks from Oraxen via datapack.

## Setting datapack enviroment

First you need to create a normal datapack that will allow you to change/add/remove things in vanilla overworld dimension.

To make it easier you can download example datapack here:
link do datapacklaloal

{% hint style="danger" %}
Remember that you need to change "pack_format" in pack.mcmeta to version of minecraft that you are using on server.
Example datapack is set for 1.19-1.19.2
Check what pack format you need to use on the page below.
https://minecraft.fandom.com/wiki/Pack_format
{% endhint %}

To get vanilla world generation configuration files you want also to download this example vanilla worldgen datapack.
https://github.com/slicedlime/examples/blob/master/vanilla_worldgen.zip

Inside "oraxen_world_generation\data\minecraft\worldgen\biome" paste biome_name.json of biome that you want to edit from vanilla_worldgen.zip
In example we are using birch forest biome, so we copy birch_forest.json to our biome folder.
We will back to this file later.

## Creating a placed_feature

### What is a placed_feature?

placed_feature is a configuration file that say to minecraft how to generate configured_feature on the world.

### How to make placed_feature?

To make easier for most of users let's support from vanilla_worldgen.zip and look how minecraft do it with the closest block that you want to generate your custome one.

For example we will create placed_feature for custom berry bush "blueberry_bush". We already have berry bushes in minecraft and there is placed_feature for that called "patch_berry_common.json". Let's copy it then to our datapack, change name of file and change path inside to our configured_future.

Should look like this:
```yaml
{
  "feature": "oraxen:blueberry_bush",
  "placement": [
    {
      "chance": 32,
      "type": "minecraft:rarity_filter"
    },
    {
      "type": "minecraft:in_square"
    },
    {
      "heightmap": "WORLD_SURFACE_WG",
      "type": "minecraft:heightmap"
    },
    {
      "type": "minecraft:biome"
    }
  ]
}
```

## Creating a configured_feature

### What is a configured_feature?

configured_feature is a configuration file that say to placed_future what and how to generate.

### How to make configured_feature?

We will use there as well vanilla to make easier.

Let's copy "patch_berry_bush.json" from configured_feature to our configured_feature folder, because that's where "patch_berry_common.json" placed_feature is going.

Change thhe name of file and open it. 
Now, let's start the configuration for oraxen string block "blueberry_bush":

At first let's change the "Name" to minecraft:tripwire because oraxen string blocks use tripwire for that.

Also we need to add to "Properties" states that tripwire use and delete "age" propertie because tripwire don't have it.

After that change the properties on exact same that you see on f3 in game when you are looking on custom string block.

Result should look something like this:

```yaml
    "feature": {
      "feature": {
        "config": {
          "to_place": {
            "state": {
              "Properties": {
		"attached": "false",
		"disarmed": "false",
		"east": "true",
		"north": "false",
		"powered": "false",
		"south": "false",
		"west": "false"					
              },
              "Name": "minecraft:tripwire"
            },
            "type": "minecraft:simple_state_provider"
          }
        },
        "type": "minecraft:simple_block"
      },
```

Same thing with noteblocks but with a few changes in "Properties" and "Name"

```yaml
    "feature": {
      "feature": {
        "config": {
          "to_place": {
            "state": {
              "Properties": {
		"instrument": "basedrum",
		"note": "2",
		"powered": "false"					
              },
              "Name": "minecraft:note_block"
            },
            "type": "minecraft:simple_state_provider"
          }
        },
        "type": "minecraft:simple_block"
      },
```

End result of "blueberry_bush" should look like this:

```yaml
{
  "config": {
    "tries": 96,
    "xz_spread": 7,
    "y_spread": 3,
    "feature": {
      "feature": {
        "config": {
          "to_place": {
            "state": {
              "Properties": {
		"attached": "false",
		"disarmed": "false",
		"east": "true",
		"north": "false",
		"powered": "false",
		"south": "false",
		"west": "false"					
              },
              "Name": "minecraft:tripwire"
            },
            "type": "minecraft:simple_state_provider"
          }
        },
        "type": "minecraft:simple_block"
      },
      "placement": [
        {
          "predicate": {
            "predicates": [
              {
                "blocks": "minecraft:air",
                "type": "minecraft:matching_blocks"
              },
              {
                "offset": [
                  0,
                  -1,
                  0
                ],
                "blocks": "minecraft:grass_block",
                "type": "minecraft:matching_blocks"
              }
            ],
            "type": "minecraft:all_of"
          },
          "type": "minecraft:block_predicate_filter"
        }
      ]
    }
  },
  "type": "minecraft:random_patch"
}
```

## Adding feature to biome generation

In oraxen_world_generation\data\minecraft\worldgen\biome open file of biome that you want to add your future, finde "features": line and scroll to lines where are patch_pumpkin, glow_lichen etc.

```yaml
      "minecraft:glow_lichen",
      "minecraft:forest_flowers",
      "minecraft:trees_birch",
      "minecraft:flower_default",
      "minecraft:patch_grass_forest",
      "minecraft:brown_mushroom_normal",
      "minecraft:red_mushroom_normal",
      "minecraft:patch_sugar_cane",
      "minecraft:patch_pumpkin"
```  

To add our placed_feature we add it like the other placed features are added but with namespace to our folder with placed_feature:

```yaml
      "minecraft:glow_lichen",
      "minecraft:forest_flowers",
      "minecraft:trees_birch",
      "minecraft:flower_default",
      "minecraft:patch_grass_forest",
      "minecraft:brown_mushroom_normal",
      "minecraft:red_mushroom_normal",
      "minecraft:patch_sugar_cane",
      "minecraft:patch_pumpkin",
      "oraxen:blueberry_bush"
``` 

Save everything and drop oraxen_world_generation folder to your server world in  world_name/datapacks folder. Restart server and if you make evrything good then you should see that on birch_forest on new chunks the blueberry bushes start to generate. 

{% hint style="danger" %}
Remember to first test it on test server before you will drop it on the server where people are playing because you can destroy your world without a way to fix it. 
{% endhint %}



