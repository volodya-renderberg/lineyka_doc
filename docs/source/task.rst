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
    'input': 'json',
    'status': 'text',
    'outsource': 'integer',
    'artist': 'text', # nik_name
    'planned_time': 'real',
    'time': 'real',
    'start': 'timestamp',
    'end': 'timestamp',
    'price': 'real',
    'specification': 'text',
    'chat_local': 'json',
    'web_chat': 'text',
    'supervisor': 'text',
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

:activity: (*str*) - активити из *asset.ACTIVITY_FOLDER[asset_type]*

:task_name: (*str*) - имя задачи, структура имени: *asset_name:task_name*

:task_type: (*str*) - тип задачи из *studio.task_types* + *service*

:input: (*str* / *list*) - для сервисной задачи (*task_type=service*) - это список имён входящих задачь. для не сервисной задачи - это имя входящей задачи.

:status: (*str*) - статус задачи из *studio.task_status*

:outsource: (*int*) - значение из [0, 1] если = 1 - задача на аутсорсе.

:artist: (*str*) - *nik_name* исполнителя.

:planned_time: (*float*) - планируемое время (ед. измерения - час).

:time: (*float*) - реально затраченное время (ед. измерения - час).

:start: (*timestamp*) - дата и время взятия задачи в работу.

:approved_date: (*timestamp*) - дата планируемого окончания работ (``должна вычисляться при инициализации экземпляра``)

:end: (*timestamp*) - дата и время приёма задачи.

:price: (*float*) - стоимость работ по задаче (ед. измерения - юнит).

:specification: ``должно быть specification`` ссылка на техническое задание.

:chat_local: ``?``

:web_chat: ``?``

:supervisor: ``?``

:readers: (*dict*) - словарь: ключ - nik_name, значение - 0 или 1 (статус проверки),  плюс одна запись: ключ - 'first_reader, значение - nik_name - это первый проверяющий - пока он не проверит даннаня задача не будет видна у других проверяющих в списке на проверку.

:output: (*list*) - список имён исходящих задач.

:priority: (*int*) - приоритет.

:extension: (*str*) - расширение файла для работы над данной задачей, начинается с точки, например: *'.blend'*
