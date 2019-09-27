# Guide for adding gradient in gradients.json file

### Color groups

```
['#259BE5', '#E5E9EC', '#7BCC9B', '#F9AFAD', '#FC96D3', '#8B56E9']
```
### JSON structure
```
"name": "Winter Neva",  // gradient name
"favorite": false,  // default value
"index": 1,  // gradient index in array ( check last index in json file )
"deg": 120,  // gradient degree ( top - zero degree )
"group": [ "#259BE5" ],  // check color groups, may contains more than one value
"gradient": [  // color array, may contains more than one object
    {
        "color": "#a1c4fd",  // HEX
        "pos": 0 // color position in gradient
    },
    {
        "color": "#c2e9fb",
        "pos": 100
    }
]
```