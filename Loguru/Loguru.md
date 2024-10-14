# Loguru

#### 

#### bug等级

```python
from loguru import logger

logger.debug("debug")
logger.info("info")
logger.warning("warning")
logger.error("error")
logger.critical("critical")

```



#### 文件设置

```python
logger.add('logger/{time}.log', rotation='1 week', retention=20)
```



#### 异常捕获

```python
@logger.catch()
def logger_text_01(x,y,z):
    return 1 / (x + y + z)
```

