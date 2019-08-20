.. additional-activities-page:

Additional Activities specification
===================================

дополнения в api
----------------

* Новый атрибут *additional_activities*
* Словарь по типу *asset.ACTIVITY_FOLDER*
* Добавить методы редактирования.
* Применение: редактирование словаря при инициализации ассета:
    * *asset.ACTIVITY_FOLDER* дополняется из *asset.project.additional_activities*
* Убрать псевдонимы
* Возможно добавить описание активити (вместо псевдонима)

дополнения в gui
----------------

* Добавляются по типам ассетов:
* Появляются в списке предлагаемых активити *set_of_tasks manager*
* Появятся в списке активити, в *task_manager*