Class Project
=============

**project()**

.. code-block:: python

  projects_keys = {
  'name': 'text',
  'path': 'text',
  'status': 'text',
  'project_database': 'json',
  'chat_img_path': 'text',
  'list_of_assets_path': 'text',
  'preview_img_path': 'text',
  }
  
  
.. py:function:: add_project(project_name, project_path)

  создаёт проект.
  
  .. note:: при создании проекта новый объект не возвращается, заполняются поля текущего экземпляра.
  
  **Параметры:**
  
  * **project_name** (*str*) - имя проекта, если имя не указано, но указана директория, проект будет назван именем директории
  * **project_path** (*str - path*) - путь к директории проекта, если путь не указан, директория проекта будет создана в директории студии
  * **return** - (*True, 'Ok!'*) или (*False, comment*)

.. py:function:: init(name[, new=True])

  инициализирует объект по имени (заполняет поля экземпляра), чтение БД.
  
  **Параметры:**
  
  * **name** (*str*) - имя проекта
  * **new** (*bool*) - если new=True - возвращает новый инициализированный объект, если False то инициализирует текущий объект
  * **return** - project (*объект*) / (*True,  'Ok!'*) или (*False, comment*)

.. py:function:: init_by_keys(keys[, new=True])

  инициализирует объект по словарю (заполняет поля экземпляра), без чтения БД
  
  **Параметры:**
  
  * **keys** (*dict*) - словарь по projects_keys
  * **new** (*bool*) - если new=True - возвращает новый инициализированный объект, если False то инициализирует текущий объект
  * **return**  - project (*объект*) / (*True,  'Ok!'*) или (*False, comment*)

.. py:function:: get_list()

  чтение существующих проектов.
  
  .. note:: не возвращает объеткы, только заполняет ``поля класса``: **list_active_projects**, **list_projects**, **dict_projects**.
  
  * **заполняемые поля:**
      :list_active_projects: (list) - список активных проектов, только имена
      :list_projects:  (list) - список всех проектов (объекты)
      :dict_projects: (dict) - словарь содержащий все проекты (объекты) с ключами по именам
  * **return** - (*True,  'Ok!'*) или (*False, comment*)

.. py:function:: rename_project(new_name)
  
  переименование проекта (данного объекта), заполняются поля экземпляра, ``перезагружает studio.list_projects. ????``
  
  **Параметры:**
  
  * **new_name** (*str*) - новое имя отдела
  * **return** - (*True, 'Ok!'*) или (*False, comment*).

.. py:function:: remove_project()

  удаляет проект из БД (не удаляя файловую структуру), ``перезагружает studio.list_projects ???``, приводит объектк empty (все поля по projects_keys = False).
  
  **Параметры:**
  
  * **return** - (*True, 'Ok!'*) или (*False, comment*).

.. py:function:: edit_status(status)

  изменение статуса проекта.
  
  **Параметры:**
  
  * **status** (*str*) - присваиваемый статус
  * **return** - (*True, 'Ok!'*) или (*False, comment*)

.. py:function:: make_folders(root)

  создаёт файловую структуру проекта, при отсутствии.
  
  **Параметры:**
  
  * **root** (*str - path*) - корневой каталог проекта
  * **return** - *None*.
