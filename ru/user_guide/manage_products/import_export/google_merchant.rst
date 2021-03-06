**********************************************************
Как экспортировать данные товаров в Google Merchant Center
**********************************************************

CS-Cart позволяет экспортировать информацию о товарах в `Google Merchant Center <https://www.google.ru/retail/merchant-center/>`_. Для этого нужно экспортировать данные товаров файл, а затем `загрузить этот файл в Google <https://support.google.com/merchants/#topic=3404780>`_.

===========================
Шаг 1. Устанавливаем модули
===========================

#. Откройте страницу **Модули → Управление модулями** в панели администратора.

#. Перейдите на вкладку **Просмотреть все доступные модули**.

#. Убедитесь, что установлены модули **Конструктор прайс-листов** и **Прайс-лист для Google Base**; если нет, установите их.

   * Модуль :doc:`"Конструктор прайс-листов" </user_guide/addons/data_feeds/index>` позволяет экспортировать данные в CSV-файлах, которые организованы так, как требуют сторонние сервисы.

   * Модуль :doc:`"Прайс-лист для Google Base" </user_guide/addons/google_export/index>` позволяет использовать в прайс-листах поля, которые требует Google.

     .. fancybox:: img/data_feeds_and_google_export.png
         :alt: Чтобы экспортировать товары в Google Merchant Center, у вас в магазине должны быть установлены модули "Конструктор прайс-листов" и "Прайс-лист для Goolge Base".

=====================================
Шаг 2. Проверяем и редактируем товары
=====================================

.. important::

    На этом шаге нужно проверить, есть ли у вас в магазине `вся информация о товарах, которую нужно передавать в Google <https://support.google.com/merchants/answer/7052112>`_, и в правильном ли виде она представлена.

#. Некоторые из полей, которые нужны Google, уже созданы автоматически как :doc:`характеристики товара </user_guide/manage_products/features/index>` из группы **Google export features**. Вы увидите их на странице редактирования товара и сможете их задать, если они нужны.

   .. fancybox:: img/google_export_features.png
       :alt: Характеристики Google export features

#. В зависимости от товара или страны, Google может потребовать дополнительные поля. Если у вас их нет, то :doc:`создайте для них характеристики </user_guide/manage_products/features/product_features>` и задайте значения этих характеристик для нужных товаров.

   .. note::

       Если у вас есть :doc:`вариации товаров </user_guide/manage_products/products/product_variations>`, то `item_group_id <https://support.google.com/merchants/answer/6324507>`_ для них сгенерируется автоматически по принципу "один уникальный ID для отдельной позиции в каталоге".

===============================
Шаг 3. Создаём прайс-лист (фид)
===============================

#. :doc:`Создайте новый прайс-лист </user_guide/addons/data_feeds/create_df>` или отредактируйте существующий прайс-лист *Google base* в соответствии с вашими требованиями и `спецификациями Google <https://support.google.com/merchants/answer/7052112>`_. Вот пара важных моментов:

   * Когда вы создаёте прайс-лист для Google, обязательно выберите **Макет** *google_export*. Это позволит вам экспортировать опции товаров Google Merchant Center, как описано в шаге 2.2, но только если вы добавите эти опции на вкладке **Таблица соответствия полей**.

   * На вкладке **Общее** на странице редактирования прайс-листа выделите оба типа (*Простой товар* и *Вариация товара*). Выделить несколько типов можно, удерживая клавишу Ctrl.

   * Убедитесь, что код :doc:`основной валюты вашего магазина </user_guide/currencies/index>` соответствует стандарту `ISO 4217 <http://www.currency-iso.org/en/home/tables/table-a1.html>`_, а :doc:`в настройках </user_guide/settings/general>` используется одна из следующих единиц веса: lb, oz, g, kg.

   * GTIN является ключевым идентификатором товара. Если в характеристиках вы не указали GTIN, вместо него будет использован SKU товара из поля CODE.

   * Правила, связанные с налогами:

     * Для США не следует добавлять налог к стоимости товара.

     * Для Канады и Индии не следует добавлять налог на добавленную стоимость (НДС) к стоимости. 

     * В остальных случаях НДС необходимо добавлять к стоимости товара.

#. Чтобы создать файл фида, нажмите на кнопку с изображением шестерёнки и выберите **Загрузить**.

   .. fancybox:: img/download_data_feed.png
       :alt: Нажмите на кнопку с изображением шестерёнки и выберите "Загрузить", чтобы скачать файл фида.

===========================================
Шаг 4. Отправляем прайс-лист (фид) в Google
===========================================

У Google есть инструкции, как `зарегистрировать <https://support.google.com/merchants/answer/188475>`_ и `загрузить <https://support.google.com/merchants/answer/188477>`_ фид данных о товарах. Мы рекомендуем сначала загрузить тестовый фид и убедиться, что не возникло никаких ошибок.
