##### 1。模版继承

新建 一个base.html拷贝org\_list.html内容到里面, 把org\_\_list.html清空当成子继承

把需要继承重写的部分给包住，

```py
{% block title %}......{% endblock %}
```

需要自定义css

```py
{% block custom_css %}   {% endblock %}
```

需要自定义js

```py
{% block custom_js %} {% endblock %}
```

重置标题

```
{% block custom_bread %}
{% endblock %}
```

重置内容

```
{% block content %}
{% endblock %}
```

加载课程详情的url

```

```

