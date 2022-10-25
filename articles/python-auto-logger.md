---
title: "Pythonのクラスのメソッド全てに自動でデコレーターを適用する" # 記事のタイトル
emoji: "😸" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["python"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---
関数単体へのデコレーターの適用すぐ出るけど、今回の使い方は中々無いので忘れないように残しておく。


https://stackoverflow.com/a/23726462
```
def for_all_methods(decorator):
    def decorate(cls):
        for attr in cls.__dict__: # there's propably a better way to do this
            if callable(getattr(cls, attr)):
                setattr(cls, attr, decorator(getattr(cls, attr)))
        return cls
    return decorate

@for_all_methods(mydecorator)
class C(object):
    def m1(self): pass
    def m2(self, x): pass
```

上の機能を使い自動で全てのメソッドでログを出したいのでこんな感じにする
```
def apply_logger():
    def logger(fn):
        # https://stackoverflow.com/a/6307868
        from functools import wraps

        @wraps(fn)
        def wrapper(*args, **kwargs):
            print(f"START: {fn.__name__}")

            out = fn(*args, **kwargs)

            print(f"END: {fn.__name__}")
            # Return the return value
            return out

        return wrapper

    def decorate(cls):
        for attr in cls.__dict__: # there's propably a better way to do this
            if callable(getattr(cls, attr)):
                setattr(cls, attr, logger(getattr(cls, attr)))
        return cls
    return decorate


@apply_logger()
class A:
    def a(self):
        print("a")

    def b(self):
        print("b")

    def c(self):
        print("c")


if __name__ == "__main__":
    a = A()
    a.a()
    a.b()
    a.c()
```

