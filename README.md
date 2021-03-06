<p align="center">
  <img src="colors_accessibility/static/colors_accessibility_logo.svg" alt="Logo" />
</p>

<p align="center">
  <a href="/LICENSE">
    <img src="https://img.shields.io/github/license/phryniewicz/colors-accessibility" alt="MIT License" />
  </a>
  <a href="/SIZE">
    <img src="https://img.shields.io/github/repo-size/phryniewicz/colors-accessibility" alt="Repository Size" />
  </a>
  <a href="/ISSUES">
    <img src="https://img.shields.io/bitbucket/issues/phryniewicz/colors-accessibility" alt="Issues" />
  </a>
  <a href="/LANGUAGES">
    <img src="https://img.shields.io/github/languages/count/phryniewicz/colors-accessibility" alt="Languages" />
  </a>
</p>

# Table of Contents:
<a name="table-of-contents"></a>

* [Introduction](#introduction)
* [Overview](#overview)
* [Installation](#installation)
* [Usage](#usage)
  * [Color](#color)
    * [Input values](#input-values)
    * [Conversions](#conversions)
    * [Relative luminance](#relative-luminance)
    * [Single space representation](#single-space-representation)
    * [Color representations](#color-representations)
    * [Values to dictionary](#values-to-dictionary)
  * [Accessibility processor](#accessibility-processor)
    * [Contrast ratio](#contrast-ratio)
    * [WCAG compliant colors representation](#wcag-compliant-colors-representation)
* [Further improvements](#further-improvements)




# Introduction
<a name="introduction"></a>
This library help to handle different color values inputs in three color spaces: `RGB`, `HSL` and `HEX`.
The main functionality is ability to check the contrast between two colors as well adjusting those colors to fit 
WCAG 2.1 specification.

# Overview
<a name="overview"></a>

This library provides two main features:
* A class ***Color*** that represents a color in many 
possible color spaces. This class provides following functionalities:
  * Validating color values for each color space - using ***Validator*** class.
  * Processing (formatting) color values for each color space - changing format of values to list, dictionary, as well
  as normalizing values in appropriate color spaces.
  * Converting color values between color spaces.
* A class ***AccessibilityProcessor*** that provides following
functionalities:
  * Calculating contrast ratio between two colors.
  * Checking if the contrast between two colors has ratio big enough for levels **AA** and **AAA**, 
  with ration **4.5:1** and **7:1** respectively. 
  * Calculating and getting color that is **AA** or **AAA** level of contrast with the given color - by changing 
  either **foreground** color, **background** color or both.


# Installation
<a name="installation"></a>
To install `colors-accessibility` library, run the following command:
```
pip install colors-accessibility
```
`color-accessibility` library does not have any dependencies.

# Usage
<a name="usage"></a>

## Color
<a name="color"></a>

***Color*** class currently handles three color spaces: `rgb`, `hsl`, and `hex`.
This class can process different type of input values. To initialize a color, following inputs are valid:

### Input values
<a name="input-values"></a>

For `rgb` color space:

<div align="center">
  <img src="colors_accessibility/static/rgb_representations.svg" alt="rgb color inputs" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


# All representations below are valid and will be processed correctly.
valid_rgb_representations = [
 [100, 20, 50], ['100', 20, 50], ['100', '20', '50'], {'red': 200, 'green': 20, 'blue': 50},
 {'r': '50', 'g': 0.2, 'b': 100}
]

colors = [
  Color('rgb', representation) for representation in valid_rgb_representations
]
```
</details>  

For `hex` color space:

<div align="center">
  <img src="colors_accessibility/static/hex_representations.svg" alt="hex color inputs" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


# All representations below are valid and will be processed correctly.
valid_hex_representations = [
 '1ad', '#1ad', '1ac4bb', '#1ac4bb', ['1ad'], ['#11aabb'],
  {'hex': '1ac4ba'}
]

colors = [
  Color('hex', representation) for representation in valid_hex_representations
]
```
</details>

For `hsl` color space:

<div align="center">
  <img src="colors_accessibility/static/hsl_representations.svg" alt="hsl color inputs" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


# All representations below are valid and will be processed correctly.
valid_hsl_representations = [
 [120, 0.1, 0.2], ['120', 20, 0.1], ['120', '20', '0.1'],
 {'hue': '0.89', 'saturation': 20, 'lightness': '0.1'}
]

colors = [
  Color('hsl', representation) for representation in valid_hsl_representations
]
```
</details>

### Conversions
<a name="Conversions"></a>

`Color` class provides functionality of converting between `rgb`, `hsl` and `hex` color spaces.
We can both calculate color values in different color space an

<div align="center">
  <img src="colors_accessibility/static/color_conversions.svg" alt="color conversions" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


color = Color('rgb', {'red': 170, 'green': 0.23, 'blue': 0})
print(color)

# Calculate coordinates in different color spaces:

hex_color_values = color.to_hex()
print(hex_color_values)

hsl_color_values = color.to_hsl()
print(hsl_color_values)

# Set color to different color space:
color.set_to('hex')
print(color)
```
</details>

### Relative luminance
<a name="relative-luminance"></a>
We also have an ability to calculate [relative luminance](https://en.wikipedia.org/wiki/Relative_luminance) of a color.

<div align="center">
  <img src="colors_accessibility/static/relative_luminance.svg" alt="relative luminance" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


color = Color('rgb', {'red': 170, 'green': 0.23, 'blue': 0})

relative_luminance = color.relative_luminance()
print(relative_luminance)
```
</details>

### Single space representation
<a name="single-space-representation"></a>
We can get color representations for the specified color. We can choose from: `rgb`, `hsl`, `hex` and `all`. With `all`
we get representation for all three color spaces.

<div align="center">
  <img src="colors_accessibility/static/get_single_space_representation.svg" alt="get single space representation" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


color = Color('rgb', [120, 53, 89])
print(color)

representation = color.get_representations('hex')
print(representation
```
</details>

### Color representations
<a name="color-representations"></a>
Here we get the representations for all three spaces.

<div align="center">
  <img src="colors_accessibility/static/get_color_representations.svg" alt="get color representations" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


color = Color('rgb', [120, 53, 89])
print(color)

representation = color.get_representations('all')

print(representation.rgb)
print(representation.hex)
print(representation.hsl)
```
</details>

### Values to dictionary
<a name="values-to-dictionary"></a>
We can format the color representation to a dictionary.

<div align="center">
  <img src="colors_accessibility/static/to_dictionary.svg" alt="values to dictionary" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


color = Color('rgb', [120, 50, 0.3])
print(color.color_values)

values_dictionary_representations = color.to_dict()
print(values_dictionary_representations)
```
</details>

## Accessibility processor
<a name="accessibility-processor"></a>

### Contrast ratio
<a name="contrast-ratio"></a>
We can calculate contrast ratio of a color.

<div align="center">
  <img src="colors_accessibility/static/accessibility_processor_contrast.svg" alt="contrast ratio" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import Color


rgb_color = Color('rgb', [120, 53, 89])
hex_color = Color('hex', '#783957')

processor = AccessibilityProcessor(rgb_color, hex_color)
print(processor)

rgb_luminance, hex_luminance = processor.get_luminance_values(
  processor.foreground_color,
  processor.background_color
)
print(rgb_luminance)
print(hex_luminance)

constrast = processor.calculate_contrast(rgb_luminance, hex_luminance)
print(contrast)
```
</details>

### WCAG compliant colors representation
<a name="wcag-compliant-colors-representation"></a>
We can get the WCAG compliant colors representation - we get them by tweaking HSL color values till contrast between the
two input colors are on adequate levels.

<div align="center">
  <img src="colors_accessibility/static/wcag_compliant_representation.svg" alt="wcag compliant representation" width=75%/>
</div>
<details>
<summary>Code</summary>

```
from colors_accessibility import AccessibilityProcessor, Color
from pprint import pprint


rgb_color = Color('rgb', [120, 198, 73])
hex_color = Color('hex', '#783957')

processor = AccessibilityProcessor(rgb_color, hex_color)
wcag_compliant_colors = processor.get_all_wcag_compliant_color()
pprint(wcag_compliant_colors.get('lightness').get('background'))
```
</details>

# Further improvements
<a name="further-improvements"></a>
Here are some of the planned further improvements:

- [ ] Add ability to get color representations for more color spaces
- [ ] Refactor the code
- [ ] Add methods to print color representations
- [ ] Add methods for easier getting well formatted color representations
- [ ] Add more tests to increase the coverage