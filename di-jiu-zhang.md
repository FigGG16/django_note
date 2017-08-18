1.全局搜索配置

配置课程的搜索，实现"或“的功能需要包含Q

```
from django.db.models import Q
```

```py
 search_keywords = request.GET.get('keywords',"")
        if search_keywords:
            # 课程搜索                              i代表不区分大小写
            all_courses = all_courses.filter(Q(name__icontains=search_keywords)|Q(desc__icontains=search_keywords)|Q(detail__icontains=search_keywor
```



