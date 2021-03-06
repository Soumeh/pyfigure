# pyfigure

Generate configuration files for classes.

## Installation

`pip install pyfigure`

## Usage

To generate a configuration file for a class, you must extend the class with `Configurable`, from the `pyfigure` module.

Then, to add options to the configuration, create a new `Config` class, and add variables of the `Option` class.

```py
class YourClass(Configurable):

    class Config:

        option_name: type = Option(
            default,
            parse,
            description,
        )

        class sub_config:

            cub_option: type = Option()
```

### Where the arguments are as follows:

|argument|description
|-|-
`default`|The default value of the option
`description`|A comment to explain what the option is for
`parse`|A function that parses the given value, returns a new value or throws an exception

`file.py`
```py
from pyfigure import Configurable, Option
from typing import Literal, List

class ExampleClass(Configurable):

    #config_file = 'other_file.toml'

    class Config:
        string_option: str = Option('default', "Any string works here.")
        integer_option: int = Option(24, "Any integer here.")
        float_option: float = Option(15.55, "Any float.")
        bool_option: bool = Option(True, "Any boolean.")
        list_option: list = Option(['e', 'a', 'o'], "A list of things.")
        int_list_option: List[int] = Option([1, 2, 3], 'A list of integers.')
        choice_option: Literal['spam', 'eggs'] = Option('spam', "Either 'spam' or 'eggs'")
        class sub_config:
            sub_option: str = Option('this value in a sub value')
            class sub_sub_config:
                sub_sub_option: str = Option('this value is a sub value of a sub value')

    #def __init__(self):
    #    Configurable.__init__(self)

ec = ExampleClass()
print(ec.config)
```

This class, when initialized, will print:

```py
{
    'string_option': 'default',
    'integer_option': 24,
    'float_option': 15.55,
    'bool_option': True,
    'list_option': ['e', 'a', 'o'],
    'int_list_option': [1, 2, 3],
    'choice_option': 'spam',
    'sub_config': {
        'sub_option': 'this value in a sub value',
        'sub_sub_config': {
            'sub_sub_option': 'this value is a sub value of a sub value'
        }
    }
}
```

And create a new file:

`file.toml`
```toml
string_option = "default" # Any string works here.
integer_option = 24 # Any integer here.
float_option = 15.55 # Any float.
bool_option = true
list_option = ["e", "a", "o"] # A list of things.
int_list_option = [1, 2, 3] # A list of integers.
choice_option = "spam" # Either 'spam' or 'eggs'

[sub_config]
sub_option = "this value in a sub value"

[sub_config.sub_sub_config]
sub_sub_option = "this value is a sub value of a sub value"
```
