.. _class-log-page:

Class Log
=========

**log(studio)**

**level** = 'project'

Данные хранимые в БД (имя столбца : тип данных):

.. code-block:: python

  logs_keys = {
    'version': 'text',              # hex 4 символа
    'date_time': 'timestamp',       # время и дата записи
    'activity': 'text',             # ативити задачи
    'task_name': 'text',            # имя задачи
    'action': 'text',               # тип записи из log.log_actions
    'artist': 'text',               # nik_name артиста, кто делает запись
    'description': 'text',          # коментарий
    'source': 'json', 				# для push - версия коммита источника (в случае sketch - список версий по всем веткам, порядок совпадает с порядком записи веток в branch), для publish - версия push источника.
    'branch' : 'text',              # ветка - в случае push, publish для sketch - список веток.
    'time' : 'integer',             # время затраченное на commit, ед. измерения секунда.
    }
    
  artists_logs_keys = {
    'project_name': 'text',
    'task_name': 'text',
    'full_time': 'integer',         # суммарное время затраченое артистом на задачу, ед. измерения секунда.
    'price': 'real',                # сумма начисленная за выполнение задачи. вносится по принятию задачи.
    'start': 'timestamp',           # дата-время создания записи, запись создаётся при первом open задачи.
    'finish': 'timestamp',          # дата-время принятия задачи.
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

:log_actions: (*list*) - *['commit', 'push', 'publish', 'open', 'report','recast' , 'change_artist', 'close', 'done', 'return_a_job', 'send_to_outsource', 'load_from_outsource']*

:task: (*task*) - экземпляр :ref:`class-task-page` принимаемый при создании экземпляра класса, содержит все атрибуты и методы :ref:`class-task-page`.
    
Методы
------

.. py:function:: write_log(logs_keys[, artist_ob=False])

  запись лога активити задачи.
  
  .. note:: self.task - должен быть инициализирован

  .. rubric:: Параметры:

  * **logs_keys** (*dict*) - словарь по *studio.logs_keys* - обязательные ключи: *comment, version, action*
  * **artist_ob** (*bool/artist*) - если *False* - значит создаётся новый объект *artist()* и определяется текущий пользователь
  * **return** - (*True, 'Ok!'*) или (*False, comment*)

.. py:function:: read_log([action=False])

  чтение лога активити задачи.
  
  .. note:: self.task - должен быть инициализирован

  .. rubric:: Параметры:

  * **action** (*bool / str / list*) если *False* - то возврат для всех *action*, если *list* - то будет использован оператор ``WHERE OR`` тоесть возврат по всем перечисленным экшенам.
  * **return** - (*True, ([список словарей логов, сотрирован по порядку], [список наименований веток])*) или (*False, comment*)

.. py:function:: get_push_logs([task_data=False, time_to_str = False])

  возврат списка push логов для задачи.
  
  .. note:: Возможно устаревшая

  .. rubric:: Параметры:

  * **task_data** (*bool/dict*) - если *False* - значит читается *self.task* ``лучше не использовать``
  * **time_to_str** (*bool*) - если *True* - то преобразует дату в строку
  * **return** - (*True, ([список словарей логов, сотрирован по порядку], [список наименований веток])*) или (*False, comment*)

.. py:function:: camera_write_log(artist_ob, comment, version[, task_data=False])

  запись лога для сохраняемой камеры шота.

  .. rubric:: Параметры:

  * **artist_ob** - (*artist*) - объект *artist*, его никнейм записывается в лог
  * **comment** (*str*) - комментарий
  * **version** (*str/int*) - номер версии *<= 9999* ``возможно должно быть автоопределение ``
  * **task_data** (*bool/dict*) - если *False* - значит читается *self.task* ``лучше не использовать``
  * **return** - (*True, 'Ok!'*) или (*False, comment*)

.. py:function:: camera_read_log([task_data=False])

  чтение логов камеры шота.

  .. rubric:: Параметры:

  * **task_data** (*bool/dict*) - если *False* - значит читается *self.task* ``лучше не использовать``
  * **return** - (*True, [{camera_log}, ... ]*) (возвращаемый список сортирован по порядку) или (*False, comment*)

.. py:function:: playblast_write_log(artist_ob, comment, version[, task_data=False])

  запись лога создаваемого плейбласта шота.

  .. rubric:: Параметры:

  * **artist_ob** - (*artist*) - объект *artist*, его никнейм записывается в лог
  * **comment** (*str*) - комментарий
  * **version** (*str/int*) - номер версии *<= 9999* ``возможно должно быть автоопределение ``
  * **task_data** (*bool/dict*) - если *False* - значит читается *self.task* ``лучше не использовать``
  * **return** - (*True, 'Ok!'*) или (*False, comment*)
  
.. py:function:: playblast_read_log ([task_data=False])

  чтение логов плейбластов шота.
  
  .. rubric:: Параметры:
  
  * **task_data** (*bool/dict*) - если *False* - если *False* - значит читается *self.task* ``лучше не использовать``
  * **return** - (*True, [{playblast_log}, ... ]*) (возвращаемый список сортирован по порядку) или (*False, comment*)