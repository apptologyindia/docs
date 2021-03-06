**************************
Конвертация валют в PayPal
**************************

Модуль "Платежи через PayPal" позволяет отправлять счета в PayPal в других валютах, а не только в базовой. Это нужно, если:

* счет продавца на PayPal отличается от базовой валюты в интернет-магазине;

* есть требования PayPal о проведении платежа в другой валюте. Например, платежи между резидентами России должны проводиться в рублях.

==============
Как настроить?
==============

Указать необходимую валюту можно в диалоге настройки платежной системы. В списке выбора валют будут отображены все доступные для платежной системы валюты.

Выбрать можно будет только ту валюту, которая была предварительно добавлена в магазине: для конвертации суммы из одной валюты в другую необходимо знать курс и отношение валюты к базовой валюте.

====================
Как добавить валюту?
====================

Чтобы в настройках PayPal нужная валюта была доступна для выбора, ее необходимо добавить в самом магазине (**"Адиминстрирование → Валюты"**). Требования:

* Необходимо указать код (Code) — должен быть равен 3х значному коду валюты по стандарту `ISO 4217 <https://ru.wikipedia.org/wiki/ISO_4217>`_.

* Необходимо указать курс (Rate) относительно базовой валюты.

После добавления валюта будет доступна в диалоге настройки платежной системы.

========================
Процесс отправки платежа
========================

-------------------------------------------------
Если валюта заказа и платежной системы отличаются
-------------------------------------------------

В платежную систему будет отправлена итоговая сумма заказа в валюте, указанной в настройках платежной системы. Конвертация суммы будет проведена по курсу, указанному в настройках валюты.

Для платежных систем, которые принимают, кроме итоговой суммы, список товаров, шиппинги, налоги, будет отправлена только итоговая сумма заказа. Это связанно с тем, что при конвертации есть риск потерять остатки, которые могут повлиять на итоговую сумму и в результате платежная система, при проверке входных данных, откажет в проведении платежа.

**Пример:**

* Состав заказа:

  * Товар 1: 211 руб.
    
  * Товар 2: 487 руб.

* Сумма заказа: 698 руб.

* Платежная система настроена с валютой USD.

* Курс: 1 USD = 63 RUB (0.0158)

**Результат:**

* Состав заказа:

  * Товар 1: 3.33 $.
    
  * Товар 2: 7.69 $.

* Сумма по товарам: 11.02 $. 

* Сумма заказа с конвертацией: 11.03 $.

**В итоге потерялся 0.01 $.**

Чтобы избежать таких проблем, в случае если валюты заказа и валюта платежной системы отличаются, будет отправлена только одна сумма - итоговая сумма заказа.

------------------------------------------------
Если валюта заказа и платежной системы совпадают
------------------------------------------------

В этом случае суммы по заказу останутся неизменными.

==================
Известные проблемы
==================

* Конвертация валют на стороне PayPal. Если у покупателя нет аккаунта с валютой выставленного счета, то для проведения платежа PayPal проведет внутреннюю конвертацию валют по своему курсу. Этот курс может отличаться от курса в магазине, в итоге покупатель потеряет 1% - 2% процента от покупки.

* Если у продавца в аккаунте PayPal нет валюты, в которой выставляются счета из магазина, то заказ останется в статусе "Open" до тех пор, пока продавец не подтвердит платеж в PayPal.
