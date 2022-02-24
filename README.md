# Как загрузить свой модуль на PyPI?
Мы будем пользоваться модулем *poetry* для осуществления нашей цели.

Для начала вам нужно написать модуль, который вы будете публиковать. Также я вам советую для начала опубликовать модуль на **[Test-PyPI](https://test.pypi.org)**, потому что у вас могут возникнуть проблемы в коде, баги, которые вы не заметили. Тем самым вы сможете оберечь себя от *лишних релизов* на PyPI.

Зарегистрируйтесь на **[Test-PyPI](https://test.pypi.org)** и **[PyPI](https://pypi.org)**, регистрация простая.

# Poetry - Python
1. **🥑 Установка poetry**
2. **🍍 Иницилизация, подготовка к публикации**
3. **🍑 Публикация модуля на [Test-PyPI](https://test.pypi.org)**
4. **🍇 Публикация модуля на [PyPI](https://pypi.org)**

## 🥑 Установка poetry
**[Официальный сайт Poetry](https://python-poetry.org/)**, здесь вы сможете более подробней разобраться с *poetry*

Способы установки *poetry*
- **🥀 cURL**
```
$ curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```
- **🍃 PowerShell**
```
$ (Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```
- **🍋 pip**
```
$ pip install --user poetry
```

## 🍍 Иницилизация, подготовка к публикации
Открываем **CMD**, переходим в директорию где находится *папка/файл* и вводим команду: **`poetry init`**
```py
[Package name]: название вашего файла или папки, в которой хранятся файлы с кодом. По умолчанию - текущая директорию
[Version] (0.1.0): желаемае версия проекта, по умолчанию - 0.1.0
[Description]: Небольшое описание проекта
[Author] (You <youremail@example.com>): автор(ы) проекта
[License]: текстовый документ с описанием лицензии, по умолчанию - ничего
[Compatible Python version] (3.x): Желаемая версия Python, по умолчанию - текущая
[... main dependencies]: Хотите ли определить основные зависимости проекта? Зависимости, это доп. модули, которые будут устанавливаться с вашим модулем
```

На счет *зависимостей*, если вы импортируете различные модули в своем проекте, советую указать их в *зависимостях* (кроме дефолтных), тем самым, если у пользователя устанавливающего ваш модуль не будет к примеру модуля `requests`, то он автоматический установится вместе с вашим модулем, при условии того, если вы указали его в *зависимостях*.

В директории создался файл `pyproject.toml`, вы можете открыть его в редакторе и добавить пару строк. Например, можно указать *большое* описание для проекта, для этого нужно написать строчку `readme = "файл с описанием (к примеру README.md)"`, также вы можете указать ключевые слова `keywords = ["список из максимум 5 ключевых слов"]`, можно указать свой репозиторий на GitHub `repository = "ссылка на ваш репозиторий"`, или страницу проекта `homepage = "ссылка на страницу"`. Подробней в [оф. документации Poetry](https://python-poetry.org/docs/)

## 🍑 Публикация модуля на [Test-PyPI](https://test.pypi.org)
Вот и пришло время опубликовать модуль в открытый доступ! \
Чтобы опубликовать модуль на *Test-PyPI* нужно прописать следующуе:

```py
poetry config repositories.testpypi https://test.pypi.org/legacy/
```

```py
poetry publish -r testpypi --username YOUR_USERNAME --password YOUR_PASSWORD --build
```

## 🍇 Публикация модуля на [PyPI](https://pypi.org)
Все проще чем кажется. Но прежде чем опубликовать модуль на основной PyPI, советую опубликовать его на [test.pypi.org](https://test.pypi.org) и проверить на наличие багов, ошибок в коде и т.п.

```py
poetry publish --username YOUR_USERNAME --password YOUR_PASSWORD --build
```

# Twine (дописывается)
1. **❤ Установка twine**
2. **🧡 Создание setup.py & setup.cfg**
3. **💛 Иницилизация проекта**
4. **💜 Публикация на PyPi**

## ❤ Установка twine
[GitHub Twine](https://github.com/tweecode/twine) \
Способы установки *Twine*
- **pip**
```
$ pip install twine
```
- **Windows Installer** \
[Скачать Twine для Windows](https://github.com/tweecode/twine/releases/tag/v1.4.3)
- **OS X Installer** \
[Скачать Twine для OS X](https://twinery.org/)

## 🧡 Создание setup.py & setup.cfg
Для начала создадим рабочую директорию.
```
$ md workplace
$ cd workplace
```

Представим структуру директории.
```
[workplace (наша рабочая директория)]
|
|___[package (наш пакет с модулями)]
    |
    |__...
```

В рабочей директории нужно создать два файла, которые будут отвечать за настройку нашего проекта, для того чтобы в дальнешейм опубликовать его на PyPi. \
*Примеры приведенные ниже использовать без точки (.) в конце*
<details><summary>Создание файла в консоли, на Windows</summary>
   
   1. echo > setup.cfg.
   2. echo > setup.py.
   
</details>
<details><summary>Создание файла в консоли, на Linux</summary>
   
   1. touch setup.cfg.
   2. touch setup.py.
   
</details>

### setup.cfg
```cfg
[egg_info]
tag_build = 
tag_date = 0
```

### setup.py
```py
from setuptools import setup # pip install setuptools
from io import open


def read(filename):
   """Прочитаем наш README.md для того, чтобы установить большое описание."""
   with open(filename, "r", encoding="utf-8") as file:
      return file.read()


setup(name="Название твоего пакета",
   version="0.1", # Версия твоего проекта. ВАЖНО: менять при каждом релизе
   description="Короткое описание твоего проекта на PyPi",
   long_description=read("README.md"), # Здесь можно прописать README файл с длинным описанием
   long_description_content_type="text/markdown", # Тип контента, в нашем случае text/markdown
   author="Имя автора",
   author_email="почта_автора@gmail.com",
   url="https://github.com/Fsoky/Upload-library-to-PyPI", # Страница проекта
   keywords="api some_keyword tools" # Ключевые слова для упрощеннего поиска пакета на PyPi
)
```

## 💛 Иницилизация проекта
Чтобы иницилизировать проект, в консоли нужно прописать:
```
$ python setup.py sdist
```
После чего в нашей рабочей директории создадуться папки: `dist`, `package_name.egg_info`. \
В `.egg_info`, вы можете найти не большую информацию о своем проекте, в `dist` храниться архив, который будет публиковаться на PyPi.

## 💜 Публикация на PyPi
Чтобы опубликовать проект, воспользуемся утилитой *Twine*.

- Публикация на [Test-PyPI](https://test.pypi.org)
```
$ twine upload -r testpypi dist/*
```
- Публикация на [PyPI](https://pypi.org)
```
$ twine upload dist/*
```

После этой команды, вы увидите:
```
username: Ваш ник на PyPi
password: Сюда вводите ваш пароль (его не будет видно)
```
В случае успешной публикации, вы получите ссылку на PyPi с проектом.
