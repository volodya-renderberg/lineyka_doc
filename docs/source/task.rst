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
    'readers': 'json', # словарь: ключ - nik_name, значение - 0 или 1 (статус проверки),  плюс одна запись: ключ - 'first_reader', значение - nik_name - это первый проверяющий - пока он не проверит даннаня задача не будет видна у других проверяющих в списке на проверку.
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

:input: (*str* / *list*) - для сервисной задачи (*task_type=service*) - это список имён входящих задач. для не сервисной задачи - это имя входящей задачи.

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

:readers: (*dict*) - словарь: ключ - *nik_name*, значение - 0 или 1 (статус проверки),  плюс одна запись: ключ - *'first_reader'*, значение - *nik_name* - это первый проверяющий - пока он не проверит даннаня задача не будет видна у других проверяющих в списке на проверку.

:output: (*list*) - список имён исходящих задач.

:priority: (*int*) - приоритет.

:extension: (*str*) - расширение файла для работы над данной задачей, начинается с точки, например: *'.blend'*

:asset: (*asset*) - экземпляр :ref:`class-asset-page` принимаемый при создании экземпляра класса, содержит все атрибуты и методы :ref:`class-asset-page`.

Методы
------

Чтение
~~~~~~

.. py:function:: init(task_name[, new = True])

  заполнение полей экземпляра по *studio.tasks_keys*
  
  .. rubric:: Параметры:

  * **task_name** (*str*) - имя задачи. данные задачи будут считаны из базы данных
  * **new** (*bool*) - если *True* - то возвращается новый инициализированный экземпляр класса *task*, если *False* - то инициализируется текущий экземпляр
  * **return**:
      * если *new=True* - инициализированный экземпляр, 
      * если *new=False* - (*True, 'Ok!'*) / или (*False, comment*)

.. py:function:: init_by_keys(keys[, new=True])

  заполнение полей экземпляра по *studio.tasks_keys*
  
  .. rubric:: Параметры:

  * **keys** (*dict*) - словарь данных задачи, получаемый в функции *__read_task()*
  * **new** (*bool*) - если *True* - то возвращается новый инициализированный экземпляр класса *task*, если *False* - то инициализируется текущий экземпляр
  * **return**:
    *если *new=True* - инициализированный экземпляр, 
    *если *new=False* - (*True, 'Ok!'*)
    
.. py:function:: get_list([asset_id=False, task_status = False, artist = False])

  получение списка задач ассета (экземпляры).

  .. rubric:: Параметры:

  * **asset_id** (*str*) - требуется если экземпляр *task.asset* не инициализирован, либо требуется список задач не для инициализированного ассета
  * **task_status** (*str*) - фильтр по статусам задач
  * **artist** (*str*) - фильтр по имени
  * **return** - (*True, task_list(список задач - экземпляры)*) или (*False, коммент*)

.. py:function:: get_tasks_by_name_list(task_name_list[, assets_data = False])

  возвращает задачи (экземпляры) по списку имён задач, из различных ассетов.
  
  .. note:: *task.asset.project* - инициализирован

  .. rubric:: Параметры:

  * **task_name_list** (*list*) - список имён задач
  * **assets_data** (*dict*) ``?? или экземпляр`` - *dict{asset_name: {asset_data},...}* результат функции *asset.get_name_data_dict_by_all_types()*, если не передавать - будет произведено чтение БД
  * **return** - (*True, { task_name: task_data(dict), ... }*) или (*False, коммент*)
    
Смены статусов
~~~~~~~~~~~~~~

.. py:function:: service_input_to_end(assets)

  изменение статуса текущей сервис задачи (задача инициализирована), по проверке статусов входящих задач. и далее задач по цепочке.
  
  .. rubric:: Параметры:
  
  * **task_data** (*dict*) - текущая задача
  * **assets** (*dict*) - словарь всех ассетов по всем типам (ключи - имена, данные - ассеты ``(словари)??``) - результат функции *asset.get_name_data_dict_by_all_types()*
  * **return** - (*True, new_status*) или (*False, коммент*)

.. py:function:: from_input_status(input_task[, this_task=False])

  возвращает новый статус текущей задачи (если *this_task=False*), на основе входящей задачи.
  
  .. rubric:: Параметры:
  
  * **input_task** (*task / False*) входящая задача
  * **this_task** (*task / False*) - если *False* - то предполагается текущая задача
  * **return** - *new_status*

.. py:function:: this_change_from_end([this_task=False, assets = False])

  замена статусов исходящих задач при изменении статуса текущей задачи с *done* или с *close*.
  
  .. rubric:: Параметры:
  
  * **this_task** (*task / False*) - если *False* то текущая задача
  * **assets** (*dict*) - словарь всех ассетов по всем типам (ключи - имена, данные - ассеты ``(словари)??``) - результат функции *asset.get_name_data_dict_by_all_types()*
  * **return** - (*True, 'Ok!'*) / или (*False, comment*)

.. py:function:: this_change_to_end(self[, assets = False])

  замена статусов исходящих задач при изменении статуса текущей задачи на *done* или *close*.
  
  .. rubric:: Параметры:
  
  * **task** - инициализирован
  * **assets** (*dict*) - словарь всех ассетов по всем типам (ключи - имена, данные - ассеты ``(словари)??``) - результат функции *asset.get_name_data_dict_by_all_types()*
  * **return** - (*True, 'Ok!'*) / или (*False, comment*)
  
.. py:function:: service_add_list_to_input(input_task_list)

  добавление списка задач во входящие сервисной задаче, со всеми вытикающими изменениями статусов.

  .. note:: *task.asset* инициализирован
  
  .. rubric:: Параметры:

  * **input_task_list** (*list*) - список задач (экземпляры)
  * **return** - (*True, new_ststus, append_task_name_list)*)  или (*False, коммент*)

.. py:function:: service_add_list_to_input_from_asset_list(asset_list[, task_data=False])

  добавление задач во входящие сервисной задаче из списка ассетов. Какую именно добавлять задачу из ассета, определяет алгоритм.

  .. rubric:: Параметры:

  * **asset_list** (*list*) - подсоединяемые ассеты (словари, или экземпляры)
  * **task_data** (*dict*) - изменяемая задача, если *False* - значит предполагается, что *task* инициализирован ``лучше не использовать``
  * **return** - (*True, (this_task_data, append_task_name_list)*) ``?? пересмотреть``  или (*False, коммент*)

.. py:function:: service_remove_task_from_input(removed_tasks_list[, task_data=False, change_status = True])

  удаление списка задач из входящих сервисной задачи.

  .. rubric:: Параметры:

  * **task_data** (*dict*) - изменяемая задача, если *False* - значит предполагается, что *task* инициализирован ``лучше не использовать``
  * **removed_tasks_list** (*list*) - содержит словари удаляемых из инпута задач
  * **return** - (*True, (new_status, input_list)*) или (*False, коммент*)
      * **new_status** (*str*)- новый статтус данной задачи
      * **input_list** (*list*) - фактически *task.input*

.. py:function:: service_change_task_in_input(removed_task_data, added_task_data[, task_data=False])

  замена входящей задачи одной на другую для сервисной задачи.

  .. rubric:: Параметры:

  * **removed_task_data** (*dict*) - удаляемая задача ``?? или экземпляр``
  * **added_task_data** (*dict*) - добавляемая задача ``?? или экземпляр``
  * **task_data** (*dict*) - изменяемая задача, если *False* - значит предполагается, что *task* инициализирован. ``лучше не использовать``
  * **return** - (*True, (this_task_data, append_task_name_list)*)  или (*False, коммент*)

Пути
~~~~

.. py:function:: get_final_file_path([task_data=False])

  получение пути к последней версии файла задачи.
  
  .. rubric:: Параметры:

  * **task_data** (*dict*) - требуется если не инициализирован *task* ``лучше не использовать``
  * **return_data** - (*True, final_file_path, asset_path*) или  (*False, comment*)

.. py:function:: get_version_file_path(version[, task_data=False])

  получение пути к файлу задачи по указанной версии.
  
  .. rubric:: Параметры:

  * **task_data** (*dict*) - требуется если не инициализирован *task* ``лучше не использовать``
  * **version** (*str*) - *hex* 4 символа
  * **return** - (*True, version_file_path*) или  (*False, comment*)

.. py:function:: get_new_file_path([task_data=False])

  получение пути к файлу задачи для новой версии
  
  .. rubric:: Параметры:

  * **task_data** (*dict*) - требуется если не инициализирован *task* ``лучше не использовать``
  * **return** - (*True, (new_dir_path, new_file_path)*)

.. py:function:: get_publish_file_path(activity)

  получение пути к паблиш версии файла активити.
  
  .. rubric:: Параметры:

  *	**activity** (*str*) - активити
  * **return** - (*True, publish_file_path*) или  (*False, comment*)

.. py:function:: open_file([look=False, current_artist=False, tasks=False, input_task=False, open_path=False])

  откроет файл в приложении - согласно расширению.
  
  .. rubric:: Параметры:

  * **look** (*bool*) - если *True* - то статусы меняться не будут, если *False* - то статусы меняться будут
  * **current_artist** (*artist*) - если не передавать, то в случае *look=False* - будет выполняться *get_user()* - лишнее обращение к БД
  * **tasks** (*dict*) - словарь задач данного артиста по именам. - нужен для случая когда *look=False*, при отсутствии будет считан - лишнее обращение к БД
  * **input_task** (*task*) - входящая задача - для *open_from_input*
  * **open_path** (*unicode/str*) - путь к файлу - указывается для *open_from_file*
  * **return** (*True, file_path - куда открывается файл*) или (*False, coment*)

.. py:function:: push_file(description, current_file[, current_artist=False])

  локальная запись новой рабочей версии файла, сохранение версии + запись *push* лога.
  
  .. rubric:: Параметры:

  * **description** (*str*) - комментарий к версии
  * **current_file** (*unicode/str*) - текущее местоположение рабочего файла (как правило в темп)
  * **current_artist** (*artist*) - если не передавать, то будет выполняться *get_user()* - лишнее обращение к БД
  * **return** (*True, new_file_path*) или (*False, comment*)
  
Кеш
~~~

.. py:function:: get_versions_list_of_cache_by_object(ob_name[, activity = 'cache', extension = '.pc2', task_data=False])

  список версий кеша для экземпляра.

  .. rubric:: Параметры:

  * **ob_name** (*str*) - имя 3d экземпляра
  * **activity** (*str*) - по умолчанию *"cache"* (для *blender*) - для других программ может быть другим, например *"maya_cache"*
  * **extension** (*str*) - расширение файла кеша
  * **task_data** (*dict*) - читаемая задача(словарь), если *False* - значит предполагается, что *task* инициализирован. ``лучше не использовать``
  * **return**:
      * (*True, cache_versions_list*)  где *cache_versions_list* список кортежей - [*(num (str), ob_name,  path), ...*]
      * (*False, коммент*)

.. py:function:: get_final_cache_file_path(cache_dir_name[, activity = 'cache', extension = '.pc2', task_data=False])

  путь к последней версии кеша для экземпляра.

  .. rubric:: Параметры:

  * **cache_dir_name** (*str*) - "*asset_name*" + "_" + "*ob_name*"
  * **activity** (*str*) - по умолчанию *"cache"* (для *blender*) - для других программ может быть другим, например "*maya_cache*"
  * **extension** (*str*) - расширение файла кеша
  * **task_data** (*dict*) - читаемая задача, если *False* - значит предполагается, что *task* инициализирован. ``лучше не использовать``
  * **return**  - (*True, path*) или (*False, коммент*)

.. py:function:: get_new_cache_file_path(cache_dir_name[, activity = 'cache', extension = '.pc2', task_data=False])

  путь к новой версии кеша для экземпляра.

  .. rubric:: Параметры:

  * **cache_dir_name** (*str*) - "*asset_name*" + "_" + "*ob_name*"
  * **activity** (*str*) - по умолчанию "*cache*" (для *blender*) - для других программ может быть другим, например "*maya_cache*"
  * **extension** (*str*) - расширение файла кеша
  * **task_data** (*dict*) - читаемая задача, если *False* - значит предполагается, что *task* инициализирован. ``лучше не использовать``
  * **return** - (*True, (new_dir_path, new_file_path)*) или (*False, коммент*)

.. py:function:: get_version_cache_file_path(version, cache_dir_name[, activity = 'cache', extension = '.pc2', task_data=False])

  путь к определённой версии файла кеша экземпляра.

  .. rubric:: Параметры:

  * **version** (*str*) - *hex* 4 символа
  * **cache_dir_name** (*str*) - "*asset_name*" + "_" + "*ob_name*"
  * **activity** (*str*) - по умолчанию *"cache"* (для *blender*) - для других программ может быть другим, например *"maya_cache"*
  * **extension** (*str*) - расширение файла кеша
  * **task_data** (*dict*) - читаемая задача, , если *False* - значит предполагается, что *task* инициализирован. ``лучше не использовать``
  * **return_data** - (*True, path*) или (*False, коммент*)
  
Создание задач
~~~~~~~~~~~~~~

.. py:function:: create_tasks_from_list(list_of_tasks)

  создание задач ассета по списку.
  
  .. note:: *task.asset* - должен быть инициализирован

  .. rubric:: Параметры:

  * **list_of_tasks** (*list*) - список задач (словари по *tasks_keys*)
  * **return** - (*True, 'ok'*) или (*False, коммент*)

.. py:function:: add_single_task(task_data)

  создание одной задачи.
  
  .. note:: *task.asset* - должен быть инициализирован.
  
  .. note:: обязательные поля в *task_data*: *activity*, *task_name*, *task_type*, *extension*, если передать поля *input*, *output* - то будут установлены соединения и призведены проверки, и смены статусов

  .. rubric:: Параметры:

  * **return** - (*True, 'ok'*) или (*False, коммент*)
  
Редактирование
~~~~~~~~~~~~~~

.. py:function:: change_activity(new_activity)

  замена активити текущей задачи

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **new_activity** (*str*)
  * **return_data** -  (*True, task_data*) или (*False, коммент*)

.. py:function:: change_price(new_price)

  замена стоимости текущей задачи

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **new_price** (*float*)
  * **return** -  (*True, task_data*) или (*False, коммент*)

.. py:function:: changes_without_a_change_of_status(key, new_data, task_data=False)

  замена параметров задачи, которые не приводят к смене статуса.

  .. rubric:: Параметры:

  * **key** (*str*) - ключ для которого идёт замена
      * допустимые ключи для замены:
          * *activity*
          * *task_type*
          * *season*
          * *price*
          * *tz*
          * *extension*
  * **new_data** (по типу ключа) - данные на замену
  * **task_data** (*bool/dict*) - изменяемая задача, если *False* - значит предполагается, что *task* инициализирован. ``лучше не использовать``
  * **return** - (*True, 'ok'*) или (*False, коммент*)

.. py:function:: add_readers(add_readers_list)

  добавление проверяющих для текущей задачи.

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **add_readers_list** (*list*) - список никнеймов проверяющих
  * **return** - (*True, readers(dict - в формате записи как в задаче), change_status(bool)*) или (*False, коммент*)

.. py:function:: make_first_reader(nik_name)

  обозначение превого проверяющего, только после его проверки есть смысл проверять остальным проверяющим, и только после его приёма данная задача появится в списке на проверку у остальных читателей. Предполагается что это технический проверяющий от отдела, где идёт работа.

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **nik_name** (*str*) - никнейм артиста
  * **return** - (*True, readers(dict - в формате записи как в задаче)*) или (*False, коммент*)

.. py:function:: remove_readers(remove_readers_list)

  удаляет проверяющего из списка проверяющих, а также удалит его как первого проверяющего, если он таковой.

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **remove_readers_list** (*list*) - список никнеймов удаляемых из списка читателей
  * **return** - (*True, readers(dict - в формате записи как в задаче), change_status(bool)*) или (*False, коммент*)

.. py:function:: change_artist(new_artist)

  замена артиста и возможная замена при этом статуса.

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **new_artist** (*str/artist*) - *nik_name* или *artist* (экземпляр), лучше передавать экземпляр для экономии запросов
  * **return_data** - (*True, (new_status, int(artist_outsource))*) или (*False, коммент*)

.. py:function:: change_input(new_input)

  изменение входа не сервисной задачи, с вытикающими изменениями статусов.

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **new_input** (*str*) - имя новой входящей задачи
  * **return** - (*True, (new_status, old_input_task_data, new_input_task_data)*) или (*False, коммент*)

.. py:function:: accept_task()

  приём задачи, статус на *done* (со всеми вытикающими сменами статусов), создание паблиш версии, выполнение хуков.

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **return** - (*True, 'ok'*) или (*False, коммент*)

.. py:function:: readers_accept_task(current_artist)

  приём задачи текущим проверяющим, изменение статуса в *task.readers*, если он последний то смена статуса задачи на *done* (со всеми вытикающими сменами статусов).

  .. note:: *task* - должен быть инициализирован.

  .. rubric:: Параметры:

  * **current_artist** (*artist*) - экземпляр класса артист, должен быть инициализирован - *artist.get_user()*
  * **return** - (*True, 'ok'*) или (*False, коммент*)

.. py:function:: close_task()

  закрытие задачи, смена статуса на *close* (со всеми вытикающими сменами статусов)

  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **return** - (*True, 'ok'*) или (*False, коммент*)

.. py:function:: rework_task(current_user = False, task_data=False)

  отправка задачи на переработку из статуса на проверке, при этом проверяется наличие свежего (последние 30 минут) коментария от проверяющего. продолжение возможно только после редактирования *chat().read_the_chat()*

  .. rubric:: Параметры:

  * **task_data** (*dict*) - изменяемая задача, если *False* - значит предполагается, что *task* инициализирован
  * **current_user** (*artist*) - экземпляр класса артист, должен быть инициализирован - *artist.get_user()* - если *False* - то чат проверятся не будет (для тех нужд)
  * **return** - (*True, 'ok'*) или (*False, коммент*)

.. py:function:: return_a_job_task(task_data=False)

  возврат в работу задачи из завершённых статусов (*done*, *close*).

  .. rubric:: Параметры:

  * **task_data** (*dict*) - изменяемая задача, если *False* - значит предполагается, что *task* инициализирован
  * **return** - (*True, new_status*) или (*False, коммент*)

.. py:function:: change_work_statuses(change_statuses)

  тупо смена статусов в пределах рабочих, что не приводит к смене статусов исходящих задач.
  
  .. note:: *task* - должен быть инициализирован

  .. rubric:: Параметры:

  * **change_statuses** (*list*) - [*(task_ob, new_status), ...*]
  * **return_data** - (*True, {task_name: new_status, ... } *) или (*False, коммент*)
