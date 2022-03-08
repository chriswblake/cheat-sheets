
## Testing
```python
# Basic Modules
import unittest

# Pip Modules

# Project modules

class StageTest(unittest.TestCase):
    def setUp(self):
        self.template_thing = "hello world"
    def tearDown(self):
        del self.template_thing

    def test_my_method(self):
        # Define
        my_input = self.template_thing

        # Process
        result = my_method(my_input)

        # Assert
        self.assertEqual(result, "hello-world")
```



## Typing
```python
# Dictionary
from typing import Dict
data:Dict[datetime, Device.StageData] = {}

# Method input/return
def greeting(name: str) -> str:
    return 'Hello ' + name

# Complex input
from typing import Union
def add_data(self, date_time:Union[datetime,int], speed_rpm:float):
    pass
```


## References
- typing - https://docs.python.org/3/library/typing.html