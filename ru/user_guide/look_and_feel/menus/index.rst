****
Меню
****

В данном разделе можно создавать пользовательские меню для витрины. Меню используются для упорядочивания контента (например, ссылок, страниц, категорий, и т.д.) на страницах магазина. Используя блок *Меню* на странице с макетами, можно разместить меню в любой части витрины. Меню позволяют предоставить всю необходимую информацию, не занимая слишком много места.

В CS-Cart используется два типа меню: **созданные вручную** и **динамические**.

* **Созданные вручную** меню используют контент, добавленный пользователем вручную, например, внутренние и внешние ссылки в меню **Быстрых ссылок**.

* **Динамические** меню используют контент самого магазина, например, дерево категорий в меню **Главное меню**. 

.. image:: img/menus.png
    :align: center
    :alt: Меню

Чтобы добавить новое меню, нажмите кнопку **+**. Чтобы изменить существующее меню, нажмите на кнопку с изображением **шестерёнки** рядом с выбранным меню и выберите **Редактировать**.

Чтобы создать новое меню и вывести его на витрину, выполните следующие шаги:

==========================
Шаг 1. Создаем пустое меню
==========================

.. note::

    Все меню создаются пустыми.

1. В панели администратора откройте страницу **Дизайн → Меню**.

2. Нажмите **+**, чтобы добавить меню.

3. В открывшемся окне укажите **Название** меню (например, *Новое меню*).

.. image:: img/menu_01.png
    :align: center
    :alt: Новое меню

4. Нажмите кнопку **Создать**.

=====================
Шаг 2. Заполняем меню
=====================

Вы можете заполнять меню вручную или используя контент магазина.

1. Нажмите кнопку с изображением **шестеренки** рядом с созданным меню и выберите **Редактировать элементы**. В открывшемся окне нажмите **+**.

2. В открывшемся окне укажите:

   * **Родительский элемент** — выберите подходящий родительский уровень.
   * **Название** — введите название элемента (например, *Новый элемент*).
   * **Поз.** — укажите позицию в списке элементов.
   * **URL** — введите URL страницы, которая будет открываться при переходе по ссылке (напрнимер, *index.php?dispatch=categories.catalog*).
   * **Элемент активен для** — элемент меню будет активен для указанной страницы (например, *sitemap.view*).

     .. note ::

         Если вы используете этот элемент для двух или более страниц, оставьте это поле пустым.

   * **Создать подменю** — выберите, требуется ли подменю для данного элемента. Подменю может включать *категорию* (подкатегории выбранной категории) или *страницу* (дочерние страницы выбранной страницы).
   * **Пользовательский CSS-класс** — CSS-класс будет добавлен к пункту меню. Это позволит задать CSS-правила для конкретного пункта меню.

.. image:: img/menu_02.png
    :align: center
    :alt: Новый элемент меню

3. Нажмите кнопку **Создать**.

============================
Шаг 3. Создаем блок для меню
============================

Чтобы вывести меню на витрину, вам потребуется создать :doc:`блок <../layouts/blocks/index>` типа *Меню*.

1. В панели администратора откройте страницу **Дизайн → Макеты**.

2. Нажмите **+** на контейнере, в котором будет располагаться блок, и выберите вкладку **Добавить блок**.

3. Переключитесь на вкладку **Создать новый блок** и в списке выберите тип контейнера **Меню**.

4. В открывшемся окне введите название блока (например, *Новое меню*) и нажмите кнопку **Создать**.

5. На созданном блоке нажмите на значок **шестерёнки** и укажите:

   * Шаблон в поле **Шаблон**.
   * Оболочку в поле **Оболочка**.
   * CSS-класс в поле **Пользовательский CSS-класс** при необходимости.

6. Переключитесь на вкладку **Контент** и в поле **Меню** выберите нужное меню из списка, или создайте новое меню, нажав на ссылку **Редактировать меню**.

.. image:: img/new_menu.png
    :align: center
    :alt: Новое меню

7. Нажмите **Сохранить**.

.. toctree::
    :maxdepth: 2
    :hidden:
    :glob:

    *
