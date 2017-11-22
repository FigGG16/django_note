

```
class ArticleAdmin(object):    list_display = [ 'title', 'body', 'created_time','last_modified_time','status','abstract','views','likes','topped','category']    search_fields = ['STATUS_CHOICES', 'title','status','abstract','views','likes','topped','category']    list_filter = ['title', 'body', 'created_time','last_modified_time','status','abstract','views','likes','topped','category']    # style_fields = {"body": "ueditor"}    formfield_overrides={'body': AdminMarkdownxWidget}
```

