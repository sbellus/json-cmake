## JSON parser for CMake

Module JSONParser.cmake contains macro `sbeParseJson` to parse given JSON string. 
Macro fills given variable with list of all names in json string. For each name it creates variable and sets the value from json string.
To clear created variables use nacro `sbeClearJson`.

### Example

JSON to parse:
``` json
{"menu": {
    "header": "SVG Viewer",
    "items": [
        {"id": "Open"},
        {"id": "OpenOld", "label": null },
        {"id": "OpenNew", "label": "Open New"},
        null,
        {"id": "ZoomIn", "label": ["Zoom In", "Zoom At"] },
        {"id": "ZoomOut", 
               "label": 
	         { 
	         "short": ["zo", "zout"], 
	         "long":"Zoom Out"
	         }
    	},
        {"id": "OriginalView", "label": "Original View"},
    	null
     ],
    "elements" : ["one", "two", { "number":"three", "Desc": "Number" }, null ]
}}
```

Let assume that above JSON is stored in variable `jsonTest` in CMake. Then it can be parsed with following lines of code.
``` cmake
sbeParseJson(example "${jsonTest}")

# Now you can use parsed variables.
foreach(var ${example})
    message("${var} = ${${var}}")
endforeach()

# When you are done, clean parsed variables
sbeClearJson(example)
```

Macro `sbeParseJson` creates following variables and its values.
```
example.menu.header = SVG Viewer
example.menu.items = 0;1;2;3;4;5;6;7
example.menu.items[0].id = Open
example.menu.items[1].id = OpenOld
example.menu.items[1].label = null
example.menu.items[2].id = OpenNew
example.menu.items[2].label = Open New
example.menu.items[3] = null
example.menu.items[4].id = ZoomIn
example.menu.items[4].label = 0;1
example.menu.items[4].label[0] = Zoom In
example.menu.items[4].label[1] = Zoom At
example.menu.items[5].id = ZoomOut
example.menu.items[5].label.short = 0;1
example.menu.items[5].label.short[0] = zo
example.menu.items[5].label.short[1] = zout
example.menu.items[5].label.long = Zoom Out
example.menu.items[6].id = OriginalView
example.menu.items[6].label = Original View
example.menu.items[7] = null
example.menu.elements = 0;1;2;3
example.menu.elements[0] = one
example.menu.elements[1] = two
example.menu.elements[2].number = three
example.menu.elements[2].Desc = Number
example.menu.elements[3] = null
```

