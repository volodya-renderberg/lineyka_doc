.. _class-task-page:

Class Task
==========

**task(studio)**

**level** = 'project'

Данные хранимые в БД (имя столбца : тип данных):

.. code-block:: python

  tasks_keys = {
    'activity': 'text',
    'task_name': 'text',
    'task_type': 'text',
    'season': 'text',
    'input': 'json',
    'status': 'text',
    'outsource': 'integer',
    'artist': 'text', # nik_name
    'planned_time': 'text',
    'time': 'text',
    'start': 'timestamp',
    'end': 'timestamp',
    'supervisor': 'text',
    'approved_date': 'text',
    'price': 'real',
    'tz': 'text',
    'chat_local': 'json',
    'web_chat': 'text',
    'readers': 'json', # словарь: ключ - nik_name, значение - 0 или 1 (статус проверки),  плюс одна запись: ключ - 'first_reader, значение - nik_name - это первый проверяющий - пока он не проверит даннаня задача не будет видна у других проверяющих в списке на проверку.
    'output': 'json',
    'priority':'integer',
    'extension': 'text',
    }

Создание экземпляра класса:
---------------------------

.. code-block:: python
  
  import edit_db as db
  
  project = db.project()
  asset = db.asset(project)
  
  task = db.task(asset) # asset - обязательный параметр при создании экземпляра task
  # доступ ко всем параметрам и методам принимаемого экземпляра asset через task.asset
  
Атрибуты
--------

:activity: (*str*)

:task_name: (*str*)

:task_type: (*str*)

:season: ``?``

:input: (*str* / *list*)

:status: (*str*)

:outsource: (*int*)

:artist: (*str*) - *nik_name*

:planned_time: 

:time:

:start:

:end:

:supervisor: ``?``

:approved_date:

:price:

:tz: ``должно быть specification``

:chat_local: ``?``

:web_chat: ``?``

:readers: 

:output:

:priority:

:extension: