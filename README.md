# CLI builder

A simple class to ease up the process of building applications run in a terminal. For the most part, it only offers a python class which will load predefined commands, parse input, ensure the user inputs the command properly, and finally features a prebuilt "help" command which will display the name, description, and syntax of all predefined functions.

# Documentation

Creates a class:

``` python
from simple_CLI_builder import *

app = CLI()
```

## Load predefined functions 

from a json file:
``` python
from simple_CLI_builder import *

app = CLI()
app.json_to_lookup('defined_functions.json')
```

where **defined_functions.json**:
``` python
{
    "commands": [
        {
            "name": str,
            "description": str,
            "syntax": str,
            "min_args": int,
            "max_args": int,
            "defined_flags": str
        }
    ]
}
```

Only the **name** field is required. Not including the other fields will have their values defaulted to **None**. <br> <br>
If you would like to not use a **.json** and manually populate the table for whatever god forsaken reason you can create a list of objects identical to the ones seen in the **.json** file and call **app.populate_lookup(*list*)** instead of **app.json_to_lookup(*path*)**

## Parsing input

``` python
from simple_CLI_builder import *

app = CLI()
app.json_to_lookup('defined_functions.json')

while True:
    app.parse_input(input("> "))
```

Upon input, the property **app.current_command** will be populated with an instance of the class **Executed_Command()** where the class is defined as follows:

```python
class Executed_Command():
    def __init__(self):
        self.name = None
        self.args = []
        self.flags = []
        self.properties = None
```

The command's arguements can be accessed through **app.current_command.*property***. For example:

```python
from simple_CLI_builder import *

app = CLI()
app.json_to_lookup('defined_functions.json')

app.parse_input("command arg1 arg2")
print(app.current_command.args)
```

would return:

``` python
['arg1', 'arg2']
```

This works with flags as well.

To access the predefined properties of the command (such as its name, description, min and max number of args, etc) being called use **app.current_command.properties.*property***. For example:

```python
from simple_CLI_builder import *

app = CLI()
app.json_to_lookup('defined_functions.json')

app.parse_input("command arg1 arg2")
print(app.current_command.properties.name)
```

would return the **name** value of the current command found in the previously defined commands (same as the ones found in **defined_functions.json**)

From here, a simple switch statement can be used to provide functionality to the command. For example:

```python
from simple_CLI_builder import *

app = CLI()
app.json_to_lookup('defined_functions.json')

while True:
    app.parse_input(input("> "))

    match app.current_command.properties.name:
        case 'help':
            app.help() # Prints all command names along with their syntax
                       # This is a predefined function in the class

        case 'hello':
            print('Hello, world!') 
```




