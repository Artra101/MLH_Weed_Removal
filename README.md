This is a copy of etcipnja's Farmware MLH that I changed a bit to my needs for the farmbot I had to make for a school project.

# Problem:

When you have to deal with a lot of plants you might be puzzled that you have to create a dedicated sequence for each
instance of a plant. For exmaple: if you have 10 weeds - you probably need to create a weed removal sequence for every weed
individually. 

# Solution:

The idea is to write a loop that executes needed sequences for each eligible plant.
- Apply filters to select plants to be treated       		(Example: Select all Carrots with status "planned")
- Execute initial sequence                                  (Example: "Pick up seeder")
- For each plant:
    - Execute a sequence before moving to plant's location  (Example: "Grab a seed")
    - Move to plant's location
    - Execute a sequence at plant's location                (Example: "Plant a seed")
    - Update plant tags 	                                (Example: "Mark this instance of carrot as "planted")
- Execute end sequence                                      (Example: "Return seeder")

# Reference:

- FILTER BY PLANT NAME
    - Filter by plant names (comma separated) for example "Carrot, Beets", case insensitive, '*' means select all
- FILTER BY META DATA
    - MetaData is a piece of information (key:value) saved with the plant. Unfortunatelly you won't see it anywhere on
    the Farm Designer, but it is printed to the log. This farmware can update metadata, so you can use it in the
    filter later. See below about expected format and examples.
- INIT SEQUENCE NAME
    - MLH Mount Weeder
- SEQUENCE NAME AFTER MOVE
    - MLH Weed
- END SEQUENCE NAME
    - MLH Dismount Weeder
- SAVE IN META DATA
    - Each plant will get its meta data updated upon successful completion of "AFTER" sequence. Please see below about
    expected format and examples
- DEFAULT Z AXIS VALUE WHEN MOVING
    - 0


Meta data fields require specially formatted input. In Python it is called "list of tupple pairs". Here is the example:

```
[ ( ‘key1’, ‘value1’ ) , ( ‘key2’, ‘value2’ ) ]
```

You can use whatever you want as a ‘key' and ‘value’ as long as they are strings.
More than one element in the list allows you to filter by multiple criteria and update multiple meta data tags at once!
Put "None" if you want to skip metadata feature.

There are special words with special meaning:
- key ‘plant_stage’ will not deal with metadata, instead it works with the attribute “Status” visible
in Farm Designer for every plant. Only valid values that go with this key are ‘planned’, ‘planted’ and ‘harvested’
- key ‘del’ in SAVE filed causes to delete currently saved meta data for this plant. If ‘value' is ‘*’ - all metadata is
deleted, otherwise only one key specified in ‘value’ is deleted
- value ‘today’ is replaced with actual today’s date. In FILTER you can write ‘!today’ which means “not today’.



# Installation:

Use this manifest to register farmware
https://raw.githubusercontent.com/Artra101/MLH_Weed_Removal/master/MLH/manifest.json

# Bugs:

I noticed that if you change a parameter in WebApplication/Farmware form - you need to place focus on some other
field before you click "RUN". Otherwise old value is  passed to farmware script even though the new value
is displayed in the form.


# Credits:

The original idea belongs to @rdegosse with his Loop-Plants-With-Filters. https://github.com/rdegosse/Loop-Plants-With-Filters/blob/master/README.md
This Farmware - is a simplified version of it with nice perks about saving/filtering meta data

