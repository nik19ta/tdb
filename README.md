![Logo](./logo/logo.png)

- База данных которая представляет из себя файлы json и файл db.js для удобной работы с ними 
- данные хранятся в обьектах json 





0. [Taki DataBase](#Подключение)
    1. [Подключение](#Подключение)
    2. [Изменить конфиг](#config_db)
0. [Добавить обьект](#create_obj)
1. [Удалить обьект по параметру](#del_obj)
1. [Обновление данных](#update_obj)
    1. [Обновить обьект по параметру](#update_obj)
    2. [Обновить поле](#update_field)
3. [Выбрать обьект/обьекты по параметру](#select_obj)


--- 

# Taki DataBase:

## Подключение

```js
const db = require('./path/to/db.js')
```
После импортирования файла вы можете работать с базой данных, например 

```js
db.create_obj({
    name: 'tdb',
    version: '1.0'
})
```


## config_db

> это конфиг бд

```json
{
    "encoding":"utf8", // кадировка
    "filename": "./db.json" // файл в котором находятся данные
}
```


Использование:

##  create_obj 

> Эта функция создаёт объект в массиве который мы передаём первый параметр, а вторым передаём объект который пушится в этот маси

> Для каждого объекта генерится уникальный id, по которому можно потом удалять объект
```js

db.create_obj('examples', {
    file: './name',
    log: true
})

```
Результат:

```json
{
    "examples": [{
        "file": "./name",
        "log": true,
        "id": 0
    }]
}
```


## del_obj

> Эта функция удаляет объект из массива который мы передаем по значению и ключу который мы передаём, например эта фунция удалит из масива "examples" обьект у которого id равен трём

```js
db.del_obj('examples', 'id', 3)
```

## update_obj

> Эта функция обновляет обьект/обьекты которые соответствуют требованиям, первый параметр это наименование масива, второй это ключ по которому сравнивают, третьй это значиние с которым стравнивают, а четвёртое и пятое это ключ/значение которое надо поменять/добавить новое значение

```js
db.update_obj('examples', 'id', 2, 'log', false)
```

Вот что было

```json
{
    "examples": [{
        "file": "./name",
        "log": true,
        "id": 2
    }]
}
```

Вот что стало после выполнения функции
```json
{
    "examples": [{
        "file": "./name",
        "log": false,
        "id": 2
    }]
}
```


## update_field
> Эта функция позволяет удобно обновить поле в обьекте

входные параметры: `name`, `key`, `value`

```js
// name - название обьекта => data[name]
```
```js
// key - поле которое надо обновить => data[name][key]
```
```js
// value - новое значение => data[name][key] = value
```
Например:

```js
db.update_obj('example', 'stages', 5)
```
Было:
```json
{   
    "project_status": {
        "stages": 10,
        "actual_stage": 3
    }
}
```
Стало после выполнения функции:
```json
{   
    "project_status": {
        "stages": 5,
        "actual_stage": 3
    }
}
```




## select_obj 

> Позволяет вывести объект/объекты которые соответствуют требованиям

> Если объектов соответствующих требований не будет найдено функция вернёт null

В данном примере из массива examples, выведутся все объекты с id равным 2
```js
let res = db.select_obj('examples', 'id', 2);
```

Вот результат:

```sh
[{ file: './name', log: false, id: 2 }]
```
Если элементов соответствующие требованиям будет несколько то выводится:
```sh
[
    { file: './name', log: false, id: 2 },
    { file: './name', log: false, id: 2 }
]
```
Если не будет объектов соответствующие требованиям то выведется:

```sh
null
```
