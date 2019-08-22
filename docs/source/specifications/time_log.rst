.. time-log-page:

Time Log specification
======================

task атрибуты:
--------------

* **open_time**:
    * не имеет значения записывается или нет в бд
    * время открытия задачи, назначается в функции *open*
* **time** - стнет *json* - {юзер: время (итоговое)}
* **full_time** - Время общее всех юзеров

task.log (project-level)
------------------------

* Добавить в :ref:`class-log-page` затраченое время - атрибут ``time``
    * запись при каждом коммите
    * при открытии задачи заполнять атрибут ``task.open_time``
    * при комите записывать разницу в ``log.time`` и обновлять ``task.open_time`` временем коммита.
    * после внесения записи в ``log.time`` суммирование в: ``task.time``, ``task.full_time``, ``artist_log.full_time``

artist_log (studio-level)
-------------------------

.. note:: :ref:`class-log-page` добавить методы для чтения-записи принимающие объект *artist*.

* **table_name**: ``nik_name:log``
* **table_columns**: ``[project_name, task_name, full_time, $, start, finish]`` (новый словарь *studio.artist_log_keys*)
    * **full_time** - ссумируется при каждом коммите.
    * **$** - вносится по принятию задачи.
    * **finish** - вносится по принятию задачи (Время принятия, чтобы не было проблем с оплатой за период).