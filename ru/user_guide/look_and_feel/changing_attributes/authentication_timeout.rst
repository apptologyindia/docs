**************************************************
Как поменять время сеанса работы с учетной записью
**************************************************

По умолчанию один сеанс работы идет 2 часа: после этого вам придется снова войти в учетную запись.

Изменим время сеанса:

1. Откройте файл **config.php** в папке с установленным CS-Cart.

2. Найдите этот фрагмент кода:

   ::

     // Session live time
     define('SESSION_ALIVE_TIME', SECONDS_IN_HOUR * 2); // 2 hours

3. Замените этот фрагмент кода на:

   ::

     // Session live time
     define('SESSION_ALIVE_TIME', TIMEOUT_IN_SECONDS);

.. note::

    Вместо **TIMEOUT_IN_SECONDS** напишите желаемое время сеанса в секундах.

4. Сохраните файл.

.. important::

    Эти изменения нужно будет внести заново, если вы обновите CS-Cart/Multi-Vendor.
