# Pebble Pre-Builder

Update resources media by scanning Pebble resources folder and generate C header file based on application keys.

## Usage

There are two ways to use Pebble Pre-Builder :

* Run `ppb -gALL` to the folder where your appinfo.json file is.
* Modify the wscript file of your pebble project. 

For the second case, you have to import ruby in the top of your wscript :

```wscript
import os.path
from sh import ruby
```

Then, define the pre-build function :

```wscript
def prebuild(ctx):
    ruby('path to the ppb script', g='ALL')
```

Finally, just put to following line into the build function :

```wscript
def build(ctx):
    prebuild(ctx)
    ...
```

Tap `pebble build` and watch the magic happen in your appinfo.json file and `src/generated` folder.
 
## Installation

Pebble Pre-Builder use a ruby script to scan your folder and update your appinfo.json.  
If `ppb` doesn't work properly, tap this following lines in a terminal :

```
# For mac users
brew install ruby && gem install json
# For unix users
sudo apt-get install ruby && gem install json
```

## Images

Just put your images into the `resources/images` and build the watchapp with the pebble tool.
Pebble Pre-Builder takes the name of your image files to update the appinfo.json.  
Example is always better than a bunch of words so let's take a look to the following lines :

```
# Folder tree
resources
|- images
|  |- cross-pbi.png
|  |- logo~color.png
|  |- logo~bw.png
|  |- timeline-pbi8.png

# Updated appinfo.json file
...
"resources": {
	"media": [
		{
			"type": "png",
			"file": "images/logo.png",
			"name": "IMAGE_LOGO"
		},
		{
			"type": "pbi",
			"file": "images/cross-pbi.png",
			"name": "IMAGE_CROSS"
		},
		{
			"type": "pbi8",
			"file": "images/timeline-pbi8.png",
			"name": "IMAGE_TIMELINE"
		}
	]
}
...
```

Pebble Pre-Builder is a tool to increase speed development but it's not increase the Pebble SDK so do not forget `png` is the only image format supported by the watch.  
To set the menu icon of your app, name your image file `menu_icon(-*).png`.  
The image types values can be :

* pbi
* pbi8
* png-trans

## Fonts

Generate font resources is not that easy than generate image resources.
Like your images, put your font files into the `resources/fonts` and now pay attention.
Pebble Pre-Builder should know which size and which allowed characters you want for each font files.
Like the previous section, Example is friendly to understand new concept :

```
# Folder tree
resources
|- fonts
|  |- main-ascii-12-18.ttf
|  |- second-date-time.ttf

# Updated appinfo.json file
...
"resources": {
	"media": [
		{
		    "characterRegex": "[ -~]",
			"type": "font",
			"file": "fonts/man-ascii-12-18.ttf",
			"name": "FONT_MAIN_ASCII_12"
		},
		{
		    "characterRegex": "[ -~]",
			"type": "font",
			"file": "fonts/man-ascii-12-18.ttf",
			"name": "FONT_MAIN_ASCII_18"
		},
				{
		    "characterRegex": "[0-9a-zA-Z ]",
			"type": "font",
			"file": "fonts/second-date-time.ttf"
			"name": "FONT_SECOND_DATE_14"
		},
				{
		    "characterRegex": "[0-9APM ]",
			"type": "font",
			"file": "fonts/second-date-time.ttf"
			"name": "FONT_SECOND_TIME_14"
		}
	]
}
...
```

If you don't provide size after allowed characters, the default font size is 14.  
The allowed characters values can be :

* ascii
* number
* letter
* alphanumeric
* time 
* date
* temperature

## Raw & Animation

The resource files doesn't need treatment like image or font resources.  
Simply add your animation (do not forget only `apng` format is supported by Pebble watch and only on Basalt platform) into `resources/animations` and your other resource files into `resources`.  
There are no restrictions here so do what you want !

## C generation

To communicate between your watch application and your smartphone, Pebble use keys store in `appinfo.json`.
Pebble Pre-Builder generate a C file named `pebble-keys.h` into `src/generated` folder based on `appinfo.json`.

```
# appinfo.json
...
"appKeys": {
    "dummy": 0,
    "foo bar": 42 
},
...

# generated pebble-keys.h
#ifndef PEBBLE_KEYS.H
#define PEBBLE_KEYS.H

typedef enum {
    PEBBLE_KEYS_DUMMY = 0,
    PEBBLE_KEYS_FOO_BAR = 42
} PebbleKeys;

#endif

```

## Warning

Pebble Pre-Builder introduce a bunch of rules. If you respect this  the following lines, everything should be ok :

* Do not forget to rebuild the watchapp each time you modify the resources folder.  
* Do not add or modify by hand the resources section of your appinfo.json.  
* Do not put your resources files into nested folder.  
* Only the supported images (png, pbi, pbi8, png-trans) and fonts () are generated.
* Do not name several files with the same name - without extension part of course.
* Resources file starting by a dot character is skipped.

## Licence

Copyright (c) 2015 Alexis Le Provost  
Licensed under the MIT license.