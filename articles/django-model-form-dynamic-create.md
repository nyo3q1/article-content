---
title: " ModelFormを動的に生成しdictの内容をバリデートして保存する" # 記事のタイトル
emoji: "🕺" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["Python", "django"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
BlogというModelがあるとして、こんな感じに書くと任意のフィールドを更新できる👊

```python
from django.forms.models import modelform_factory


def update(instance: Blog, data: dict) -> None:
    # fields=list(data.keys()) で検証したいフィールドを指定
    BlogForm = modelform_factory(Blog, fields=list(data.keys()))
    blog_form = BlogForm(data=data, instance=instance)
    blog_form.is_valid()
    blog_form.save()
```
