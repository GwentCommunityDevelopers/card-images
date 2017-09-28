# Gwent-data Card Generator 1.2
This PHP script reads the content of the cards.json file generated by **Gwent Data** and generate cards images to be used with Gwent tools.

## Requirement
The script needs PHP >= 5.6 installed with Imagick and Imagick PHP extension installed. It can be called using HTTP or command line.

The Gwent cards artworks need to be extracted from the game using the *high* card size (located in the `high.standard` game file)  with their original filenames. The [Unity Assets Bundle Extractor](https://github.com/DerPopo/UABE) can be used for this.

## Installation
1. If you're not familiar with `composer`, [download it](https://getcomposer.org/) and [install the dependencies](https://getcomposer.org/doc/01-basic-usage.md#installing-dependencies) of the script.
2. Copy the cards artworks into the `assets/artworks/` folder.
3. In the `card_generator.php` file, change the value of :
  - `VERSION` to get the right Gwent version
  - `MAX_CARDS` to specify how many cards should be generated to the max
4. Copy the last `cards.json` generated file in the same folder as `card_generator.php`

## Generating Gwent cards
There is two way to generate the cards :
1. Generate all the cards the json file contains by calling the script
2. Generate a few specific cards (ideal for updates) by using the `cards` parameter and giving the ID of the cards separed with commas.
  - Command line : ```php card_generator.php cards 112101,112102,112105...```
  - HTTP : ```localhost/card_generator.php?cards=112101,112102,112105,...```
To be sure the artwork exists for a card, it would be generated only if the value of `released` is 1 in the json file.

Each card will be generated in the `images/**GWENT_VERSION**/**CARD_ID**/**VARIATION_ID**/` folder, with 4 different size versions (original, high, medium, low, thumbnail).

## Generating custom cards
To generate a custom card, you need to put the image file in the `assets/custom-artworks/` folder
The script needs to be called with the `custom` parameters, followed by those parameters :
  - **filename** (default : default.png) : filename of the image in the custom-artwork folder
  - **strength** (default : 0) : Strength of the card. Must be an integer between 1 and 99
  - **faction** (default : neutral) : Card faction. Must be one of these : `northernrealms`, `scoiatael`, `skellige`, `monster`, `nilfgaard`, `neutral`
  - **type** (default : bronze) : Card type. Must be one of these : `bronze`, `silver`, `gold`
  - **rarity** (default : common) : Card rarity. Must be one of these : `common`, `rare`, `epic`, `legendary`
  - **position** (optional) : Card position. If given, must be one of these : `melee`, `ranged`, `siege`, `multiple`
  - **spy** (optional) : Card is an agent. If given, must be `1`
  - **count** (optional) : Card countdown. If given, must be an integer between 1 and 9

Each parameter must be given as `parameter=value`.
The custom artwork will be resized with the same height or width as a Gwent artwork, then cropped to fit. If you don't want your image to be cropped, the Height/Width ratio of the image must be 10/7.
The card will be generated in the `custom-images` folder with the same different sizes as Gwent cards.

### Example
  - Command line : ```php card_generator.php custom strength=7 filename=my_custom_card.png faction=nilfgaard spy=1 position=multiple```
  - HTTP : ```localhost/card_generator.php?custom=1&strength=7&filename=my_custom_card.png&faction=nilfgaard&spy=1&position=multiple```
