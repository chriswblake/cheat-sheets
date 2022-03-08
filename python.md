
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