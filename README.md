# Pebble Pre-Builder

Update resources media by scanning Pebble resources folder.

## Usage

There are two ways to use Pebble Pre-Builder.
The first one is run `ppb` to the folder where your appinfo.json file is. 
The second one - the smart one - is to modify the wscript file of your pebble project.
First, you have to import ruby in the top of your wscript :

```wscript
import os.path
from sh import ruby
```

Then, define the pre-build function :

```wscript
def prebuild(ctx):
    ruby('path to the ppb script')
```

Finally, just put to following line into the build function :

```wscript
def build(ctx):
    prebuild(ctx)
    ...
```

Tap `pebble build` and watch the magic happen in your appinfo.json file.

**Do not forget to rebuild the watchapp each time you modify the `resources` folder.**
**Do not add or modify by hand the resources section of your appinfo.json.**
**Do not put your resources files into nested folder.**
 
## Dependencies

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
|  |- logo.png
|  |- background~color.pbi

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
			"file": "images/background.pbi",
			"name": "IMAGE_BACKGROUND"
		}
	]
}
...
```

To set the menu icon of your app, the name your image file `icon.*`.

**Only the supported image formats (png, pbi, pbi8, png-trans) are generated.** 

## Fonts

Generate font resources is not that easy than generate image resources.
Like your images, put your font files into the `resources/fonts` and now pay attention.
Pebble Pre-Builder should know which size you want for each font files.
To do that, put the `ppbinfo.json` file into `resources/fonts` folder and define the font size like :

```
# Folder tree
resources
|- fonts
|  |- ppbinfo.json
|  |- test.ttf
|  |- test-thin.otf
|  |- test-bold.otf

# ppbinfo.json
{
   "test.ttf": [12, 18],
   "test-thin.otf: [16]
}

# Updated appinfo.json file
...
"resources": {
	"media": [
		{
			"characterRegex": "[ -~]",
			"file": "fonts/test.ttf",
			"name": "FONT_TEST_12"
		},
		{
			"characterRegex": "[ -~]",
			"file": "fonts/test.ttf",
			"name": "FONT_TEST_18"
		},
		{
			"characterRegex": "[ -~]",
			"file": "fonts/test-thin.ttf",
			"name": "FONT_TEST_16"
		},
		{
			"characterRegex": "[ -~]",
			"file": "fonts/test-bold.ttf",
			"name": "FONT_TEST_BOLD_14"
		},
	]
}
...
```

If you don't provide this file or if there is no information about a certain font file, Pebble Pre-Builder will generate the font with a size of 14.

## Licence

Copyright (c) 2015 Alexis Le Provost
Licensed under the MIT license.