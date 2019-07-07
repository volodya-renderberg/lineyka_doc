Class Workroom
==============

**workroom()**

.. code-block:: python

  workroom_keys = {
  'name': 'text',
  'id': 'text',
  'type': 'json' # список типов задач, которые выполняются данным отделом.
  }
  
.. py:function:: init_by_keys(keys[, new = True])

  заполнение полей объекта по ключам из keys, если new=True - то возвращает новый объект.
  
  **Параметры:**
  
  * **keys** (*dict*) - словарь по workroom_keys
  * **new** (*bool*) - если True - то возвращается новый инициализированный объект класса workroom, если False - то инициализируется текущий объект
  * **return** - (*True, 'Ok!'*) / workroom - новый инициализированный объект, или (*False, comment*)

.. py:function:: add(keys[, new=False])

  создание отдела.
  
  **Параметры:**
  
  * **keys** (*dict*) - словарь ключей по workroom_keys, name - обязательный параметр.
  * **new** (*bool*) - если True - то возвращает инициализированный объект.
  * **return** - (*True, 'Ok!'*) / workroom - новый инициализированный объект, или (False, comment)
  
.. py:function:: get_list([return_type = False, objects=True])

  получение списка отделов.
  
  .. note:: Заполняет поля ``класса``: **list_workroom**, **dict_by_name**, **dict_by_id**.
  
  **Параметры:**
  
  * **return_type** - параметр определяющий структуру возвращаемой информации:
      :False: возврат (*True, [список отделов - объекты или словари]*)
      :'by_name': возврат - (*True, {словарь по именам - значения отделы}*)
      :'by_id': возврат- (*True, {словарь по id - значения отделы}*)
  * **objects** (*bool*) - определяет в каком виде возвращаются отделы, если False - то словари, а если True - то объекты класса workroom
  * **return** - (*True, data*) , или (*False, comment*)

.. py:function:: get_name_by_id(id)
  
  возвращает имя отдела по его id.
  
  .. note:: возможно лучше не использовать
  
  **Параметры:**
  
  * **id** (*str*)- id отдела
  * **return** - (*True, workroom_name*) или (*False, комментарий*).

.. py:function:: get_id_by_name(name)

  возвращает id отдела по его имени.
  
  .. note:: возможно лучше не использовать
  
  **Параметры:**
  
  * **name** (*str*)- имя отдела.
  * **return** - (*True, workroom_id*) или (*False, комментарий*).

.. py:function:: name_list_to_id_list(name_list)

  возвращает список id по списку имён
  
  .. note:: возможно лучше не использовать
  
  **Параметры:**
  
  * **name_list** (*list*)- список имён
  * **return** - (*True, list_of_id*) или (*False, комментарий*).

.. py:function:: id_list_to_name_list(id_list)

  возвращает список имён по списку id
  
  .. note:: нужен при записи
  
  **Параметры:**
  
  * **id_list** (*list*)- список id
  * **return** - (*True, name_list*) или (*False, комментарий*).

.. py:function:: rename_workroom(new_name)

  переименование отдела (текущего объекта).  перезапись параметра name.
  
  **Параметры:**
  
  * **new_name** (*str*)- новое имя отдела.
  * **return** - (*True, 'Ok!'*) или (*False, комментарий*).

.. py:function:: edit_type(new_type_list)

  замена типов отдела (текущего объекта). перезапись параметра type. Отделу присваивается один или несколько типов задач - для которых он предназначен.
  
  **Параметры:**
  
  * **new_type_list** (*list*)- список типов из task_types
  * **return** - (*True, 'Ok!'*) или (*False, комментарий*).
