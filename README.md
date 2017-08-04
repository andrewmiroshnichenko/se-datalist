[![Published on webcomponents.org](https://img.shields.io/badge/webcomponents.org-published-blue.svg)](https://www.webcomponents.org/element/andrewmiroshnichenko/se-datalist)
# se-datalist

Polymer wrapper around native **select** and **datalist** tags with browser-dependent appearance. If brower don't support **datalist** tag it will fall back to **select** which is supported fairly well. Note that all bugs, that are belong to native **datalist** tag will appear on this custom element as well, since it just refer to general tag support.

## Install
```
$ bower install se-datalist
```
Make sure you have the [Polymer CLI](https://www.npmjs.com/package/polymer-cli) installed. Then run `polymer serve` to serve your element locally.

## Viewing component

```
$ polymer serve
```

## Running tests

```
$ polymer test
```
# API
##Component have five public properties.
1. List

|                             |         |
| ---                         | ---     |
| **Property name**           | list    |
| **Property type**           | Array   |
| **Default value**           | []      |
| **Can be set from html**    | No      |
| **Corresponding attribute** |         |
|                             |         |

List of the component options. Empty by default, read-only. All manipulations with the list should be accomplished by corresponding component methods(addOptions, removeOptions and removeAllOptions). So, by default, component is initializing without options, and then will populated using JS.
```html
      <se-datalist></se-datalist>
```
```javascript
      var customEl = document.querySelector('se-datalist');
      customEl.list = ['1', '2', 'coconut'];
      customEl.list;  // returns empty Array, because list property is read-only 
      customEl.addOptions(['a', 'b']);
      customEl.list;  // returns Array ['a', 'b']
      customEl.removeOptions(['a']);
      customEl.list;  // returns Array ['b']
 ```
2. Disabled

|                             |            |
| ---                         | ---        |
| **Property name**           | disabled   |
| **Property type**           | Boolean    |
| **Default value**           | false      |
| **Can be set from html**    | Yes        |
| **Corresponding attribute** | disabled   |
|                             |            |

If true - disables possibility of user interaction with component. But component’s value still will be changeable from code.
**Warning:** presence of this property as attribute of `<se-datalist>` tag will lead to disabling of component, even if it's value will be `false`.

```html
      <se-datalist disabled="false"></se-datalist>
```
```javascript
      var customEl = document.querySelector('se-datalist');
      customEl.disabled; // returns Boolean true
      customEl.disabled = false;
      customEl.disabled; // returns Boolean false
 ```
3. Placeholder

|                             |             |
| ---                         | ---         |
| **Property name**           | placeholder |
| **Property type**           | String      |
| **Default value**           | -           |
| **Can be set from html**    | Yes         |
| **Corresponding attribute** | placeholder |
|                             |             |

String that is shown as component’s placeholder when component’s value is empty. Length of this property is limited to 20 characters, bigger placeholder will explicitely cut to this length. When component’s value changes from empty to non-empty placeholder should move to “upper left corner” of the component.

```html
      <se-datalist></se-datalist>
```
```javascript
      var customEl = document.querySelector('se-datalist');
      customEl.placeholder; // returns String ''
      customEl.placeholder = 'Select an option';
      customEl.placeholder; // returns String 'hh:mm'
 ```
 4. Selected option

|                             |                |
| ---                         | ---            |
| **Property name**           | selectedOption |
| **Property type**           | String         |
| **Default value**           | ''             |
| **Can be set from html**    | No             |
| **Corresponding attribute** |                |
|                             |                |
Current option that is selected as component’s value. Can't be set from HTML because we can't set list from there. On attempt to set non-existing (in `list` property) selected option this property will be set to empty Sring if `list` property is empty or if no option is selected yet. If some selected option was already selected - it will remain as `selectedOption` property value.
If current `selectedOption` will removed from `list` - first option in list become selected. If all options will be removed, `selectedOption` become empty.
```html
      <se-datalist></se-datalist>
```
```javascript
      var customEl = document.querySelector('se-datalist');
      customEl.addOptions(['One', 'Two', 'Three']); // Setup component’s list of options
      customEl.selectedOption = 'Two';
      customEl.selectedOption; // returns String 'Two'
      customEl.removeOptions(['Two']);
      customEl.selectedOption; // returns String 'One'
      customEl.selectedOption = 'Four';
      customEl.selectedOption; // returns String 'One'
      customEl.removeOptions(['One', 'Three']);
      customEl.selectedOption; // returns String ''
 ```
 5. Is select

|                             |                |
| ---                         | ---            |
| **Property name**           | isSelect       |
| **Property type**           | Boolean        |
| **Default value**           | false          |
| **Can be set from html**    | Yes            |
| **Corresponding attribute** | is-select      |
|                             |                |
With this property set to **true** component will initialize as **select** even if **datalist** is available in current browser. In order to achieve this behavior you should set it upon initialization as attribute of `se-datalist` tag.
```html
      <se-datalist is-select></se-datalist>
```
## Methods

### addOptions(optionsList: *Array*) => *void*/*false*
This method accepts array of options, and populates the component’s list property. If option from `optionsList` isn't already present in `list` - component will add it to the end of `list`, and if it's duplicate of existing option - method will return `false`.
```html
      <se-datalist></se-datalist>
```
```javascript
      var customEl = document.querySelector('se-datalist');
      customEl.addOptions(['One', 'Two', 'Three']);
      customEl.list; // returns Array ['One', 'Two', 'Three']
      customEl.addOptions(['Four']);
      customEl.list; // returns Array ['One', 'Two', 'Three', 'Four']
      customEl.addOptions(['Four', 'Five']);  // returns Boolean 'false' on adding option 'Four'
      customEl.list; // returns Array ['One', 'Two', 'Three', 'Four', 'Five']
```
### removeOptions(optionsList: *Array*) => *void*
This method accepts array of options, and removes them from the component’s list property. If option from `optionsList` isn't already present in `list` - method will return `false`, and if it's existing option - component will remove it from `list`.
```html
      <se-datalist></se-datalist>
```
```javascript
      var customEl = document.querySelector('se-datalist');
      customEl.addOptions(['One', 'Two', 'Three']);
      customEl.list; // returns Array ['One', 'Two', 'Three']
      customEl.removeOptions(['Two']);
      customEl.list; // returns Array ['One', 'Three']
      customEl.removeOptions(['Four']); // returns Boolean 'false' on adding option 'Four'
      customEl.list; // returns Array ['One', 'Three']
      customEl.addOptions(['One', 'Three']);
      customEl.list; // returns Array []
```
### removeAllOptions() => *void*
Completely removes all options from the component’s `list` property.
```html
      <se-datalist></se-datalist>
```
```javascript
      var customEl = document.querySelector('se-datalist');
      customEl.addOptions(['One', 'Two', 'Three']);
      customEl.list; // returns Array ['One', 'Two', 'Three']
      customEl.removeAllOptions();
      customEl.list; // returns Array []
```
## Styling

The following custom properties are available for styling:

| Custom property                  | Description                                                         | Default                        |
| ---                              | ---                                                                 | ---                            |
| `--sedatalist-text-color`        | Color of datalist's displayed value                                 | Browser-default(black)         |
| `--sedatalist-bottom-line`       | Style of component’s bottom line. Other borders are disabled        | `1px solid rgba(0, 0, 0, .12)` |
| `--sedatalist-placeholder-color` | Color of placeholder text                                           | `#999999`                      |
| `--sedatalist-font-size`         | Charachters font size. Component dimesions will scale accordingly   | `16px`                         |
| `--sedatalist-font-family`       | Font family of component typography                                 | `Roboto, sans-serif`           |