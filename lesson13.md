# Лекция 13: Практика Python. Структура приложения

![Python Practice](./images/practice_13.png)

## Практика: собираем знания вместе

Мы уже изучили основные конструкции Python, функции, модули, работу с файлами, JSON, поиск, сортировку и Git.

Теперь наша задача - научиться использовать эти инструменты **вместе внутри одного приложения**.

На этой лекции мы не будем вводить много новой теории. Вместо этого создадим небольшой проект и разберём, как организовать его структуру, разделить код между файлами и постепенно добавлять новый функционал.

В качестве проекта создадим **Movie Manager** - консольное приложение для управления собственной коллекцией фильмов.

Программа будет уметь:

- хранить фильмы в JSON;
- показывать список фильмов;
- добавлять новые фильмы;
- искать фильмы;
- изменять и удалять их;
- фильтровать фильмы;
- сортировать данные;
- считать небольшую статистику.

Главная цель - не просто написать работающую программу, а научиться организовывать код так, чтобы проект было удобно читать, изменять и расширять.

---

## Техническое задание

Перед началом разработки важно понимать, **что именно должна делать программа**.

В реальной разработке программист обычно не начинает работу сразу с написания кода. Сначала необходимо изучить требования к проекту, определить основные данные, функциональность и ограничения.

Сегодня мы будем разрабатывать консольное приложение **Movie Manager** для управления личной коллекцией фильмов.

### Основная задача

Пользователь должен иметь возможность хранить свою коллекцию фильмов и управлять ею через консольное меню.

Программа должна позволять:

- добавлять фильмы;
- просматривать коллекцию;
- искать фильмы;
- изменять информацию о фильмах;
- удалять фильмы;
- фильтровать коллекцию;
- сортировать фильмы;
- получать статистику.

Все изменения должны сохраняться между запусками программы.

---

### Данные фильма

Каждый фильм должен содержать следующие поля:

- `id` - уникальный идентификатор фильма;
- `title` - название фильма;
- `year` - год выхода;
- `genre` - жанр;
- `rating` - рейтинг от `0` до `10`;
- `watched` - просмотрен фильм или нет.

Пример:

```python
{
    "id": 1,
    "title": "Interstellar",
    "year": 2014,
    "genre": "Sci-Fi",
    "rating": 8.7,
    "watched": True
}
```

---

### Главное меню

После запуска программы пользователь должен увидеть меню:

```text
=== MOVIE MANAGER ===

1. Показать все фильмы
2. Добавить фильм
3. Найти фильм
4. Изменить фильм
5. Удалить фильм
6. Фильтрация
7. Сортировка
8. Статистика
0. Выход
```

После выполнения выбранной операции программа должна снова возвращать пользователя в главное меню.

Работа программы завершается только после выбора:

```text
0. Выход
```

---

### Просмотр фильмов

Пользователь должен иметь возможность посмотреть все фильмы из коллекции. Для каждого фильма необходимо вывести основную информацию:

```text
ID: 1
Название: Interstellar
Год: 2014
Жанр: Sci-Fi
Рейтинг: 8.7
Просмотрен: Да
```

Если коллекция пустая, программа должна вывести соответствующее сообщение.

---

### Добавление фильма

Пользователь должен иметь возможность добавить новый фильм.

Необходимо запросить:

```text
Название
Год
Жанр
Рейтинг
Статус просмотра
```

Поле `id` пользователь вводить не должен. Программа должна автоматически создавать уникальный `id` для каждого нового фильма.

---

### Поиск фильма

Необходимо реализовать два варианта поиска.

#### Поиск по ID

Пользователь вводит `id` и получает конкретный фильм. Если фильма с таким `id` нет:

```text
Фильм с таким ID не найден.
```

#### Поиск по названию

Поиск не должен зависеть от регистра.

Например:

```text
interstellar
Interstellar
INTERSTELLAR
```

должны находить один и тот же фильм. Также необходимо поддерживать поиск по части названия.

Например:

```text
star
```

может найти:

```text
Interstellar
```

---

### Изменение фильма

Пользователь должен выбрать фильм по `id` и иметь возможность изменить его данные:

- название;
- год;
- жанр;
- рейтинг;
- статус просмотра.

`id` фильма изменять нельзя.

Если фильм не найден, программа должна вывести соответствующее сообщение.

---

### Удаление фильма

Пользователь должен иметь возможность удалить фильм по `id`. После удаления данные должны быть сохранены. Если фильма с указанным `id` нет:

```text
Фильм с таким ID не найден.
```

---

### Фильтрация

Программа должна позволять получить:

- фильмы определённого жанра;
- только просмотренные фильмы;
- только непросмотренные фильмы.

Например:

```text
Жанр: Sci-Fi
```

должен вывести все фильмы этого жанра.

---

### Сортировка

Пользователь должен иметь возможность сортировать фильмы:

- по названию;
- по году выхода;
- по рейтингу.

Для рейтинга должна быть возможность получить фильмы от самого высокого рейтинга к самому низкому.

---

### Статистика

Программа должна показывать:

- общее количество фильмов;
- количество просмотренных фильмов;
- количество непросмотренных фильмов;
- средний рейтинг фильмов;
- фильм с самым высоким рейтингом.

Пример:

```text
=== СТАТИСТИКА ===

Всего фильмов: 12
Просмотрено: 8
Не просмотрено: 4
Средний рейтинг: 7.8

Лучший фильм:
Interstellar - 8.7
```

---

### Хранение данных

Фильмы должны храниться в JSON-файле:

```text
movies.json
```

При запуске программы данные необходимо загрузить из файла.

После:

- добавления;
- изменения;
- удаления;

данные необходимо сохранить. Если файла ещё не существует, приложение должно корректно запуститься с пустой коллекцией.

---

### Проверка пользовательского ввода

Программа не должна завершаться с ошибкой при неправильном вводе пользователя.

Необходимо проверить:

- название фильма не должно быть пустым;
- год должен быть числом;
- рейтинг должен быть числом;
- рейтинг должен находиться в диапазоне от `0` до `10`;
- `id` должен быть уникальным;
- выбранный пункт меню должен существовать.

При ошибке пользователь должен получить понятное сообщение и возможность повторить ввод.

---

### Требования к структуре

Приложение нельзя полностью размещать в одном файле `main.py`. Код необходимо разделить на логические части.

Отдельно должна находиться логика:

- взаимодействия с пользователем;
- работы с фильмами;
- работы со статистикой;
- загрузки и сохранения данных.

Конкретную структуру проекта определим после анализа технического задания.

---

### Git

Проект необходимо вести с использованием Git. Изменения должны сохраняться отдельными осмысленными коммитами.

Например:

```text
Create project structure
Add JSON storage
Add movie creation
Add movie search
Add filters and sorting
Add statistics
```

Также в проекте должен присутствовать файл:

```text
.gitignore
```

После того как техническое задание изучено, можно переходить к его разбору и определению структуры будущего приложения.

---

## Разбираем техническое задание

Перед тем как писать код, важно разобрать техническое задание и понять, **из каких частей будет состоять приложение**.

Наша задача - не сразу создавать функции и файлы, а сначала определить:

- с какими данными работает программа;
- где эти данные будут храниться;
- какие действия должен выполнять пользователь;
- какие части логики можно разделить между собой.

### Основная сущность приложения

В нашем приложении основная сущность - **фильм**.

Каждый фильм содержит:

```python
{
    "id": 1,
    "title": "Interstellar",
    "year": 2014,
    "genre": "Sci-Fi",
    "rating": 8.7,
    "watched": True
}
```

Один фильм удобно представить словарём.

Но приложение должно хранить несколько фильмов, поэтому коллекцию можно представить списком словарей:

```python
movies = [
    {
        "id": 1,
        "title": "Interstellar",
        "year": 2014,
        "genre": "Sci-Fi",
        "rating": 8.7,
        "watched": True
    },
    {
        "id": 2,
        "title": "Dune",
        "year": 2021,
        "genre": "Sci-Fi",
        "rating": 8.0,
        "watched": False
    }
]
```

Получаем:

```text
Movie
  ↓
dict
  ↓
list[dict]
```

---

### Где хранить данные?

Если хранить фильмы только в списке:

```python
movies = []
```

после завершения программы все данные будут потеряны. По техническому заданию фильмы должны сохраняться между запусками приложения.

Для этого будем использовать JSON-файл:

```text
data/
└── movies.json
```

Работа с данными будет выглядеть следующим образом:

```text
movies.json
     ↓
загрузка данных
     ↓
список фильмов
     ↓
работа программы
     ↓
сохранение данных
     ↓
movies.json
```

Значит, в приложении понадобится отдельная логика для:

- загрузки данных;
- сохранения данных.

---

### Какие операции нужны приложению?

Из технического задания можно выделить несколько групп операций.

#### Работа с фильмами

Приложение должно уметь:

- показывать фильмы;
- добавлять фильм;
- искать фильм;
- изменять фильм;
- удалять фильм;
- фильтровать фильмы;
- сортировать фильмы.

Все эти операции относятся непосредственно к работе с коллекцией фильмов.

---

#### Статистика

Отдельная группа задач связана с анализом данных:

- количество фильмов;
- количество просмотренных фильмов;
- количество непросмотренных фильмов;
- средний рейтинг;
- фильм с самым высоким рейтингом.

Эта логика не изменяет фильмы, а только анализирует существующие данные.

---

#### Работа с файлами

Также приложение должно:

- загружать фильмы из JSON;
- сохранять фильмы в JSON.

Работа с файлами - отдельная задача, которая не относится напрямую к поиску, сортировке или статистике.

---

#### Взаимодействие с пользователем

Нам также понадобится часть программы, которая будет:

- показывать меню;
- получать выбор пользователя;
- запрашивать данные;
- вызывать необходимые функции;
- выводить результат.

---

## Разделяем приложение на части

После анализа технического задания можно выделить четыре основные части:

```text
Movie Manager
│
├── Пользовательский интерфейс
│
├── Работа с фильмами
│
├── Статистика
│
└── Работа с файлами
```

Теперь можно определить структуру проекта:

```text
movie_manager/
├── main.py
│
├── data/
│   └── movies.json
│
├── services/
│   ├── movies.py
│   └── statistics.py
│
├── utils/
│   └── storage.py
│
├── README.md
└── .gitignore
```

Каждая часть проекта получает свою ответственность:

```text
main.py
→ запуск приложения
→ меню
→ взаимодействие с пользователем

services/movies.py
→ добавление фильмов
→ поиск
→ изменение
→ удаление
→ фильтрация
→ сортировка

services/statistics.py
→ статистика по фильмам

utils/storage.py
→ чтение JSON
→ сохранение JSON

data/movies.json
→ хранение данных
```

Важно понимать, что такая структура появилась не случайно. Мы не создаём папки `services`, `utils` и `data` просто потому, что так принято.

Сначала мы разобрали техническое задание, определили данные и функциональность приложения, после чего разделили разные задачи между отдельными частями проекта.

Общий процесс проектирования выглядит так:

```text
Техническое задание
        ↓
Определяем данные
        ↓
Определяем функциональность
        ↓
Разделяем задачи по ответственности
        ↓
Создаём структуру проекта
        ↓
Начинаем писать код
```

Теперь у нас есть понятный план приложения, и можно переходить к созданию проекта.

---

## Создаём структуру проекта

Мы разобрали техническое задание, определили основные части приложения и теперь можем переходить к разработке. Начнём с создания структуры проекта:

```text
movie_manager/
├── main.py
│
├── data/
│   └── movies.json
│
├── services/
│   ├── movies.py
│   └── statistics.py
│
├── utils/
│   └── storage.py
│
├── README.md
└── .gitignore
```

Каждая часть проекта будет отвечать за свою задачу:

```text
main.py
→ запуск приложения
→ главное меню
→ взаимодействие с пользователем

services/movies.py
→ добавление фильмов
→ поиск
→ изменение
→ удаление
→ фильтрация
→ сортировка

services/statistics.py
→ статистика по фильмам

utils/storage.py
→ загрузка данных из JSON
→ сохранение данных в JSON

data/movies.json
→ хранение коллекции фильмов

README.md
→ описание проекта

.gitignore
→ файлы, которые Git не должен отслеживать
```

На данном этапе файлы могут оставаться пустыми. В `movies.json` создадим пустой список:

```json
[]
```

Он будет означать, что пока в нашей коллекции нет ни одного фильма.

---

## Настраиваем .gitignore

Добавим в `.gitignore` файлы и директории, которые не должны попадать в Git:

```gitignore
__pycache__/
*.pyc

.venv/
venv/

.vscode/
.idea/

.env
```

---

## Создаём Git-репозиторий

Git мы уже изучили, поэтому дальше будем использовать его как обычный инструмент разработки. Инициализируем репозиторий:

```bash
git init
```

Проверим состояние:

```bash
git status
```

Добавим созданную структуру проекта:

```bash
git add .
```

Создадим первый коммит:

```bash
git commit -m "Create project structure"
```

Теперь у нас есть начальная версия проекта, к которой всегда можно вернуться.

---

## С чего начинать разработку?

Главное меню пока писать рано. Сначала программе необходимо научиться получать данные о фильмах и сохранять их.Поэтому разработку начнём с нижнего уровня приложения:

```text
movies.json
      ↕
utils/storage.py
      ↕
services/
      ↕
main.py
```

Сначала реализуем две основные операции:

```text
load_movies()
→ загрузить фильмы из JSON

save_movies()
→ сохранить фильмы в JSON
```

После этого остальные части приложения смогут работать уже с настоящими данными.

Переходим к `utils/storage.py`.

---

## Работа с данными: storage.py

Теперь приложению нужно научиться **загружать фильмы из JSON-файла и сохранять изменения обратно**.

Эта логика не относится напрямую к фильмам, поиску или статистике, поэтому вынесем её в отдельный файл:

```text
utils/
└── storage.py
```

`storage.py` будет отвечать только за работу с хранилищем данных.

Нам понадобятся две функции:

```text
load_movies()
→ загрузить фильмы

save_movies()
→ сохранить фильмы
```

---

### Путь к файлу с данными

Наш JSON-файл находится здесь:

```text
data/
└── movies.json
```

В начале `storage.py` создадим переменную с путём к файлу:

```python
import json


FILE_PATH = "data/movies.json"
```

Название `FILE_PATH` записано в верхнем регистре, потому что значение этой переменной мы не планируем изменять во время работы программы.

---

### Загружаем фильмы

Создадим функцию:

```python
def load_movies():
    with open(FILE_PATH, "r", encoding="utf-8") as file:
        return json.load(file)
```

Полный код:

```python
import json


FILE_PATH = "data/movies.json"


def load_movies():
    with open(FILE_PATH, "r", encoding="utf-8") as file:
        return json.load(file)
```

Функция:

1. открывает `movies.json`;
2. читает JSON;
3. преобразует данные в Python;
4. возвращает список фильмов.

Например, если в `movies.json` находится:

```json
[
    {
        "id": 1,
        "title": "Interstellar",
        "year": 2014,
        "genre": "Sci-Fi",
        "rating": 8.7,
        "watched": true
    }
]
```

после:

```python
movies = load_movies()
```

переменная `movies` будет содержать обычный список Python:

```python
[
    {
        "id": 1,
        "title": "Interstellar",
        "year": 2014,
        "genre": "Sci-Fi",
        "rating": 8.7,
        "watched": True
    }
]
```

Получается:

```text
movies.json
     ↓
json.load()
     ↓
list[dict]
```

---

### Что делать, если файла нет?

По техническому заданию приложение должно корректно запускаться, даже если файл с фильмами ещё не существует.

Поэтому обработаем `FileNotFoundError`:

```python
def load_movies():
    try:
        with open(FILE_PATH, "r", encoding="utf-8") as file:
            return json.load(file)
    except FileNotFoundError:
        return []
```

Теперь, если `movies.json` отсутствует, функция просто вернёт пустой список:

```python
[]
```

---

### Что делать с пустым или повреждённым JSON?

Если файл существует, но внутри находится некорректный JSON, `json.load()` вызовет ошибку:

```text
JSONDecodeError
```

Например, пустой файл:

```text
```

или некорректные данные:

```text
[
    {
        "title": "Interstellar"
```

Поэтому обработаем и эту ситуацию:

```python
def load_movies():
    try:
        with open(FILE_PATH, "r", encoding="utf-8") as file:
            return json.load(file)
    except (FileNotFoundError, json.JSONDecodeError):
        return []
```

Теперь `load_movies()` гарантированно возвращает список:

```text
корректный JSON
      ↓
список фильмов

файла нет
      ↓
[]

JSON повреждён
      ↓
[]
```

---

## Сохраняем фильмы

Теперь реализуем обратную операцию.

Функция `save_movies()` будет получать список фильмов и записывать его в `movies.json`.

```python
def save_movies(movies):
    with open(FILE_PATH, "w", encoding="utf-8") as file:
        json.dump(movies, file, ensure_ascii=False, indent=4)
```

Здесь:

```python
json.dump()
```

записывает Python-объект в JSON-файл.

Параметр:

```python
ensure_ascii=False
```

позволяет нормально сохранять кириллицу.

А:

```python
indent=4
```

делает JSON читаемым.

Например:

```python
movies = [
    {
        "id": 1,
        "title": "Интерстеллар",
        "year": 2014,
        "genre": "Фантастика",
        "rating": 8.7,
        "watched": True
    }
]

save_movies(movies)
```

В `movies.json` получим:

```json
[
    {
        "id": 1,
        "title": "Интерстеллар",
        "year": 2014,
        "genre": "Фантастика",
        "rating": 8.7,
        "watched": true
    }
]
```

---

## Итоговый storage.py

Наш файл `utils/storage.py` сейчас выглядит так:

```python
import json


FILE_PATH = "data/movies.json"


def load_movies():
    try:
        with open(FILE_PATH, "r", encoding="utf-8") as file:
            return json.load(file)
    except (FileNotFoundError, json.JSONDecodeError):
        return []


def save_movies(movies):
    with open(FILE_PATH, "w", encoding="utf-8") as file:
        json.dump(movies, file, ensure_ascii=False, indent=4)
```

Теперь работа с данными полностью вынесена в отдельный модуль:

```text
data/movies.json
       ↕
utils/storage.py
       ↕
остальное приложение
```

Другим частям программы больше не нужно знать, **как именно открывается JSON-файл**.

Они смогут просто использовать:

```python
load_movies()
```

и:

```python
save_movies(movies)
```

---

## Проверяем работу

Временно откроем `main.py` и импортируем функции:

```python
from utils.storage import load_movies, save_movies
```

Загрузим данные:

```python
movies = load_movies()

print(movies)
```

Если `movies.json` содержит:

```json
[]
```

получим:

```text
[]
```

Теперь добавим тестовый фильм:

```python
movies.append(
    {
        "id": 1,
        "title": "Interstellar",
        "year": 2014,
        "genre": "Sci-Fi",
        "rating": 8.7,
        "watched": True
    }
)

save_movies(movies)
```

После запуска программы откроем:

```text
data/movies.json
```

В нём должен появиться наш фильм. После проверки тестовый код из `main.py` можно удалить.

---

## Сохраняем этап разработки в Git

Проверим изменения:

```bash
git status
```

Добавим их:

```bash
git add .
```

Создадим отдельный коммит:

```bash
git commit -m "Add JSON storage"
```

Теперь приложение умеет загружать и сохранять данные. Следующим шагом начнём реализовывать основную логику работы с фильмами в:

```text
services/movies.py
```

---

## Основная логика: services/movies.py

Хранилище данных готово. Теперь можно переходить к основной логике приложения - работе с фильмами.

Для этого используем:

```text
services/
└── movies.py
```

Важно разделять две задачи:

```text
main.py
→ общается с пользователем

services/movies.py
→ работает с данными
```

Например, функция поиска фильма не должна спрашивать `id` через `input()`. Она должна получить необходимые данные через параметры:

```python
get_movie_by_id(movies, movie_id)
```

Такую функцию можно использовать из разных частей программы независимо от консольного интерфейса.

---

### Генерация уникального ID

При добавлении фильма пользователь не должен самостоятельно вводить `id`. Сначала можно подумать о простом варианте:

```python
movie_id = len(movies) + 1
```

Но у него есть проблема. Представим список:

```text
1
2
3
4
```

Удалим фильм с `id = 3`:

```text
1
2
4
```

Теперь:

```python
len(movies) + 1
```

вернёт:

```text
4
```

и мы получим повторяющийся `id`. Поэтому найдём максимальный существующий `id` и увеличим его на `1`:

```python
def generate_movie_id(movies):
    if not movies:
        return 1

    return max(movie["id"] for movie in movies) + 1
```

Теперь:

```text
1
2
4
```

превратится в:

```text
новый ID → 5
```

---

### Поиск фильма по ID

Эта операция понадобится нам сразу в нескольких местах:

- поиск;
- изменение;
- удаление.

Поэтому вынесем её в отдельную функцию:

```python
def get_movie_by_id(movies, movie_id):
    for movie in movies:
        if movie["id"] == movie_id:
            return movie

    return None
```

Если фильм найден:

```python
movie = get_movie_by_id(movies, 2)
```

функция вернёт словарь фильма.

Если такого `id` нет:

```python
None
```

---

### Добавление фильма

Теперь реализуем добавление фильма. Функция получает уже подготовленные данные:

```python
def add_movie(movies, title, year, genre, rating, watched):
    movie = {
        "id": generate_movie_id(movies),
        "title": title,
        "year": year,
        "genre": genre,
        "rating": rating,
        "watched": watched
    }

    movies.append(movie)

    return movie
```

Обратите внимание: внутри функции нет:

```python
input()
```

и нет:

```python
print()
```

Функция отвечает только за создание фильма и добавление его в коллекцию. Получение данных от пользователя будет происходить в `main.py`.

---

### Поиск по названию

По техническому заданию поиск:

- не должен зависеть от регистра;
- должен работать по части названия.

Например:

```text
star
```

должен найти:

```text
Interstellar
```

Реализуем:

```python
def find_movies_by_title(movies, query):
    query = query.lower()
    result = []

    for movie in movies:
        if query in movie["title"].lower():
            result.append(movie)

    return result
```

Например:

```python
find_movies_by_title(movies, "star")
```

может вернуть:

```python
[
    {
        "id": 1,
        "title": "Interstellar",
        ...
    }
]
```

---

### Изменение фильма

Чтобы не создавать отдельную функцию для каждого поля, сделаем одну функцию:

```python
def update_movie(movies, movie_id, field, value):
    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        return False

    allowed_fields = {
        "title",
        "year",
        "genre",
        "rating",
        "watched"
    }

    if field not in allowed_fields:
        return False

    movie[field] = value

    return True
```

`id` специально отсутствует в `allowed_fields`. Это означает, что изменить его через функцию нельзя.

Например:

```python
update_movie(
    movies,
    1,
    "rating",
    9.0
)
```

изменит рейтинг фильма с `id = 1`.

---

### Удаление фильма

Для удаления сначала найдём фильм по `id`.

```python
def delete_movie(movies, movie_id):
    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        return False

    movies.remove(movie)

    return True
```

Если фильм удалён:

```python
True
```

Если фильма нет:

```python
False
```

Так `main.py` сможет самостоятельно решить, какое сообщение показать пользователю.

---

## Фильтрация фильмов

Следующая часть технического задания - фильтрация.

Нам нужны:

```text
фильмы определённого жанра
просмотренные фильмы
непросмотренные фильмы
```

---

### Фильтрация по жанру

```python
def filter_movies_by_genre(movies, genre):
    result = []

    for movie in movies:
        if movie["genre"].lower() == genre.lower():
            result.append(movie)

    return result
```

Например:

```python
filter_movies_by_genre(movies, "sci-fi")
```

и:

```python
filter_movies_by_genre(movies, "SCI-FI")
```

дадут одинаковый результат.

---

### Фильтрация по статусу просмотра

```python
def filter_movies_by_watched(movies, watched):
    result = []

    for movie in movies:
        if movie["watched"] == watched:
            result.append(movie)

    return result
```

Просмотренные:

```python
filter_movies_by_watched(movies, True)
```

Непросмотренные:

```python
filter_movies_by_watched(movies, False)
```

---

## Сортировка фильмов

Для сортировки будем использовать знакомую функцию:

```python
sorted()
```

### По названию

```python
def sort_movies_by_title(movies):
    return sorted(
        movies,
        key=lambda movie: movie["title"].lower()
    )
```

### По году

```python
def sort_movies_by_year(movies):
    return sorted(
        movies,
        key=lambda movie: movie["year"]
    )
```

### По рейтингу

По техническому заданию фильмы с высоким рейтингом должны находиться первыми:

```python
def sort_movies_by_rating(movies):
    return sorted(
        movies,
        key=lambda movie: movie["rating"],
        reverse=True
    )
```

Важно:

```python
sorted()
```

возвращает **новый список** и не изменяет исходный `movies`. Поэтому сортировка в нашем приложении будет использоваться для отображения данных и не будет менять порядок фильмов в JSON-файле.

---

## Итоговый services/movies.py

На этом этапе файл выглядит так:

```python
def generate_movie_id(movies):
    if not movies:
        return 1

    return max(movie["id"] for movie in movies) + 1


def get_movie_by_id(movies, movie_id):
    for movie in movies:
        if movie["id"] == movie_id:
            return movie

    return None


def add_movie(movies, title, year, genre, rating, watched):
    movie = {
        "id": generate_movie_id(movies),
        "title": title,
        "year": year,
        "genre": genre,
        "rating": rating,
        "watched": watched
    }

    movies.append(movie)

    return movie


def find_movies_by_title(movies, query):
    query = query.lower()
    result = []

    for movie in movies:
        if query in movie["title"].lower():
            result.append(movie)

    return result


def update_movie(movies, movie_id, field, value):
    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        return False

    allowed_fields = {
        "title",
        "year",
        "genre",
        "rating",
        "watched"
    }

    if field not in allowed_fields:
        return False

    movie[field] = value

    return True


def delete_movie(movies, movie_id):
    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        return False

    movies.remove(movie)

    return True


def filter_movies_by_genre(movies, genre):
    result = []

    for movie in movies:
        if movie["genre"].lower() == genre.lower():
            result.append(movie)

    return result


def filter_movies_by_watched(movies, watched):
    result = []

    for movie in movies:
        if movie["watched"] == watched:
            result.append(movie)

    return result


def sort_movies_by_title(movies):
    return sorted(
        movies,
        key=lambda movie: movie["title"].lower()
    )


def sort_movies_by_year(movies):
    return sorted(
        movies,
        key=lambda movie: movie["year"]
    )


def sort_movies_by_rating(movies):
    return sorted(
        movies,
        key=lambda movie: movie["rating"],
        reverse=True
    )
```

---

## Сохраняем изменения в Git

```bash
git status
git add .
git commit -m "Add movie services"
```

Теперь основная логика работы с фильмами готова. Следующий отдельный блок приложения - статистика.

---

# Статистика: services/statistics.py

Статистика не изменяет фильмы.

Она только получает существующую коллекцию и вычисляет необходимые значения.

Поэтому вынесем её отдельно:

```text
services/
└── statistics.py
```

---

### Количество фильмов

```python
def get_movies_count(movies):
    return len(movies)
```

---

### Количество просмотренных фильмов

```python
def get_watched_count(movies):
    count = 0

    for movie in movies:
        if movie["watched"]:
            count += 1

    return count
```

---

### Количество непросмотренных фильмов

Можно снова пройти по всему списку.

Но мы уже знаем:

```text
все фильмы
-
просмотренные
=
непросмотренные
```

Поэтому:

```python
def get_unwatched_count(movies):
    return len(movies) - get_watched_count(movies)
```

---

### Средний рейтинг

Если список пустой, делить на количество фильмов нельзя:

```text
0 / 0
```

Поэтому сначала проверим коллекцию:

```python
def get_average_rating(movies):
    if not movies:
        return 0

    total_rating = 0

    for movie in movies:
        total_rating += movie["rating"]

    return total_rating / len(movies)
```

---

### Фильм с самым высоким рейтингом

```python
def get_best_movie(movies):
    if not movies:
        return None

    return max(
        movies,
        key=lambda movie: movie["rating"]
    )
```

Здесь:

```python
max()
```

сравнивает фильмы по:

```python
movie["rating"]
```

---

## Итоговый services/statistics.py

```python
def get_movies_count(movies):
    return len(movies)


def get_watched_count(movies):
    count = 0

    for movie in movies:
        if movie["watched"]:
            count += 1

    return count


def get_unwatched_count(movies):
    return len(movies) - get_watched_count(movies)


def get_average_rating(movies):
    if not movies:
        return 0

    total_rating = 0

    for movie in movies:
        total_rating += movie["rating"]

    return total_rating / len(movies)


def get_best_movie(movies):
    if not movies:
        return None

    return max(
        movies,
        key=lambda movie: movie["rating"]
    )
```

Сохраняем этап:

```bash
git add .
git commit -m "Add movie statistics"
```

---

# Переходим к main.py

Теперь у нас есть почти вся внутренняя логика приложения.

```text
data/movies.json
        ↕
utils/storage.py
        ↕
services/movies.py
services/statistics.py
        ↕
main.py
```

`main.py` будет связывать все части программы вместе.

Именно здесь будет находиться:

- главное меню;
- `input()`;
- вывод информации;
- выбор нужной операции;
- вызов функций из других модулей.

---

## Импортируем необходимые функции

В начале `main.py`:

```python
from utils.storage import load_movies, save_movies

from services.movies import (
    add_movie,
    get_movie_by_id,
    find_movies_by_title,
    update_movie,
    delete_movie,
    filter_movies_by_genre,
    filter_movies_by_watched,
    sort_movies_by_title,
    sort_movies_by_year,
    sort_movies_by_rating,
)

from services.statistics import (
    get_movies_count,
    get_watched_count,
    get_unwatched_count,
    get_average_rating,
    get_best_movie,
)
```

Так `main.py` не знает, **как реализован поиск или как работает JSON**. Он просто использует готовые функции.

---

# Вывод фильма

Нам много раз понадобится красиво показать один фильм. Чтобы не повторять одинаковый `print()`, создадим функцию:

```python
def print_movie(movie):
    watched = "Да" if movie["watched"] else "Нет"

    print(f'ID: {movie["id"]}')
    print(f'Название: {movie["title"]}')
    print(f'Год: {movie["year"]}')
    print(f'Жанр: {movie["genre"]}')
    print(f'Рейтинг: {movie["rating"]}')
    print(f'Просмотрен: {watched}')
```

Для списка фильмов:

```python
def show_movies(movies):
    if not movies:
        print("Фильмы не найдены.")
        return

    for movie in movies:
        print_movie(movie)
        print("-" * 30)
```

Теперь вместо нескольких циклов можно писать:

```python
show_movies(movies)
```

---

# Проверяем пользовательский ввод

По техническому заданию программа не должна падать при неправильном вводе.

Например:

```python
year = int(input("Год: "))
```

Если пользователь введёт:

```text
hello
```

получим:

```text
ValueError
```

Поэтому создадим несколько небольших функций для ввода данных.

---

## Непустая строка

```python
def read_text(message):
    while True:
        value = input(message).strip()

        if value:
            return value

        print("Значение не должно быть пустым.")
```

---

## Целое число

```python
def read_int(message):
    while True:
        try:
            return int(input(message))
        except ValueError:
            print("Введите целое число.")
```

---

## Рейтинг

Рейтинг должен быть числом от `0` до `10`:

```python
def read_rating(message):
    while True:
        try:
            rating = float(input(message))

            if 0 <= rating <= 10:
                return rating

            print("Рейтинг должен быть от 0 до 10.")

        except ValueError:
            print("Введите число.")
```

---

## Статус просмотра

```python
def read_watched(message):
    while True:
        value = input(message).strip().lower()

        if value in ("да", "д", "yes", "y"):
            return True

        if value in ("нет", "н", "no", "n"):
            return False

        print("Введите 'да' или 'нет'.")
```

Теперь проверка пользовательского ввода находится в одном месте и может использоваться разными пунктами меню.

---

# Добавление фильма

Создадим функцию интерфейса:

```python
def handle_add_movie(movies):
    print("\n=== ДОБАВЛЕНИЕ ФИЛЬМА ===")

    title = read_text("Название: ")
    year = read_int("Год: ")
    genre = read_text("Жанр: ")
    rating = read_rating("Рейтинг: ")
    watched = read_watched("Просмотрен? (да/нет): ")

    movie = add_movie(
        movies,
        title,
        year,
        genre,
        rating,
        watched
    )

    save_movies(movies)

    print(f'Фильм "{movie["title"]}" добавлен.')
```

Обратите внимание на последовательность:

```text
main.py
получает данные
        ↓
add_movie()
изменяет список
        ↓
save_movies()
сохраняет изменения
```

Функция `add_movie()` ничего не знает о JSON.

Функция `save_movies()` ничего не знает о фильмах.

Каждая часть занимается своей задачей.

---

# Поиск фильма

Создадим отдельное меню:

```python
def handle_search(movies):
    print("\n=== ПОИСК ===")
    print("1. По ID")
    print("2. По названию")

    choice = input("Выберите вариант: ")

    if choice == "1":
        movie_id = read_int("Введите ID: ")
        movie = get_movie_by_id(movies, movie_id)

        if movie is None:
            print("Фильм с таким ID не найден.")
            return

        print_movie(movie)

    elif choice == "2":
        query = read_text("Введите название: ")
        result = find_movies_by_title(movies, query)

        show_movies(result)

    else:
        print("Неизвестный пункт меню.")
```

---

# Изменение фильма

Сначала выбираем фильм:

```python
def handle_update(movies):
    movie_id = read_int("Введите ID фильма: ")

    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        print("Фильм с таким ID не найден.")
        return
```

Покажем текущие данные:

```python
print_movie(movie)
```

Теперь дадим пользователю выбрать поле:

```text
1. Название
2. Год
3. Жанр
4. Рейтинг
5. Статус просмотра
0. Отмена
```

Полная функция:

```python
def handle_update(movies):
    print("\n=== ИЗМЕНЕНИЕ ФИЛЬМА ===")

    movie_id = read_int("Введите ID фильма: ")
    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        print("Фильм с таким ID не найден.")
        return

    print_movie(movie)

    print("\nЧто изменить?")
    print("1. Название")
    print("2. Год")
    print("3. Жанр")
    print("4. Рейтинг")
    print("5. Статус просмотра")
    print("0. Отмена")

    choice = input("Выберите пункт: ")

    if choice == "1":
        value = read_text("Новое название: ")
        field = "title"

    elif choice == "2":
        value = read_int("Новый год: ")
        field = "year"

    elif choice == "3":
        value = read_text("Новый жанр: ")
        field = "genre"

    elif choice == "4":
        value = read_rating("Новый рейтинг: ")
        field = "rating"

    elif choice == "5":
        value = read_watched("Просмотрен? (да/нет): ")
        field = "watched"

    elif choice == "0":
        return

    else:
        print("Неизвестный пункт меню.")
        return

    update_movie(
        movies,
        movie_id,
        field,
        value
    )

    save_movies(movies)

    print("Фильм изменён.")
```

---

# Удаление фильма

```python
def handle_delete(movies):
    print("\n=== УДАЛЕНИЕ ФИЛЬМА ===")

    movie_id = read_int("Введите ID фильма: ")

    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        print("Фильм с таким ID не найден.")
        return

    print_movie(movie)

    confirmation = input(
        "Удалить этот фильм? (да/нет): "
    ).strip().lower()

    if confirmation not in ("да", "д", "yes", "y"):
        print("Удаление отменено.")
        return

    delete_movie(movies, movie_id)
    save_movies(movies)

    print("Фильм удалён.")
```

---

# Фильтрация

```python
def handle_filter(movies):
    print("\n=== ФИЛЬТРАЦИЯ ===")
    print("1. По жанру")
    print("2. Просмотренные")
    print("3. Непросмотренные")

    choice = input("Выберите вариант: ")

    if choice == "1":
        genre = read_text("Введите жанр: ")
        result = filter_movies_by_genre(
            movies,
            genre
        )

    elif choice == "2":
        result = filter_movies_by_watched(
            movies,
            True
        )

    elif choice == "3":
        result = filter_movies_by_watched(
            movies,
            False
        )

    else:
        print("Неизвестный пункт меню.")
        return

    show_movies(result)
```

---

# Сортировка

```python
def handle_sort(movies):
    print("\n=== СОРТИРОВКА ===")
    print("1. По названию")
    print("2. По году")
    print("3. По рейтингу")

    choice = input("Выберите вариант: ")

    if choice == "1":
        result = sort_movies_by_title(movies)

    elif choice == "2":
        result = sort_movies_by_year(movies)

    elif choice == "3":
        result = sort_movies_by_rating(movies)

    else:
        print("Неизвестный пункт меню.")
        return

    show_movies(result)
```

Обратите внимание:

```python
result = sort_movies_by_rating(movies)
```

Мы выводим новый отсортированный список, но исходный `movies` не изменяем.

---

# Вывод статистики

```python
def handle_statistics(movies):
    print("\n=== СТАТИСТИКА ===")

    print(
        f"Всего фильмов: "
        f"{get_movies_count(movies)}"
    )

    print(
        f"Просмотрено: "
        f"{get_watched_count(movies)}"
    )

    print(
        f"Не просмотрено: "
        f"{get_unwatched_count(movies)}"
    )

    average_rating = get_average_rating(movies)

    print(
        f"Средний рейтинг: "
        f"{average_rating:.1f}"
    )

    best_movie = get_best_movie(movies)

    if best_movie is not None:
        print("\nЛучший фильм:")
        print(
            f'{best_movie["title"]} - '
            f'{best_movie["rating"]}'
        )
```

Формат:

```python
{average_rating:.1f}
```

оставляет одну цифру после точки.

Например:

```text
7.833333333
```

превратится в:

```text
7.8
```

---

# Главное меню

Теперь можно собрать все части приложения.

Создадим функцию:

```python
def show_menu():
    print("\n=== MOVIE MANAGER ===")
    print("1. Показать все фильмы")
    print("2. Добавить фильм")
    print("3. Найти фильм")
    print("4. Изменить фильм")
    print("5. Удалить фильм")
    print("6. Фильтрация")
    print("7. Сортировка")
    print("8. Статистика")
    print("0. Выход")
```

---

# Запуск приложения

При запуске программы загружаем фильмы только один раз:

```python
movies = load_movies()
```

После этого приложение работает с этим списком.

Изменения сохраняются через:

```python
save_movies(movies)
```

Создадим основную функцию:

```python
def main():
    movies = load_movies()

    while True:
        show_menu()

        choice = input("Выберите пункт: ")

        if choice == "1":
            show_movies(movies)

        elif choice == "2":
            handle_add_movie(movies)

        elif choice == "3":
            handle_search(movies)

        elif choice == "4":
            handle_update(movies)

        elif choice == "5":
            handle_delete(movies)

        elif choice == "6":
            handle_filter(movies)

        elif choice == "7":
            handle_sort(movies)

        elif choice == "8":
            handle_statistics(movies)

        elif choice == "0":
            print("До свидания!")
            break

        else:
            print("Неизвестный пункт меню.")
```

Запускаем приложение:

```python
if __name__ == "__main__":
    main()
```

Этот блок мы уже встречали при изучении модулей.

Он означает:

> вызвать `main()` только тогда, когда `main.py` запускается напрямую.

---

# Итоговый main.py

```python
from utils.storage import load_movies, save_movies

from services.movies import (
    add_movie,
    get_movie_by_id,
    find_movies_by_title,
    update_movie,
    delete_movie,
    filter_movies_by_genre,
    filter_movies_by_watched,
    sort_movies_by_title,
    sort_movies_by_year,
    sort_movies_by_rating,
)

from services.statistics import (
    get_movies_count,
    get_watched_count,
    get_unwatched_count,
    get_average_rating,
    get_best_movie,
)


def print_movie(movie):
    watched = "Да" if movie["watched"] else "Нет"

    print(f'ID: {movie["id"]}')
    print(f'Название: {movie["title"]}')
    print(f'Год: {movie["year"]}')
    print(f'Жанр: {movie["genre"]}')
    print(f'Рейтинг: {movie["rating"]}')
    print(f'Просмотрен: {watched}')


def show_movies(movies):
    if not movies:
        print("Фильмы не найдены.")
        return

    for movie in movies:
        print_movie(movie)
        print("-" * 30)


def read_text(message):
    while True:
        value = input(message).strip()

        if value:
            return value

        print("Значение не должно быть пустым.")


def read_int(message):
    while True:
        try:
            return int(input(message))
        except ValueError:
            print("Введите целое число.")


def read_rating(message):
    while True:
        try:
            rating = float(input(message))

            if 0 <= rating <= 10:
                return rating

            print("Рейтинг должен быть от 0 до 10.")

        except ValueError:
            print("Введите число.")


def read_watched(message):
    while True:
        value = input(message).strip().lower()

        if value in ("да", "д", "yes", "y"):
            return True

        if value in ("нет", "н", "no", "n"):
            return False

        print("Введите 'да' или 'нет'.")


def handle_add_movie(movies):
    print("\n=== ДОБАВЛЕНИЕ ФИЛЬМА ===")

    title = read_text("Название: ")
    year = read_int("Год: ")
    genre = read_text("Жанр: ")
    rating = read_rating("Рейтинг: ")
    watched = read_watched("Просмотрен? (да/нет): ")

    movie = add_movie(
        movies,
        title,
        year,
        genre,
        rating,
        watched
    )

    save_movies(movies)

    print(f'Фильм "{movie["title"]}" добавлен.')


def handle_search(movies):
    print("\n=== ПОИСК ===")
    print("1. По ID")
    print("2. По названию")

    choice = input("Выберите вариант: ")

    if choice == "1":
        movie_id = read_int("Введите ID: ")
        movie = get_movie_by_id(movies, movie_id)

        if movie is None:
            print("Фильм с таким ID не найден.")
            return

        print_movie(movie)

    elif choice == "2":
        query = read_text("Введите название: ")
        result = find_movies_by_title(movies, query)

        show_movies(result)

    else:
        print("Неизвестный пункт меню.")


def handle_update(movies):
    print("\n=== ИЗМЕНЕНИЕ ФИЛЬМА ===")

    movie_id = read_int("Введите ID фильма: ")
    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        print("Фильм с таким ID не найден.")
        return

    print_movie(movie)

    print("\nЧто изменить?")
    print("1. Название")
    print("2. Год")
    print("3. Жанр")
    print("4. Рейтинг")
    print("5. Статус просмотра")
    print("0. Отмена")

    choice = input("Выберите пункт: ")

    if choice == "1":
        value = read_text("Новое название: ")
        field = "title"

    elif choice == "2":
        value = read_int("Новый год: ")
        field = "year"

    elif choice == "3":
        value = read_text("Новый жанр: ")
        field = "genre"

    elif choice == "4":
        value = read_rating("Новый рейтинг: ")
        field = "rating"

    elif choice == "5":
        value = read_watched(
            "Просмотрен? (да/нет): "
        )
        field = "watched"

    elif choice == "0":
        return

    else:
        print("Неизвестный пункт меню.")
        return

    update_movie(
        movies,
        movie_id,
        field,
        value
    )

    save_movies(movies)

    print("Фильм изменён.")


def handle_delete(movies):
    print("\n=== УДАЛЕНИЕ ФИЛЬМА ===")

    movie_id = read_int("Введите ID фильма: ")

    movie = get_movie_by_id(movies, movie_id)

    if movie is None:
        print("Фильм с таким ID не найден.")
        return

    print_movie(movie)

    confirmation = input(
        "Удалить этот фильм? (да/нет): "
    ).strip().lower()

    if confirmation not in ("да", "д", "yes", "y"):
        print("Удаление отменено.")
        return

    delete_movie(movies, movie_id)
    save_movies(movies)

    print("Фильм удалён.")


def handle_filter(movies):
    print("\n=== ФИЛЬТРАЦИЯ ===")
    print("1. По жанру")
    print("2. Просмотренные")
    print("3. Непросмотренные")

    choice = input("Выберите вариант: ")

    if choice == "1":
        genre = read_text("Введите жанр: ")
        result = filter_movies_by_genre(
            movies,
            genre
        )

    elif choice == "2":
        result = filter_movies_by_watched(
            movies,
            True
        )

    elif choice == "3":
        result = filter_movies_by_watched(
            movies,
            False
        )

    else:
        print("Неизвестный пункт меню.")
        return

    show_movies(result)


def handle_sort(movies):
    print("\n=== СОРТИРОВКА ===")
    print("1. По названию")
    print("2. По году")
    print("3. По рейтингу")

    choice = input("Выберите вариант: ")

    if choice == "1":
        result = sort_movies_by_title(movies)

    elif choice == "2":
        result = sort_movies_by_year(movies)

    elif choice == "3":
        result = sort_movies_by_rating(movies)

    else:
        print("Неизвестный пункт меню.")
        return

    show_movies(result)


def handle_statistics(movies):
    print("\n=== СТАТИСТИКА ===")

    print(
        f"Всего фильмов: "
        f"{get_movies_count(movies)}"
    )

    print(
        f"Просмотрено: "
        f"{get_watched_count(movies)}"
    )

    print(
        f"Не просмотрено: "
        f"{get_unwatched_count(movies)}"
    )

    average_rating = get_average_rating(movies)

    print(
        f"Средний рейтинг: "
        f"{average_rating:.1f}"
    )

    best_movie = get_best_movie(movies)

    if best_movie is not None:
        print("\nЛучший фильм:")
        print(
            f'{best_movie["title"]} - '
            f'{best_movie["rating"]}'
        )


def show_menu():
    print("\n=== MOVIE MANAGER ===")
    print("1. Показать все фильмы")
    print("2. Добавить фильм")
    print("3. Найти фильм")
    print("4. Изменить фильм")
    print("5. Удалить фильм")
    print("6. Фильтрация")
    print("7. Сортировка")
    print("8. Статистика")
    print("0. Выход")


def main():
    movies = load_movies()

    while True:
        show_menu()

        choice = input("Выберите пункт: ")

        if choice == "1":
            show_movies(movies)

        elif choice == "2":
            handle_add_movie(movies)

        elif choice == "3":
            handle_search(movies)

        elif choice == "4":
            handle_update(movies)

        elif choice == "5":
            handle_delete(movies)

        elif choice == "6":
            handle_filter(movies)

        elif choice == "7":
            handle_sort(movies)

        elif choice == "8":
            handle_statistics(movies)

        elif choice == "0":
            print("До свидания!")
            break

        else:
            print("Неизвестный пункт меню.")


if __name__ == "__main__":
    main()
```

---

# Проверяем приложение

Теперь нужно пройти весь сценарий работы.

### Добавление

Добавьте несколько фильмов:

```text
Interstellar
2014
Sci-Fi
8.7
да
```

```text
Dune
2021
Sci-Fi
8.0
нет
```

```text
The Dark Knight
2008
Action
9.0
да
```

---

### Поиск

Проверьте:

```text
Interstellar
```

затем:

```text
interstellar
```

и:

```text
star
```

Поиск должен работать во всех трёх случаях.

---

### Изменение

Измените рейтинг одного из фильмов и убедитесь, что:

```text
data/movies.json
```

также изменился.

---

### Удаление

Удалите фильм.

Перезапустите приложение и убедитесь, что он не появился снова.

---

### Фильтрация

Проверьте:

```text
Sci-Fi
```

а затем просмотренные и непросмотренные фильмы.

---

### Сортировка

Сравните сортировку:

```text
по названию
по году
по рейтингу
```

---

### Статистика

Проверьте:

```text
общее количество
просмотренные
непросмотренные
средний рейтинг
лучший фильм
```

---

# Итоговая структура проекта

После завершения приложения получаем:

```text
movie_manager/
├── main.py
│
├── data/
│   └── movies.json
│
├── services/
│   ├── movies.py
│   └── statistics.py
│
├── utils/
│   └── storage.py
│
├── README.md
└── .gitignore
```

Но теперь эта структура - не просто набор папок.

У каждой части есть понятная ответственность:

```text
main.py
→ взаимодействие с пользователем

services/movies.py
→ логика работы с фильмами

services/statistics.py
→ вычисления и статистика

utils/storage.py
→ работа с JSON

data/movies.json
→ постоянное хранение данных
```

---

# README.md

Даже небольшой учебный проект должен иметь описание.

Создадим `README.md`:

````md
# Movie Manager

Консольное приложение на Python для управления личной коллекцией фильмов.

## Возможности

- просмотр коллекции;
- добавление фильмов;
- поиск;
- изменение;
- удаление;
- фильтрация;
- сортировка;
- статистика;
- сохранение данных в JSON.

## Запуск

```bash
python main.py
```

## Хранение данных

Данные сохраняются в:

```text
data/movies.json
```
````

---

# Финальный Git-коммит

Проверяем проект:

```bash
git status
```

Добавляем изменения:

```bash
git add .
```

Создаём коммит:

```bash
git commit -m "Complete Movie Manager"
```

Посмотрим историю:

```bash
git log --oneline
```

Например:

```text
Complete Movie Manager
Add movie statistics
Add movie services
Add JSON storage
Create project structure
```

Обратите внимание: приложение создавалось не одним огромным коммитом. Каждый законченный этап разработки имеет собственный коммит.

---

# Что мы сделали?

В начале у нас было только техническое задание.

```text
Техническое задание
        ↓
Анализ требований
        ↓
Структура проекта
        ↓
Работа с файлами
        ↓
Логика приложения
        ↓
Интерфейс
        ↓
Проверка данных
        ↓
Готовое приложение
```

Мы не просто написали несколько функций.

Мы собрали знакомые инструменты Python в одно приложение:

- списки;
- словари;
- условия;
- циклы;
- функции;
- модули;
- JSON;
- исключения;
- поиск;
- фильтрацию;
- сортировку;
- Git.

---

# Задачи на собеседование

В оставшееся время разберём несколько небольших задач. Здесь важно не только получить правильный результат, но и уметь объяснить ход решения.

---

## Задача 1. Частота элементов

Дан список:

```python
languages = [
    "python",
    "js",
    "python",
    "java",
    "js",
    "python"
]
```

Получите:

```python
{
    "python": 3,
    "js": 2,
    "java": 1
}
```

Попробуйте решить задачу самостоятельно.

<details>
<summary>Решение</summary>

```python
result = {}

for language in languages:
    if language not in result:
        result[language] = 0

    result[language] += 1

print(result)
```

</details>

---

## Задача 2. Удаление дубликатов

Дан список:

```python
numbers = [4, 2, 4, 7, 2, 8, 4]
```

Получите:

```python
[4, 2, 7, 8]
```

Порядок элементов должен сохраниться.

<details>
<summary>Решение</summary>

```python
result = []

for number in numbers:
    if number not in result:
        result.append(number)

print(result)
```

</details>

---

## Задача 3. Второй максимальный элемент

Дан список:

```python
numbers = [4, 10, 7, 10, 8]
```

Найдите второй **уникальный** максимальный элемент.

Результат:

```text
8
```

<details>
<summary>Решение</summary>

```python
unique_numbers = list(set(numbers))
unique_numbers.sort(reverse=True)

if len(unique_numbers) >= 2:
    print(unique_numbers[1])
else:
    print("Второго максимального элемента нет.")
```

</details>

---

## Задача 4. Первый уникальный символ

Дана строка:

```python
text = "aabbcddee"
```

Найдите первый символ, который встречается только один раз.

Результат:

```text
c
```

<details>
<summary>Решение</summary>

```python
symbols_count = {}

for symbol in text:
    symbols_count[symbol] = (
        symbols_count.get(symbol, 0) + 1
    )

for symbol in text:
    if symbols_count[symbol] == 1:
        print(symbol)
        break
```

</details>

---

# Итоги лекции

Сегодня мы собрали уже знакомые инструменты в полноценное приложение.

| Навык                  | Где использовали                                |
| --------------------------- | -------------------------------------------------------------- |
| Словари              | Представление фильма                        |
| Списки                | Коллекция фильмов                              |
| Функции              | Разделение логики                              |
| Модули                | Разделение приложения                      |
| JSON                        | Хранение данных                                  |
| Исключения        | Проверка пользовательского ввода |
| Линейный поиск | Поиск фильма                                        |
| `sorted()`                | Сортировка                                           |
| `lambda`                  | Ключ сортировки                                  |
| `max()`                   | ID и лучший фильм                                  |
| Git                         | История разработки                            |

Главная идея этой лекции:

```text
Хорошая программа
≠
один большой файл с работающим кодом
```

Приложение становится проще развивать, когда разные части имеют понятную ответственность:

```text
данные
логика
хранилище
интерфейс
```
