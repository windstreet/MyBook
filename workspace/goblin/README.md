# goblin finance 报表model创建


## `shell`里创建表
```python
from goblin.modules.backends.mws.models import engine
from goblin.modules.backends.mws.finance.models import *

Model.metadata.create_all(engine)
```


## 调试
#### 1、技巧：
右键目标 xml 文件，按 Alt 建，选择复制文件的 路径
```python
uri = "/Users/linrenwei/Downloads/common_events (1).xml"
```


#### 2、
```python
import xmltodict
with open(uri, "r") as f:
     body = f.read()
data = xmltodict.parse(body)
payload = data['ListFinancialEventsByNextTokenResponse']['ListFinancialEventsByNextTokenResult']['FinancialEvents']
```


#### 3、
```python
from goblin.modules.backends.mws.finance.schemas import FinancialEvents
obj = FinancialEvents(payload)
temp = obj.to_native()
```


#### 4、
```python
from goblin.modules.backends.mws.finance.utils import merge_recursive
result = merge_recursive(temp, {})
result_list = list(result)
```


#### 5、ShipmentEvent
```python
from goblin.modules.backends.mws.models import engine
from goblin.modules.backends.mws.finance.models import *
Model.metadata.create_all(engine)

import xmltodict
from goblin.modules.backends.mws.models import Session, Model
from goblin.modules.backends.mws.finance.utils import merge_recursive
from goblin.modules.backends.mws.finance.schemas import ShipmentEvent as ShipmentEventSchema
from goblin.modules.backends.mws.finance.models import ShipmentEvent

session = Session()
uri = "/Users/linrenwei/Downloads/common_events (1).xml"

with open(uri, "r") as f:
     body = f.read()
data = xmltodict.parse(body)

_payload = data['ListFinancialEventsByNextTokenResponse']['ListFinancialEventsByNextTokenResult']['FinancialEvents']
_payload = _payload['ShipmentEventList']['ShipmentEvent']
for payload in _payload:
    obj = ShipmentEventSchema(payload)
    temp = obj.to_native()
    result = merge_recursive(temp, {})
    # result_list = list(result)
    for i in result:
        try:
            gg = ShipmentEvent(**i)
        except TypeError:
            import ipdb;ipdb.set_trace()
        else:
            session.add(gg)
session.commit()
```
