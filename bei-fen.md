app目录的建立

#### 1.移动四个app目录到新建的apps目录下 ![](/assets/Snip20170720_8.png)

#### 2.会出现这个问题

![](/assets/Snip20170720_9.png)

#### 3.解决办法

![](/assets/Snip20170720_10.png)

当我们使用命令行python manage.py runserver时会报错，解决办法：

在setting里把app加到目录下

```python
    import os
    import sys

    # Build paths inside the project like this: os.path.join(BASE_DIR, ...)
    BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    sys.path.insert(0,os.path.join(BASE_DIR,'apps'))
```






