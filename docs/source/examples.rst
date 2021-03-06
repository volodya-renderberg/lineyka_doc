Examples
========

Здесь содержится необходимый минимум для написания плагина *lineyka* пользовательского уровня: доступ к задачам, доступ к файлам задач, доступ к чату, отправка задач на проверку (для приложения поддерживающего *python*)

Требования
----------

  * модуль плагина должен содержать следующие файлы из *lineyka*:
      * *edit_db.py*
      * *lineyka_chat.py*
      * *run_chat.py*
      * *run2_chat.py*
  * в *python* приложения или системы должен быть установлен *PySide*
      * версия *python* 2.7+
  * в сиситеме должен быть установлен *imagemagick*
  
Создание тестового проекта
--------------------------

Запуск и тесты пользовательского плагина имеет смысл лишь при созданном проекте, ассетах, задачах и назначенных на них исполнителях, чтобы создать всё это не вдаваясь во все тонкости работы с менеджерской панелью предлагаются нижеследующие команды, которые надо выполнить в терминале:

.. note:: предполагается что *git* установлен
  
.. code-block:: python

  # клонирование lineyka в домашнюю директорию
  cd ~
  git clone https://github.com/volodya-renderberg/lineyka.git
  
  # переключение ветки
  cd ~/lineyka
  git checkout database
  
  # создание ярлыков рабочего стола (linux)
  python setup.py 
  
  # создание проекта, ассетов, задач, назначение исполнителей на задачи
  python test.py
  
Студия, проект и ассеты создаются в системной *tmp* директории:
  * проект - *Project*
  * группы - *props* ``obj``, *locations* ``location``
  * ассеты - *topor*, *vedro*, (``props``) *location_01* (``locations``)
  * исполнители - *vofka* ``root``, *dimka*, *slavka* ``users`` (все пароли *1234*)
  
Команды python api
------------------

.. note::
  все методы как правило возвращают кортежи, первый элемент которых булевское значение, если оно *True* - то выполнение процедуры удалось, если *False* - то выполнение не удалось, и вторым элементом кортежа, в этом случае, будет сообщение о данной ошибке. Более подробно о каждой процедуре смотрите в описании методов классов.
      
Студия
~~~~~~

подробнее о :ref:`class-studio-page`

.. code-block:: python

  import edit_db as db
  
  # создание экземпляра класса
  # при создании экземпляра выполняется чтение настроек, заполнение поллей класса и экземпляра.
  # создание экземпляра обязательно при запуске полагина.
  studio = db.studio()
  
  # изменение студийных настроек:
  # (1) установка директории студии, где будет хранится метадата
  studio.set_studio(path_to_studio_folder)
  # после выполнения этой процедуры бывает лучше перезапустить плагин
  
  # (2) определение пути до файла convert.exe, приложения *imagemagick* - для линукса это '/usr/bin/convert'
  path_to_convert_exe = '/usr/bin/convert' # или другой путь
  studio.set_convert_exe_path(path_to_convert_exe)
  
  # (3) определение пути до tmp директории, где будут находится временные версии рабочих файлов
  # по умолчанию это системная tmp директория
  # является настройкой пользователя, определяется по его желанию
  studio.set_tmp_dir(path) # path путь до пользовательской tmp директории


Артист
~~~~~~

подробнее о :ref:`class-artist-page`

Авторизация
"""""""""""

.. code-block:: python

  artist = db.artist() # создание экземпляра
  
  artist.get_user() # инициализация текущего пользователя - заполнение полей экземпляра.
                    # если нет авторизованного пользователя, то artist.nikname останется False
                    # тогда надо будет выполнить авторизацию.
  
  artist.login_user(nik_name, password) # авторизация пользователя
  

Списки задач артиста
""""""""""""""""""""

.. code-block:: python
  
  # получение списка задач назначенных на исполнение артисту (для данного проекта)
  task_list = artist.get_working_tasks(project, statuses=artist.working_statuses.append('checking'))[1] # project - это экземпляр класса project
                                                                                                        # task_list - это список экземпляров класса task
  
  # получение списка задач назначенных на проверку артисту (для данного проекта)
  task_list = artist.get_reading_tasks(project, statuses='checking')[1] # project - это экземпляр класса project
                                                                        # task_list - это список экземпляров класса task


Ассет
~~~~~

подробнее о :ref:`class-asset-page`

подробнее о :ref:`class-group-page`

Списки ассетов
""""""""""""""

.. code-block:: python

  # (1) создание экземпляра
  asset = db.asset(project) # project - это экземпляр класса project

  # (2) получение списка ассетов по типу
  assets_list = asset.get_list_by_type(asset_type = type)[1] # type - тип из studio.asset_types
                                                             # assets_list - это список экземпляров класса asset
  
  # (3) получение списка ассетов группы
  
  # (3.1) получение списка групп
  group = db.group(project) # project - это экземпляр класса project
  groups_list = group.get_list(f = list_of_types)[1] # list_of_types - это список типов ассетов из studio.asset_types
                                                     # groups_list - это список экземпляров групп
  
  # (3.2) получение списка ассетов группы
  assets_list = asset.get_list_by_group(group)[1] # group - это экземпляр класса group из groups_list, полученный выше
                                                  # assets_list - это список экземпляров класса asset


Списки задач ассетов
""""""""""""""""""""

.. code-block:: python

  task = db.task(asset) # asset - это экземпляр класса asset, любой из списка assets_list, полученный выше
  
  tasks_list = task.get_list()[1] # tasks_list - это список задач данного ассета, экземпляры класса task
  
.. note::

  Среди задач будут и задачи с типом *'service'*, они не содержат файловой структуры и не используются артистом.


Задачи
~~~~~~

подробнее о :ref:`class-task-page`

Создание экземпляра
"""""""""""""""""""

.. code-block:: python

  task = db.task(asset) # asset - это экземпляр класса asset
  
Открытие или просмотр файла задачи
""""""""""""""""""""""""""""""""""

Отличие просмотра от открытия файла задачи
******************************************
  
* **Открытие** (*open*) - открываются рабочие файлы только тех задач, которые назначенны на авторизированного пользователя (из списка рабочих задач *артиста*) (см. `Списки задач артиста`_ ). Статус открываемой задачи меняется на *work*, и если у данного пользователя есть какая-либо другая задача со статусом *work* - её статус меняется на *pause*

* **Просмотр** (*look*) - открываются файлы любых задач, не зависимо от пользователя. Статусы задач не меняются. Используется проверяющими или для получения чего либо из файла задачи.

.. note::

  В обеих случаях (открытие или просмотр) файл из активити задачи будет скопирован в *studio_tmp* директорию (определяется в *studio.set_tmp_dir()*) и открыт от туда, таким образом оригиналы версий рабочих файлов защищены от нежелательных правок.
  
Открытие или просмотр последней версии рабочего файла задачи
************************************************************
  
.. code-block:: python

  # запуск последней версии рабочего файла задачи
  
  # (1) получение пути к файлу (и смена статусов для open):
  # (1.1) open
  open_path = task.open_file(launch=False)[1] # будут произведены все смены статусов, последняя версия файла активити будет скопирована в tmp
                                              # open_path - это путь до копии файла в tmp
                                              # task - экземпляр данной задачи
  # (1.2) look
  look_path = task.open_file(look=True, launch=False)[1] # смены статусов не будет, последняя версия файла активити будет скопирована в tmp
                                              # look_path - это путь до копии файла в tmp
                                              # task - экземпляр данной задачи
  # (2) Далее надо открыть look_path или open_path методом данного приложения.
  
Открытие или просмотр произвольной версии рабочего файла задачи
***************************************************************

.. code-block:: python
  
  # запуск произвольной версии рабочего файла задачи
  
  # (1) получение списка версий (чтение push логов)
  log = db.log(task) # создание экземпляра класса log
                     # task - экземпляр текущей задачи
  
  logs_list = log.get_push_logs()[1] # logs_list - это список словарей [log_dict, ...] по ключам studio.logs_keys
                                     # каждый словарь log_dict - это и есть запись лога хранимая в БД
                                     # данные этих словарей можно отображать в таблице, для выбора версии
                                     # версия лога - это значение log_dict['version'] - (hex 4 символа)
                                     
  # (2) получение пути к файлу (и смена статусов для open):
  # (2.1) open
  open_path = task.open_file(version=log_dict['version'], launch=False)[1] # будут произведены все смены статусов, указанная версия файла активити будет скопирована в tmp
                                                                           # open_path - это путь до копии файла в tmp
                                                                           # task - экземпляр данной задачи
                                                                           # log_dict - словарь лога, полученный в процедуре log.get_push_logs() (пункт 1)
  # (2.2) look
  look_path = task.open_file(version=log_dict['version'], look=True, launch=False)[1] # смены статусов не будет, указанная версия файла активити будет скопирована в tmp
                                                                                      # look_path - это путь до копии файла в tmp
                                                                                      # task - экземпляр данной задачи
                                                                                      # log_dict - словарь лога, полученный в процедуре log.get_push_logs() (пункт 1)
  
  # (3) Далее надо открыть look_path или open_path методом данного приложения.
  
подробнее о :ref:`class-log-page`
  
Открытие или просмотр файла задачи из указанного файла
******************************************************

.. code-block:: python

  # запуск рабочего файла задачи по указанному пути (this_path)
  
  # (1) копирование указаного файла в tmp и получение пути (смена статусов для open):
  # (1.1) open
  open_path = task.open_file(open_path=this_path, launch=False)[1] # будут произведены все смены статусов, последняя версия файла активити будет скопирована в tmp
                                              # open_path - это путь до копии файла в tmp
                                              # this_path - это путь до указанного файла
                                              # task - экземпляр данной задачи
  # (1.2) look
  look_path = task.open_file(open_path=this_path, look=True, launch=False)[1] # смены статусов не будет, последняя версия файла активити будет скопирована в tmp
                                              # look_path - это путь до копии файла в tmp
                                              # this_path - это путь до указанного файла
                                              # task - экземпляр данной задачи
  # (2) Далее надо открыть look_path или open_path методом данного приложения.
  
Push новой версии рабочего файла
""""""""""""""""""""""""""""""""
  
.. code-block:: python

  push_path = task.push_file(description, current_file, current_artist=current_artist)[1] # текущий рабочий файл будет скопирован в новую версию активити данной задачи
                                              # push_path - путь до созданного файла в директории активити задачи
                                              # task - экземпляр данной задачи
                                              # description - комментарий к версии (обязательный параметр)
                                              # current_file - путь к текущему рабочему файлу (получить методом данного приложения)
                                              # current_artist - текущий пользователь, экземпляр класса artist
  
Отправка задачи на проверку
"""""""""""""""""""""""""""

.. code-block:: python

  task.to_checking() # task - экземпляр текущей задачи
                     # статус задачи поменяется на 'checking'
                     # данная задача будет отображаться в списке на проверку в user интерфейсе
                     
Отправка на переделку или приём задачи проверяющим
""""""""""""""""""""""""""""""""""""""""""""""""""

.. note:: Речь идёт о задачах из списка на проверку данного артиста: `Списки задач артиста`_

.. code-block:: python

  task.rework_task(current_artist) # отправка задачи на переделку
                                   # при этом проверяется наличие свежего (последние 30 минут) коментария в чате от проверяющего
                                   # task - экземпляр данной задачи
                                   # current_artist - экземпляр класса artist (текущий юзер)
                                   # статус задачи поменяется на 'recast'
  
  task.readers_accept_task(current_artist) # приём задачи проверяющим
                                   # task - экземпляр данной задачи
                                   # current_artist - экземпляр класса artist (текущий юзер)
                                   # если данный проверяющий единственный или последний - то статус задачи поменяется на 'done'
                                   # если проверяющий не последний - то изменится лишь статус проверки


Чат
~~~

Запуск чата задачи:
"""""""""""""""""""

Для случая когда python приложения содержит библиотеку PySide
*************************************************************

.. code-block:: python

  import sys
  import run_chat
  
  sys.call_tracing(run_chat.run, (task,)) # запустится интерфейс(PySide) чата задачи
                                          # task - экземпляр текущей задачи
                                          
Для случая когда python приложения не содержит PySide
*****************************************************

.. note:: PySyde должен быть установлен в python системы.

.. code-block:: python

  import sys
  import run2_chat
  
  sys.call_tracing(run2_chat.run, (project.name, task.task_name)) # запустится интерфейс(PySide) чата задачи
                                          # project - экземпляр текущего проекта
                                          # task - экземпляр текущей задачи