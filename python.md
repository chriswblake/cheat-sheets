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

    def test_multiple(self):
        # Define
        allowed_error = 0.001
        subtests = [
            # infow temp F, expected result
            (50, 253.5),
            (100, 503.5),
            (150, 753.5),
            (200, 1003.5),
        ]

        # Process
        calculated_value = {}
        for temp_f, _ in subtests:
            calculated_value[temp_f] = calculate_result(temp_f)

        # Assert
        for temp_f, expected_value in subtests:
            with self.subTest(temp_f = temp_inflow_f):
                self.assertAlmostEqual(calculated_value[temp_f], expected_value, delta = allowed_error)
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

## Random - Choice

```python
import random
cities = ["Atlanta", "New York", "Miami", "Chicago"]
source = random.choice(cities)
destination = random.choice([c for c in cities if c != source])
```

## References

- Fake data - https://pypi.org/project/Faker/
- typing - https://docs.python.org/3/library/typing.html
