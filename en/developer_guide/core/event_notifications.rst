*******************
Event Notifications
*******************

.. note::

    The described functionality first appeared in version 4.11.1. 

CS-Cart and Multi-Vendor have a mechanism for multi-channel notifications about events, such as order creation, change of the status of an order, products being sent for administrator's review, creation of a shipment in the order, and so on.

This mechanism consists of the following primary components:

* Events

* Event dispatcher

* Messages

* Transports

* Receivers

* Notification settings

.. important::

    We have an example add-on that fully implements the mechanism for extending notifications: https://github.com/cscart/addon-notification-events-example.

.. contents::
   :backlinks: none
   :local:

======
Events
======

An event consists of the following things:

* A unique text identifier of the event.

* A set of data related to that event.

**Example:** event "Change of order details" has the ``'order.updated'`` identifier; it has the set of data about the order ``$data['order_info']`` received from the ``fn_get_order_info`` function.

.. _add-event:

-------------------------
How to Add Your Own Event
-------------------------

The **notifications/events.php** schema describes all the registered events, receivers, and transports of notifications.

The schema is available via the ``Tygh::$app['event.events_schema']`` service.

Every element of the schema has the following structure::

  (string) EventId => [
      'group' => (string) GroupId,
      'name' => [
          'template' => (string) TemplateLanguageVariable,
          'params' => [
              (string) SubstitutionName => (string) Substitution
              ...
          ],
      ],
      'data_provider' => (callable) DataProvider,
      'receivers' => [
          (string) ReceiverId => [
              (string) TransportId => BaseMessageSchema::create([
                  'area'            => (string) area,
                  'from'            => (string) from,
                  'to'              => (callable) DataValue::create(key),
                  'template_code'   => (string) template_code,
                  ...
                  'language_code'   => (callable) DataValue::create(parent_key.key, default_value),
                  'data_modifier'   => (callable) function (array $data) {
                      return array_merge($data, $added_data_value);
                  }
              ]),
              ...
          ],
          ...
      ],
  ],


* ``EventId``—identifier of the event. It is used as the first argument in ``TyghNotificationsEventDispatcher::dispatch``.
* ``GroupId``—identifier of the event group. It is used on the notification settings page for easier navigation.
* ``TemplateLanguageVariable``—the name of the language variable that stands for the event in the notification settings.
* ``SubstitutionName`` and ``Substitution``—the name and value of the parameters that adapt the language variable to the event's specifics.
* ``DataProvider``—implements the ``Notification\DataProviders\DataProvider`` interface; is used for fetching event-specific fields based on ``data`` passed to ``\Tygh\Notifications\EventDispatcher::dispatch``.
* ``ReceiverId``—identifier of the receiver. All the existing receiver identifiers can be retrieved from ``Tygh::$app['event.receivers_schema']``.
* ``TransportId``—identifier of the transport. Transports must provide it in ``\Tygh\Notifications\Transports\ITransport::getId``.
* ``BaseMessageSchema``—creates an instance of the schema class, with the data prepared for sending. Input parameters must include processed data necessary for sending the notification. Can be a text (``area``, ``from``, ``template_code``) or an instance of the ``DataValue`` class.
* ``DataValue``—the class that allows you to get the data from the input array by key. If the key isn't in the array, then the `default_value` will be used; it is ``null`` by default.
* ``data_modifier``—a callable parameter that allows you to make additional modifications of data passed in ``data`` within the function.

To register an event:

#. Select the unique event identifier (``EventId``).

#. Select the group to which the new event will belong.

   If the event belongs to a new group, create a language variable with the same identifier ``GroupId``, and the name of the event group as a value.

#. Create a language variable that will store the template for forming the name of the event (``TemplateLanguageVariable``).

#. Form a list of substitutions for the event name template (``SubstitutionName``, ``Substitution``).

   If the name of the event doesn't have substitutions, the ``'params'`` array must be left empty.

#. Specify the identifiers of user types that must receive notification about the event (``ReceiverId``).

#. Specify the name of the implemented ``DataProvider`` for processing of the event-specific data that come from ``data``.

#. For every receiver type, specify:

   * how they should receive notifications (``TransportId``);

   * the data specific for the current receiver and processed into an instance of the ``BaseMessageSchema`` class (using ``DataValue``, if necessary).

================
Event Dispatcher
================

Dispatcher is a system component that triggers events. Triggering an event sends the messages to the receivers.

Event dispatcher is registered in the ``Tygh::$app['event.dispatcher']`` service.

-----------------------
How to Trigger an Event
-----------------------

Call the event dispatcher in the places where you need to send notifications::

  Tygh::$app['event.dispatcher']->dispatch('EventId', ['order_info' => $order_info, 'user_info' => $user_info, 'settings' => $settings]);

========
Messages
========

Messages are formed based on a schema, from the data passed to the event dispatcher. A message contains all the data necessary for sending that message via a transport connected to this message type.

The array of data prepared for sending is made based on the rules from the **events.php** event schema.

Examples of implementation:

* Preparing the data for a notification about order state. This notification will be sent to administrator's email::

    'receivers' => [
        UserTypes::ADMIN => [
            MailTransport::getId() => MailMessageSchema::create([
                'area'            => 'A',
                'from'            => 'company_users_department',
                'to'              => 'company_users_department',
                'reply_to'        => DataValue::create('user_data.email'),
                'template_code'   => 'activate_profile',
                'legacy_template' => 'profiles/activate_profile.tpl',
                'company_id'      => DataValue::create('user_data.company_id'),
                'language_code'   => Registry::get('settings.Appearance.backend_default_language'),
                'data_modifier'   => function (array $data) {
                    return array_merge($data, [
                        'url' => fn_url('profiles.update?user_id=' . $data['user_data']['user_id'], 'A'),
                    ]);
                }
            ]),
        ],
    ],

``DataValue`` allows you to get data by key from the array that was passed to the dispatcher for forming the message. The function specified in ``data_modifier`` allows you to make more complex modifications of data.

The message schema is responsible for creating a message. It gets the processed data about the event and checks the validity of the data.

The schema is implemented for a specific tranpsort:

* ``\Tygh\Notifications\Transports\Mail\MailMessageSchema``—schema for a notification sent to email;

* ``\Tygh\Notifications\Transports\Internal\InternalMessageSchema``—schema for a notification sent to the Notification Center.

---------------------------
How to Add Your Own Message
---------------------------

To add a message:

#. Add a provider for the message data. The provider must implement the ``\Tygh\Notifications\DataProviders\IDataProvider`` interface or extend an existing base data provider class.

#. Specify that provider in the event schema for the specific transport.

#. In the events schema, specify the rules for processing the input data passed to the dispatcher.

++++++++++++++++++++++++++++++++++
How to Add a Message Sent to Email
++++++++++++++++++++++++++++++++++

These messages include the data necessary for sending an email via the ``Tygh::$app['mailer']`` service.

To create a new email message:

#. In the event schema, create the rules for preparing the data passed in ``\Tygh\Notifications\Transports\Mail\MailMessageSchema``.

#. The array passed to the ``create`` method of the schema contains the following properties:

   * ``to``—receiver of the message.

   * ``from``—sender of the message.

   * ``reply_to``—Reply-to of the message.

   * ``template_code``—the code of the email template.

   * ``legacy_template``—the name of the email template (if old email templates are used in the store).

   * ``language_code``—the code of the language in which email will be sent.

   * ``company_id``—identifier of the company on behalf of which the email is sent.

   * ``area``—from where the email is sent: from the admin panel or from the storefront.

   * Other keys passed in the ``$data`` array are the data for substitutions in the email template.

++++++++++++++++++++++++++++++++++++++++++++++++++++
How to Add a Message Sent to the Notification Center
++++++++++++++++++++++++++++++++++++++++++++++++++++

These messages include the data necessary for creating a notification in the Notification center via the ``Tygh::$app['notifications_center']`` service.

To create a new email message:

#. In the event schema, create the rules for preparing the data passed in ``\Tygh\Notifications\Transports\Internal\InternalMessageSchema``.

#. The array passed to the ``create`` method of the schema contains the following properties:

   * ``title``—the title of the notification.

     * ``template``—the name of the language variable.

     * ``params``—the list of substitutions in the template of the notification title; if the title doesn't have substitutions, the array must be left empty;

   * ``$message``—the text of the notification.

        * ``template``—the name of the language variable.

        * ``params``—the list of substitutions in the template of the notification text; if the text doesn't have substitutions, the array must be left empty;

   * ``severity``—severity of the notification (see ``\Tygh\Enum\NotificationSeverity``).

   * ``section``—the tab of the Notification center where the message will appear.

   * ``tag``—the tag that the notification will be marked with.

   * ``area``—the area where the notification will appear: in the administration panel or at the storefront.

   * ``action_url``—the link to which user will be directed after clicking the notification.

   * ``timestamp``—the time when the notification was created.

   * ``recipient_search_method``—the method for searching the users for whom the notifications must be created (see ``\Tygh\Enum\RecipientSearchMethods``).

     The following search methods are available:

     * ``\Tygh\Enum\RecipientSearchMethods::USER_ID``—by user ID.

     * ``\Tygh\Enum\RecipientSearchMethods::UGERGROUP_ID``—by user group ID (notifications will be created for all users in the group).

     * ``\Tygh\Enum\RecipientSearchMethods::EMAIL``—by user's email.

   * ``recipient_search_criteria``—user search criteria:

     * For ``recipient_search_method = \Tygh\Enum\RecipientSearchMethods::USER_ID``—user ID.

     * For ``recipient_search_method = \Tygh\Enum\RecipientSearchMethods::UGERGROUP_ID``—user group ID.

     * For ``recipient_search_method = \Tygh\Enum\RecipientSearchMethods::EMAIL``—user e-mail.

==========
Transports
==========

Transports handle the actual sending of messages of specific types.

Example of implementation:

* ``\Tygh\Notifications\Transports\MailMailTransport``—sends messages to email, works with the messages that conform with ``\Tygh\Notifications\Transports\Mail\MailMessageSchema``.

* ``\Tygh\Notifications\Transports\InternalTransport``—sends messages to the Notification center, works with the messages that conform with ``\Tygh\Notifications\Transports\Internal\InternalMessageSchema``.

-----------------------------
How to Add Your Own Transport
-----------------------------

The list of identifiers of transports used in the system is available via the ``Tygh::$app['event.transports_schema']`` service.

To add an own transport:

#. Select an identifier with which the transport will be registered in the system (``TransportId``).

#. Create a class that implements the ``\Tygh\Notifications\Transports\ITransport`` interface.

#. Specify the selected identifier in the ``getId()`` method of that class.

#. Register a new provider of that transport in ``Tygh::$app['event.transports.{TransportId}']``.

#. Create a language variable with  ``event.transport.TransportId`` as the identifier, and the name of the transport as a value.

=========
Receivers
=========

Every event has a group of receivers who may be notified about the event. For example, order status change can send notifications to the customer, store administrator, and the vendor from whom the product was bought.

----------------------------
How to Add Your Own Receiver
----------------------------

The list of identifiers of the receivers is available via the ``Tygh::$app['event.receivers_schema']`` service.

To add a new type of receivers:

#. Write a processor of the ``get_notification_rules`` hook and add the receiver's identifier into the ``$force_notification`` array.

#. Create a language variable with ``event.receiver.ReceiverId`` as the identifier, and the name of the receiver type as a value.

#. Add the receivers to the event schema and specify the transports that deliver notifications to these receivers.

=====================
Notification Settings
=====================

By default, it is assumed that if an event is present in ``Tygh::$app['event.events_schema']``, then the event requires notifying all receivers via all transports. This behavior is altered via the notification settings. They describe which receiver must get notifications about events, and via what transport.

.. important::

    Notification settings are specified under **Administration → Notifications** *for the entire system*. Notifications can be configured for every type of receivers, for each event and each transport.

The page for configuring notifications shows only the relevant data. It doesn't show:

* events without receivers;

* receivers not attached to any event;

* transports that don't send events to any receivers.

The changes to the rules are saved in the database in the ``notification_settings`` table; they are available via the ``Tygh::$app['event.notification_settings']`` service.

------------------------------
Overloading Notification Rules
------------------------------

Overloads allow you to prevent sending event notifications to specific receivers, even if notification settings require it.

A set of overloads is an object of the ``\Tygh\Notifications\Settings\Ruleset`` class, and is created by the ``Tygh::$app['event.notification_settings.factory']`` rule factory. A set of overloads is passed as one of the parameters when an event is triggered.

Example: the order editing page has checkboxes "Notify customer", "Notify orders department", and "Notify vendor". They can prevent sending a notification about order status change, even if the notification settings require it.

::

  $notification_rules = Tygh::$app['event.notification_settings.factory']->create([
      UserTypes::CUSTOMER => false,
      UserTypes::ADMIN    => true,
      UserTypes::VENDOR   => true,
  ]);

  Tygh::$app['event.dispatcher']->dispatch(
      'order.updated',
      $order_info,
      $notification_rules
  );

==============================================
How to Start Using the New Notification System
==============================================

#. Find all the places in your add-ons, where emails are sent via the **mailer** service (``Tygh::$app['mailer']->send()``) or the deprecated **\Tygh\Mailer** class (``\Tygh\Mailer::sendMail()``).

#. Create events for these situations; see :ref:`"How to Add Your Own Event" <add-event>`.

#. (optional) Implement an alternative mechanism for informing users via notifications in the Notification center.

#. Replace sending emails with triggering an event via the **event.dispatcher** service: ``Tygh::$app['event.dispatcher']->dispatch()``.
