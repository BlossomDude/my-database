---
Owner: Blossom Dude
Last edited time: 2023-11-15T17:41
Created time: 2023-11-15T17:41
---
Доп. вопросы

Что такое lambda выражение в Python?

Назови пример использования lambda выражений.

Что такое декораторы?

Источники

Распаковки `*args` (_arguments_) и `**kwargs` (_keyword arguments_) используется для передачи любого количества аргументов в параметры функций.

# Доп. вопросы

Раз мы подняли тему функций, то еще вопросы:

## Что такое lambda выражение в Python?

Lambda выражение - это анонимная функция, которую можно вызывать на месте и передавать туда аргументы. Синтаксис:

```Python
# lambda <arguments>: <expression>
lambda : print("Why do I exist?")
lambda x: x ** 2
lambda x, y: x + y
```

### Назови пример использования lambda выражений.

Например у нас есть список строк состоящих из имен и фамилий. Мы хотим отсортировать этот список по фамилиям, тогда мы можем использовать метод `list.sort()` и аргумент `key`, а в key передать `lambda`:

```Python
l = ["Sergey Tsutsin", "Egor Rogov", "Dmitry Novikov", "Vasya Pupkin"]
l.sort(key=lambda name: name.split(" ")[-1].lower())
print(l) # ['Dmitry Novikov', 'Vasya Pupkin', 'Egor Rogov', 'Sergey Tsutsin']
```

## Что такое декораторы?

Декоратор - функция-обертка для функции. Используется для добавления логики перед выполнением основной функции. Если в оборачиваемую функцию нужно передать аргументы, то используется синтаксис распаковки `*args` и `**kwargs`. Например мы хотим засекать время выполнения функции:

```Python
def benchmark(func):
    import time
    
    def wrapper(*args, **kwargs):
        start = time.time()
        return_value = func(*args, **kwargs)
        end = time.time()
        print('[*] Время выполнения: {} секунд.'.format(end-start))
        return return_value
    return wrapper

@benchmark
def fetch_webpage(url):
    import requests
    webpage = requests.get(url)
    return webpage.text

webpage = fetch_webpage('https://google.com')
print(webpage)
```

# Источники

1. (видео) **Как использовать *args и **kwargs в python?**: [https://www.youtube.com/watch?v=gmoJdMBmcyk](https://www.youtube.com/watch?v=gmoJdMBmcyk)
2. (видео) **Lambda Expressions & Anonymous Functions**: [https://www.youtube.com/watch?v=25ovCm9jKfA](https://www.youtube.com/watch?v=25ovCm9jKfA)
3. (видео) **Как устроены декораторы в python?**: [https://www.youtube.com/watch?v=tNAoiptzuuo](https://www.youtube.com/watch?v=tNAoiptzuuo)
4. (статья) **Декораторы в Python: понять и полюбить**: [https://tproger.ru/translations/demystifying-decorators-in-python/](https://tproger.ru/translations/demystifying-decorators-in-python/)