************************
Служба доставки Boxberry
************************

Начиная с версии 4.6.1, в CS-Cart входит :doc:`модуль </user_guide/addons/boxberry/index>` для интеграции со `службой доставки Boxberry <http://boxberry.ru/>`_.

==========================
Настройка способа доставки
==========================

1. Откройте страницу **Администрирование → Доставка и налоги → Способы доставки**.

2. Нажмите кнопку **+** (*Добавить способ доставки*).

3. Откроется страница создания способа доставки. Обязательно задайте следующие настройки:

   * **Название** — введите название способа доставки, которое покупатель увидит при оформлении заказа; например, *Доставка до пункта выдачи Boxberry*.

   * **Расчёт тарифа** — выберите вариант *В режиме реального времени*.

   * **Перевозчик** — выберите *Boxberry*.

   * **Служба доставки** — выберите один из вариантов:

     * *Курьерская доставка*;

     * *Курьерская доставка (без наложенного платежа)*;

     * *Доставка до пункта выдачи*;

     * *Доставка до пункта выдачи (без наложенного платежа)*.

   .. fancybox:: img/boxberry_shipping_method_general.png
       :alt: Создание способа доставки Boxberry.

4. Нажмите кнопку **Создать**. У способа доставки появятся дополнительные вкладки. Перейдите на вкладку **Настроить**:

   * **API token** — получите его у сотрудников Boxberry. Также вы можете воспользоваться ссылкой для получения или найти API token в личном кабинете Boxberry.

   * **Код пункта приёма посылок** — код пункта выдачи, в который вы сдаёте посылки для доставки. Чтобы получить список пунктов выдачи, перейдите на страницу `"Справка API сервисы Boxberry" <http://api.boxberry.de/?act=info&sub=api_info_services>`_, найдите метод ``PointsForParcels`` и перейдите по ссылке этого метода.

   * **Вес по умолчанию (в граммах)** — чтобы стоимость доставки была рассчитана правильно, CS-Cart должен передавать в Boxberry вес каждого доставляемого товара. Если при создании или редактировании товара вы не указали вес, то будет передаваться значение из этого поля. 

     .. important::

         Чтобы стоимость доставки рассчитывалась правильно, укажите вес на странице редактирования у каждого товара.

   .. fancybox:: img/boxberry_shipping_method_configure.png
       :alt: Настройки способа доставки Boxberry.

5. Если необходимо, установите свои надбавки на стоимость доставки во вкладке **Стоимость доставки**.

6. Нажмите кнопку **Сохранить**.

   .. note::

       `В личном кабинете Boxberry <http://api.boxberry.de/?act=settings&sub=view>`_ есть дополнительные настройки. Там вы сможете отключить отображение определённых пунктов выдачи на карте и задать параметры расчёта стоимости доставки.

Теперь на витрине должен отображаться новый способ доставки, которым сможет воспользоваться покупатель. При нажатии на адрес пункта выдачи откроется карта во всплывающем окне, и покупатель сможет выбрать другой пункт выдачи.

.. fancybox:: img/boxberry_at_checkout.png
    :alt: Способ доставки Boxberry при оформлении заказа.

.. fancybox:: img/boxberry_map.png
    :alt: Карта пунктов выдачи Boxberry при оформлении заказа.

=======================================
Передача информации о заказе в Boxberry
=======================================

1. Откройте страницу **Заказы → Все заказы**, найдите нужный заказ и нажмите на его название.

2. Откроется страница с информацией о заказе. Нажмите **Создать отдельную отгрузку**.

   .. fancybox:: img/boxberry_order.png
       :alt: Создание отгрузки для Boxberry.

3. Появится всплывающее окно с информацией об отгрузке. Выберите перевозчика *Boxberry* и нажмите кнопку **Создать**.

   .. important::

       Чтобы получилось создать отгрузку и отправить в Boxberry информацию о заказе, в адресе доставки обязательно должен быть указан телефон покупателя.

   .. fancybox:: img/boxberry_shipment_popup.png
       :alt: Настройка отгрузки для Boxberry.

4. Отгрузка будет создана, а на странице заказа появится номер отслеживания.

   .. fancybox:: img/boxberry_tracking_number.png
       :alt: Номер отслеживания Boxberry на странице заказа в CS-Cart.

5. Для отгрузки можно распечатать этикетку. Для этого перейдите на список отгрузок по ссылке **Отгрузки** либо через меню **Заказы → Отгрузки → Способы доставки**. После этого нажмите на кнопку с изображением шестерёнки рядом с отгрузкой и выберите вариант **Распечатать этикетку**.

   .. fancybox:: img/boxberry_print_label.png
       :alt: Печать этикетки для отгрузки Boxberry в CS-Cart.
