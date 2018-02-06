.. _store_notification:

Notifications
==================

``store.notifications`` is a collection with error and  warning message provide
by controllers. This variable is automatically set up by shopinvader to provide
error message after form submiting on a shopinvader controller.

Shopinvader clean up this collection each page call. Each ``store.notifications``
entries have ``type`` and ``message`` attributes.

Example :
  JSON price data

  .. code-block:: json

    [
      {
        "type":"danger",
        "message":"Invalid Form"
      }
    ]


Display notificaton
  .. code-block:: liquid

    {% if store.notifications != null %}
      {% for notification in store.notifications%}
        <div class="alert alert-{{notification.type}}">
        {{notification.message}}
        </div>
      {% endfor%}
    {% endif%}
