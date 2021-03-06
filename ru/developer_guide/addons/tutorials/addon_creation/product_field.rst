**********************************************
6. Добавим новую настройку для товара. Часть 1
**********************************************

В прошлых уроках мы создали модуль, научились подключать ему стили и очищать кэш. 

Продолжим расширять функциональность модуля, добавим новое поле (настройку) для товара в панели администратора c помощью модуля.

1.  Пройдите на страницу редактирования товара в панели администратора.

    .. note::

        Панель администратора → Товары → Товары → Название любого товара

    .. fancybox:: img/howto_addon_10.png
        :alt: Первый модуль

2.  Обычно все модули добавляют настройки товаров в специальной вкладке «Модули».

    .. fancybox:: img/howto_addon_11.png
        :alt: Первый модуль

    Мы также добавим новую настройку в данную вкладку. 

    .. note::

        Вы можете добавлять новые настройки в любое место шаблона, где есть хуки.

3.  Найдём шаблон, который отображает страницу редактирования товара.

    Все шаблоны панели администратора расположены в папке:

    ``/design/backend/templates/``

    Шаблоны страниц расположены в папке:

    ``/design/backend/templates/view``

    Откройте данную папку на вашем сервере.

4.  Что дальше?

    Всё очень просто, папка ``/design/backend/templates/view`` разделена на подпапки по объектам ``products`` , ``categories`` и т.д. Какой нам нужен?

5.  Смотрим URL страницы в браузере.

    .. image:: img/howto_addon_13.png
        :alt: Первый модуль

    URL: ``ваш_домен/admin.php?dispatch=products.update&product_id=12``

    Параметр ``dispatch=products.update`` означает:

    *   ``products`` — папка в ``/design/backend/templates/view``

    *   ``update`` — название файла шаблона: **update.tpl**

6.  Открываем папку:

    ``/design/backend/templates/view/products``

    Находим и открываем файл:

    ``/design/backend/templates/view/products/update.tpl``

7.  Проверям, туда ли мы попали.

    Добавим ``<p>Test</p>`` куда нибудь в начало файла.

    .. literalinclude:: files/update.tpl
        :emphasize-lines: 3
        :language: smarty
        :linenos:

8.  Открываем страницу редактирования товара в браузере и смотрим результат. 

    Слово «Test» появилось. Отлично!

    .. image:: img/howto_addon_14.png
        :alt: Первый модуль

9.  Таким образом найден шаблон отображающий страницу в панели администратора. По этому принципу вы можете найти и изучить шаблоны других страниц. 

Идём дальше. Изучим шаблон. 

