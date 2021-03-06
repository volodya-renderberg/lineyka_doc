.. _hooks-page:

Hooks
=====

	Процедуры проектного уровня.
	
	.. rubric:: Хранение:

	* дефолтные хуки в - *lineyka/hooks/*
	* для студийных работников - *studio/project/hooks/*
	* для аутсорсеров - *work_folder/project/hooks/*
	
	.. rubric:: Функционал:
	
	* При создании нового проекта хуки загружаются из приложения. 
	* Импорт экспорт из проекта в проект (по аналогии с *set_of_tasks*)
	* Аутсорсеру:
		* заливается при создании проекта.
		* функция обновления.
	* Обнуление до дефолта(загрузка из приложения) по типам процессов.
	
	.. rubric:: Виды хуков:
	
	* *pre* - Проверка удовлетворения условиям, возврат (*False, message*) или (*True, 'Ok!'*)
	* *post* - Совершение действий с файлами.
	
	.. rubric:: Процессы для которых используются хуки:
	
	* **look**
	* **open**
	* **commit**
	* **push**
	* **publish**
	* **send_to_report**
	* **accept**
	
	.. rubric:: Запускаются в *task*:
	
	* *pre_commit()*, 
	* *post_commit()*... 
	* Итд
	
	.. rubric:: Начинка.
	
	* Передаваемые данные в хук:
		* *task*
		* *source_path* - путь откуда копируется файл (структура зависит от типа задачи).
		* *target_path* - путь куда перекладывается файл (структура зависит от типа задачи).
	* Импорты библиотек приложений через *try/except*.
		* *None* по умолчанию.
		* В зависимости от значения библиотек выбирать действия. 
		* Если библиотека *None* значит открыто через юзер интерфейс, возможно обращение к файлу в фоне.
	
	.. rubric:: Оперативка:
	
	* *look/open/commit/push/publish* - включить ранее написанное перетаскивание кеша .pc2
