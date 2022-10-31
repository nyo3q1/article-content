# 多段にネストしている class の中に特定の attribute を保持しているクラスを list で取得する

```python
def get_inner_classes(cls):
    import collections
    import inspect

    def main(cls_):
        def has_choics(cls_attribute_) -> bool:
            return hasattr(cls_attribute_, "choices")

        def cls_or_func(cls_attribute_):
            return cls_attribute_ if has_choics(cls_attribute_) else get_inner_classes(cls_attribute_)

        return [
            cls_or_func(cls_attribute) for cls_attribute in cls_.__dict__.values() if inspect.isclass(cls_attribute)
        ]

    def flatten(l):
        # ネストすればするほど list も深くなるのでフラットにする
        # https://stackoverflow.com/a/2158532
        for el in l:
            if isinstance(el, collections.abc.Iterable) and not isinstance(el, (str, bytes)):
                yield from flatten(el)
            else:
                yield el

    return list(flatten(main(cls)))
```

## 参考情報
https://stackoverflow.com/a/42326409
https://stackoverflow.com/a/2158532
