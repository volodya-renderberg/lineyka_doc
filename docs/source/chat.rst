.. _class-chat-page:

Class Chat
==========

**chat(studio)**

**level** = 'project'

Данные хранимые в БД (имя столбца : тип данных):

.. code-block:: python

  chats_keys = {
    'message_id':'text',
    'date_time': 'timestamp',
    'date_time_of_edit': 'timestamp',
    'author': 'text',
    'topic': 'json',
    'color': 'json',
    'status': 'text',
    'reading_status': 'text',
    }
    
Создание экземпляра класса:
---------------------------

.. code-block:: python
  
  import edit_db as db
  
  project = db.project()
  asset = db.asset(project)
  task = db.task(asset)
  
  chat = db.chat(task) # task - обязательный параметр при создании экземпляра chat
  # доступ ко всем параметрам и методам принимаемого экземпляра chat - через chat.task
  
Атрибуты
--------

:message_id: (*str*) -
:date_time: 'timestamp',
:date_time_of_edit: 'timestamp',
:author: (*str*) -
:topic: (*dict*) -
:color: (*dict*) - ``??``
:status: (*str*) -
:reading_status: (*str*) -
:task: (*task*) - экземпляр :ref:`class-task-page` принимаемый при создании экземпляра класса, содержит все атрибуты и методы :ref:`class-task-page`.