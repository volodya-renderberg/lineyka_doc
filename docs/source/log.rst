.. _class-log-page:

Class Log
=========

**log(studio)**

**level** = 'project'

Данные хранимые в БД (имя столбца : тип данных):

.. code-block:: python

  logs_keys = {
    'version': 'text',
    'date_time': 'timestamp',
    'activity': 'text',
    'task_name': 'text',
    'action': 'text',
    'artist': 'text',
    'description': 'text',
    'branch' : 'text',
    }
    
Создание экземпляра класса:
---------------------------

.. code-block:: python
  
  import edit_db as db
  
  project = db.project()
  asset = db.asset(project)
  task = db.task(asset)
  
  log = db.log(task) # task - обязательный параметр при создании экземпляра log
  # доступ ко всем параметрам и методам принимаемого экземпляра task - через log.task
  
Атрибуты
--------

:version: (*str*) - *hex* 4 символа

:date_time: (*timestamp*) - время и дата записи

:activity: (*str*) - ативити задачи

:task_name: (*str*) - имя задачи

:action: (*str*) - тип записи из [*'push', 'publish'*]

:artist: (*str*) - *nik_name* артиста, кто делает запись

:description: (*str*) - коментарий

:branch: (*str*) - ветка ``пока не реализовано использование``

:task: (*task*) - экземпляр :ref:`class-task-page` принимаемый при создании экземпляра класса, содержит все атрибуты и методы :ref:`class-task-page`.
    
Методы
------

.. py:function:: write_log(logs_keys[, artist_ob=False])

  запись лога задачи.
  
  .. note:: self.task - должен быть инициализирован

  .. rubric:: Параметры:

  * **logs_keys** (*dict*) - словарь по *studio.logs_keys* - обязательные ключи: *comment, version, action*
  * **artist_ob** (*bool/artist*) - если False - значит создаётся новый объект *artist()* и определяется текущий пользователь
  * **return** - (*True, 'Ok!'*) или (*False, comment*)

.. py:function:: read_log([action=False])

  чтение лога задачи.
  
  .. note:: self.task - должен быть инициализирован

  .. rubric:: Параметры:

  * **action** (*bool / str*) если False - то возврат для всех *action*
  * **return** - (*True, [{log}, ... ]*) (возвращаемый список сортирован по порядку) или (*False, comment*)

.. py:function:: get_push_logs([task_data=False, time_to_str = False])

  возможно устаревшая - возврат списка push логов для задачи.

  .. rubric:: Параметры:

  * **task_data** (*bool/dict*) - если False - значит читается *self.task* ``лучше не использовать``
  * **time_to_str** (*bool*) - если True - то преобразует дату в строку
  * **return** - (*True, [{push_log}, ... ]*) (возвращаемый список сортирован по порядку) или (*False, comment*)

.. py:function:: camera_write_log(artist_ob, comment, version[, task_data=False])

  запись лога для сохраняемой камеры шота.

  .. rubric:: Параметры:

  * **artist_ob** - (*artist*) - объект *artist*, его никнейм записывается в лог
  * **comment** (*str*) - комментарий
  * **version** (*str/int*) - номер версии *<= 9999*
  * **task_data** (*bool/dict*) - если False - значит читается *self.task* ``лучше не использовать``
  return - (True, 'Ok!') или (False, comment)

.. py:function:: camera_read_log([task_data=False])

  чтение логов камеры шота.

  .. rubric:: Параметры:

  * **task_data** (*bool/dict*) - если False - значит читается *self.task* ``лучше не использовать``
  * **return** - (*True, [{camera_log}, ... ]*) (возвращаемый список сортирован по порядку) или (*False, comment*)

.. py:function:: playblast_write_log(artist_ob, comment, version[, task_data=False])

  запись лога создаваемого плейбласта шота.

  .. rubric:: Параметры:

  * **artist_ob** - (*artist*) - объект *artist*, его никнейм записывается в лог
  * **comment** (*str*) - комментарий
  * **version** (*str/int*) - номер версии *<= 9999*
  * **task_data** (*bool/dict*) - если False - значит читается *self.task* ``лучше не использовать``
  * **playblast_read_log** (*task_data=False*) - чтение логов плейбластов шота
  * **task_data** (*bool/dict*) - если False - значит читается *self.task*, если передаётся, то только задача данного ассета
  * **return** - (*True, [{playblast_log}, ... ]*) (возвращаемый список сортирован по порядку) или (*False, comment*)