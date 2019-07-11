.. _class-studio-page:

Class Studio
============

**studio()**

**level** = 'studio'

Атрибуты
--------

:studio_folder: (*str*) - ``атрибут класса`` путь к директории хранения мета данных студии.

:tmp_folder: (*str*) - ``атрибут класса`` путь к директории хранения временных рабочих файлов.

:convert_exe: (*str*) - ``атрибут класса`` путь к *exe* файлу *convert* приложения *Image Magick*.

Методы
------

.. py:function:: set_studio(path)
  
  определяет директорию для хранения мета данных студии. Заполняет ``атрибут класса`` *studio_folder*. (см. `Атрибуты`_ )
  
  .. note:: в редких случаях там же могут создаваться и директории проектов (временные студии для аутсорса).
  
  .. rubric:: Параметры:
  
  * **path** (*str*) - путь к директории студии
  * **return** - (*True, 'Ok!'*) или (*False, Comment*)
  
.. py:function:: set_tmp_dir(path)

  определяет директорию для расположения временных рабочих файлов, по умолчанию системная темп директория. Заполняет ``атрибут класса`` *tmp_folder*. (см. `Атрибуты`_ )
  
  .. rubric:: Параметры:
  
  * **path** (*str*) - путь к директории
  * **return** - (*True, 'Ok!'*) или (*False, Comment*)
  
.. py:function:: set_convert_exe_path(path)

  определяет путь к *exe* файлу *convert* приложения *Image Magick*. Заполняет ``атрибут класса`` *convert_exe*. (см. `Атрибуты`_ )
  
  .. note:: Приложение *Image Magick* используется для редактирования картинок: конвертирование в *.png* при паблише и создания превью.
  
  .. rubric:: Параметры:
  
  * **path** (*str*) - путь к файлу *convert.exe*
  * **return** - (*True, 'Ok!'*) или (*False, Comment*)
  
.. py:function:: set_share_dir(path)

  пока не используется

.. py:function:: get_share_dir()

  пока не используется
  
.. py:function:: get_extension_dict()

  возвращает словарь соответствия расширений файлов и соответствующих им приложений. *extension_dict* - это словарь: **ключи** - расширения файлов, **значения** - экзешники для открытия этих типов файлов.

  .. rubric:: Параметры:
  
  * **return** - (*True, extension_dict*)  или (*False, Comment*)

.. py:function:: edit_extension_dict(key, path)

  редактирование словаря соответствия расширений файлов и соответствующих им приложений *extension_dict*.
  
  .. rubric:: Параметры:
  
  * **key** (*str*) - расширение файла с точкой, например: *'.blend'*
  
  * **path** (*str*) - путь к исполняемому файлу приложения, или имя исполняемого файла (если позволяют настройки переменной *PATH* системы)
  
  * **return** - (*True, 'Ok!'*) или (*False, Comment*)

.. py:function:: edit_extension(extension, action[, new_extension = False])
