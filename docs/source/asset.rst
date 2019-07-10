Class Asset
===========

**asset(studio)**

**level** = 'project'

Данные хранимые в БД (имя столбца : тип данных):

.. code-block:: python

  asset_keys = {
  'name': 'text',
  'group': 'text',
  'type': 'text',
  'season': 'text', # ``?``
  'priority': 'integer',
  'description': 'text',
  'content': 'text',
  'id': 'text',
  'status': 'text',
  'parent': 'json' # {'name':asset_name, 'id': asset_id} - возможно не нужно
  }
  
Создание экземпляра класса:
---------------------------

.. code-block:: python
  
  import edit_db as db
  
  project = db.project()
  
  asset = db.asset(project) # project - обязательный параметр при создании экземпляра asset
  # доступ ко всем параметрам и методам принимаемого экземпляра project через asset.project
  
Атрибуты
--------

:name: (*str*) - имя ассета (уникально)

:group: (*str*) - *id* группы

:type: (*str*) - тип ассета из *studio.asset_types*

:season: (*str*) - *id* сезона ``?``

:priority: (*int*) - [0 - inf]

:description: (*str*) - 

:content: (*str*) - ``?``

:id: (*str*) - *hex*

:status: (*str*) - *['active', 'none']*

:parent': (*dict*) - ``?``

:project: (*project*) - экземпляр *project* принимаемый при создании экземпляра класса, содержит все атрибуты и методы класса *project*.

Методы
------



