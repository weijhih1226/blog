---
title: Python入門
categories: [Python]
tags: [Python]
---

## 物件導向

定義類別(Class)。

```python
# 人類別
class Person:
    id = 0  # 類別屬性(class attribute)

    # 建構式
    def __init__(self, name='未命名', age=None, gender='未知'):
        self.name = name        # 實例／資料屬性(instance/data attribute)
        self.age = age
        self.gender = gender

    def set_age(self , age):
        self.age = age

    def get_age(self):
        return self.age
```

物件導向三大特色：封裝、繼承、多型。

## 魔術方法(Magic Method)

利用內建函數`dir()`查詢某個物件的全部屬性，會發現屬性當中很多以`__xxx__`命名，。

```python
>>> dir(int)
```

Python在print某物件時，預設為顯示該實體物件所在記憶體位址，是因為其會先呼叫該物件的`__str__()`，若未定義才會再呼叫`__repr__()`，而預設兩者都是該物件的記憶體位址。

```python
class NewString:
    def __repr__(self):
        return 'representation'
    def __str__(self):
        return 'string'

print([Object Name].__str__())
```

```python
class NewString:
    def __init__(self):
        print(self)
    def __repr__(self):
        return 'representation'
    def __str__(self):
        return 'string'
```

## 斷言(Assertion)


## 參考資料
- https://docs.google.com/document/d/1QKmxsHHiL-ywRXtzX3cKTQMrPMPZ8KoX_yWwJlyzquo/edit
- https://docs.google.com/document/d/1370fsW9a9YBS6QGaMLqzrY9GnDI9p_f1NXkvjtkbamk/edit