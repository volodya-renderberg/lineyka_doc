Class Group
===========

**group(studio)**

**level** = 'project'

.. code-block:: python
  
  group_keys = {
  'name': 'text',
  'type': 'text',
  'season': 'text',
  'description': 'text',
  'id': 'text',
  }
  
Создание экземпляра класса:

.. code-block:: python
  
  import edit_db as db
  
  project = db.project()
  group = db.group(project) # объект project - обязательный параметр при создании экземпляра group
  # доступ ко всем параметрам и методам принимаемого объекта project через group.project
  
.. py:function:: init(group_name[, new = True])

  заполнение полей объекта по *group_keys*.

  **Параметры:**
  
  * **group_name** (*str*) - имя группы. данные группы будут считаны из базы данных
  * **new** (*bool*) - если *True* - то возвращается новый инициализированный объект класса group, если *False* - то инициализируется текущий объект
  * **return:** 
      :если new= *True*: - инициализированный объект, 
      :если new= *False*: - (*True, 'Ok!'*) / или (*False, comment*)

.. py:function:: init_by_keys(keys[, new=True])

  заполнение полей объекта по *group_keys*.

  **Параметры:**
  
  * **keys** (*dict*) - словарь данных группы по *group_keys*
  * **new** (*bool*) - если *True* - то возвращается новый инициализированный объект класса *group*, если *False* - то инициализируется текущий объект
  * **return:**
      :если new= *True*: - инициализированный объект
      :если new= *False*: - (*True, 'Ok!'*)

.. py:function:: create(keys[, new=True])

  создание группы

  **Параметры:**
  
  * **keys** (*dict*) - словарь по *group_keys* (*name* и *type* (тип ассетов) - обязательные ключи)
  
  .. note:: если *type* подразумевает привязку к сезону(*type* из *studio.asset_types_with_season*), то *season* - так же обязательный параметр.
  
  * **new** (*bool*) - возвращать новый объект или инициализировать текущий
  * **return:**
      :если *new* = *True*: - (*True, new_group (group)*)
      :если *new* = *False*: - (*True, 'Ok!'*) или (*False, comment*)

.. py:function:: create_recycle_bin()

  создание группы - корзина, для удалённых ассетов. Процедура выполняется при создании проекта.

  **Параметры:**

  * **return** - (*True, 'Ok!'*) или (*False, comment*).

.. py:function:: get_list([f = False])

  возвращает список групп (объекты) согласно фильтру f.
  
  .. note:: заполняет ``поля класса``: **list_group**, **dict_by_name**, **dict_by_id**, **dict_by_type**

  **Параметры:**
  
  * **f** (*list / bool*) - *False* или список типов(типы ассета)
  * **return** (*True, [список групп - объекты]*)  или (*False, comment*).

.. py:function:: get_by_keys(keys)

  возвращает список групп(объекты) удовлетворяющих *keys*.

  **Параметры:**
  
  * **keys** (*dict*) - словарь по *group_keys*
  * **return** (*True, [список групп - объекты]*)  или (*False, comment*)

.. py:function:: get_by_name(name)

  возвращает группу(объект) по имени.
  
  .. note:: Обёртка на *get_by_keys()*

  **Параметры:**
  
  * **name** (*str*) - имя группы
  * **return** (*True, группа - объект*)  или (*False, comment*)

.. py:function:: get_by_id(id)

  возвращает группу(объект) по *id*.
  
  .. note:: Обёртка на *get_by_keys()*

  **Параметры:**
  
  * **id** (*str*) - *id* группы
  * **return** (*True, группа - объект*)  или (*False, comment*)

.. py:function:: get_by_season(season)

  возвращает список групп(объекты) данного сезона.
  
  .. note:: Обёртка на *get_by_keys()*

  **Параметры:**
  
  * **season** (*str*) - сезон
  * **return** (*True, [список групп - объекты]*)  или (*False, comment*)

.. py:function:: get_by_type_list(type_list)

  возвращает список групп(словари) по списку типов.
  
  .. note:: Обёртка на *get_list()*

  **Параметры:**
  
  * **type_list** (*list*) - список типов ассетов из *asset_types*
  * **return** (*True, [список групп - объекты]*)  или (*False, comment*)

.. py:function:: rename(new_name)

  переименование текущего объекта группы.

  **Параметры:**
  
  * **new_name** (*str*) - новое имя группы
  * **return** - (*True, 'Ok!'*) или (*False, comment*)

.. py:function:: edit_comment(comment)

  редактирование комментария текущего объекта группы.

  **Параметры:**
  
  * **comment** (*str*) - текст коментария
  * **return** - (*True, 'Ok!'*) или (*False, comment*)
