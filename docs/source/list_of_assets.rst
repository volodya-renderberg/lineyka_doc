.. _class-list_of_assets-page:

Class List_of_assets
====================

**list_of_assets(studio)**

**level** = 'project'

..

    Запись и редактирование временного списка ассетов {*имя, тип, set_of_tasks*} из редактора создания асетов. Запись в формат *json*, после создания ассетов, список очищается.

Данные хранимые в БД (имя столбца : тип данных):

.. code-block:: python

  list_of_assets_keys = [
    'asset_name',
    'asset_type',
    'set_of_tasks',
    ]