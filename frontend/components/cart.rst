====
Cart
====
store.cart provides you the current cart. this variable is only available after add to cart action.
store.cart allow cart lines with product data, shipping address, delivery mode,
Carts are store in odoo database. To avoid performance problems, cart data are cached, each cart updating (add new product, set a shipping address…) refresh cached datas via an Odoo API call.
Each cart update recalculate grand total, subtotal, shipping cost and VAT amount automatically.

Attributes
==========

-----------
Cart header
-----------

.. csv-table:: store.cart attributes
  :header: "Name", "Type", "Desciption"
  :widths: 15, 15, 70

  "anonymous_token",	"String", "*Deprecated* Token for anonymous customer"
  "anonymous_email",	"String", "*Deprecated* Email anonymous customer"
  "amount_tax",	"Float", "VAT grand total (Item and shipping cost included)"
  "amount_total",	"Float", "Grand total"
  "amount_untaxed",	"Float", "Total without taxes"
  "available_carriers", "Array", "Collection of carrier *See carrier documentation below*"
  "available_payment_method", "Array", "Collection of payment method ref:`cart_payment_method` documentation"
  "carrier", "Object", "Carrier selected"
  "current_step", "String", "Cart Step, *See cart step documentation below*"
  "date_order", "Date", "The creation cart date"
  "done_steps", "Array", "The list of step previously done  *See cart step documentation below*"
  "id", "Integer", "Cart ID"
  "item_amount_tax", "Float", "VAT amout of Item subtotal (sum of product's VAT)"
  "item_amount_total", "Float", "Product subtotal  (sum of product's total without shippment)"
  "item_amount_untaxed", "Float", "Product subtotal without VAT"
  "item_number", "Integer", "Sum of all product's quantities"
  "name", "String", "Cart reference"
  "order_line", "Array", "Cart lines See :ref:`cart_lines` documentation"
  "partner", "Object", "Cart's customer"
  "partner_invoice", "Object", "Invoice address"
  "partner_shipping", "Object", "Shipping address"
  "payment_method",  "Object", "Payment method selected"
  "state",  "String", "Odoo sale order status"
  "shipping_amount_tax",  "Float", "Shipping VAT amout"
  "shipping_amount_untaxed",  "Float", "Shipping cost without VAT"
  "shipping_amount_total",  "Float", "Shipping cost with VAT included"
  "trackings", "Array", "Parcel tracking URLs, not available for cart."
  "use_different_invoice_address", "Boolean", "Set True if shipping and invoice address are not the same"


Actions
==========

Some actions are available to manage cart content. Cart actions use html form.
Each action need form submitting with specific parameters.

Usally, we use hidden input to specify these parameters values.

You have to use get or post for form method. All actions require
``action_proxy`` and ``action_method``.

``action_proxy`` is use to specify Shopinvader controller
(cart, cart/item, addresses...)

``action_method`` is use to specify method apply to  Shopinvader controller

action_method html variable is use
to specify method on the shopinvader controller in the same way as REST API.

.. note::
  Due to Chrome restriction on form method authorized, Shopinvader html form
  can't use form attribute method to specify ``DELETE``, ``POST``, ``GET``,
  ``PUT`` method use on shopinvader controller, ``action_method``
  replace form method role.

-------------------
Checkout process
-------------------
Actually Shopinvader use muli-page checkout.
To move from one step to the next you need to submit a HTML form with specific
parameters to indicate the current and the next checkout step.

This process is necessary  to ensure the cart data viability.

Usally we recommended these cart steps :

  * Cart summary
  * Customer login
  * Select shipping address
  * Select Shipping mode and payment
  * order summary

You have to set up your cart steps into odoo backend configuration.

.. csv-table:: HTML form input
  :header: "Input name", "Value", "Desciption"
  :widths: 15, 15, 70

  "action_proxy", "cart", "Shopinvader cart controller name"
  "action_method", "put", "Method on controller"
  "current_step", "", "current step ID"
  "next_step", "", "next step ID"

You can submit other cart controller parameters like shipping address, shipping
method in the same time.


HTML form to go to the next step
  .. code-block:: html

    <form method="post" action="invader/cart/update">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />
      <input type="hidden" name="current_step" value="cart_index">
      <input type="hidden" name="next_step" value="cart_login">
      <input type="submit" value="Update"/>
    </form>




------------------------------------
Select Shipping and billing address
------------------------------------

In your cart a customer can enter two addresses: billing address or shipping
address.

shipping and billing address are an item of ``store.addresses``. ``store.addresses``
collection (: :ref:`see store.addresses documentation <addresses>`).

*Controller* invader/cart/update

HTML form to choose shipping
  .. code-block:: html

    <form method="post" action="invader/cart/update">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />
      <div>
        <h2>Select shipping address</h2>
        {% with_scope  address_type: "address" %}
          {% for address in store.addresses %}
            <div>
              <input type="radio" name="partner_shipping" value="{{address.id}}"/>
              {{address.display_name}}
            </div>
          {% endfor %}
        {% endwith_scope %}
      </div>
      <div>
        <h2>Select billing address</h2>
        {% with_scope  address_type: "address" %}
          {% for address in store.addresses %}
            <div>
              <input type="radio" name="partner_invoice" value="{{address.id}}"/>
              {{address.display_name}}
            </div>
          {% endfor %}
        {% endwith_scope %}
      </div>
      <input type="submit" value="Update"/>
    </form>



------------------------
Select shipping method
------------------------

*Controller* invader/cart/update

``store.cart.available_carriers`` provide carrier list.

HTML form to go to the next step
  .. code-block:: html

    <form method="post" action="invader/cart/update">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />

      {%for carrier in store.cart.available_carriers%}
        <div>
          <input type="radio" name="carrier" value="{{carrier.id}}"/>
          <b>{{carrier.name}}</b>
        </div>
      {% endfor %}

      <input type="submit" value="Select carrier"/>
    </form>


.. _cart_payment_method:
------------------------
Select payment method
------------------------

*Controller* invader/cart/update

``store.cart.available_payment_method`` provide carrier list.

HTML form to go to the next step
  .. code-block:: html

    <form method="post" action="invader/cart/update">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />

      {%for payment_method in store.cart.available_payment_method%}
        <div>
          <input type="radio" name="payment_method" value="{{payment_method.id}}"/>
          <b>{{payment_method.name}}</b>
        </div>
      {% endfor %}

      <input type="submit" value="Select payment method"/>
    </form>



.. _cart_lines:



==========
Cart item
==========


Attributes
===========

An order_line represents a single line in the shopping cart.
This object can be accessed in all Liquid templates via ``store.cart.order_line``.
There is one cart line for each product variant in the shopping cart.

Cart and cart lines are stored directly in odoo.

This object has the following attributes:

.. csv-table:: store.cart.order_line attributes
  :header: "Name", "Type", "Desciption"
  :widths: 15, 15, 70

  "discount", "Float", "Discount rate"
  "id", "Integer", "cart line ID (used for update and delete lines)"
  "is_delivery", "Boolean", "product already delivered. not for cart"
  "product", "Object", "Product object with id, image, url key, *see exemple below*"
  "product_uom_qty", "Integer", "Product quantity"
  "price_unit", "Float", "Unit product price (without discount)"
  "price_subtotal", "Float", "total of cart line without taxes"
  "price_subtotal_gross", "Float", "total of cart line taxes included"


*Cart line json*

  .. code-block:: json

      {
        "order_line":[
          {
            "product":{
              "images":[
                {"small":"<URL image>" ,"large":"<URL image>","medium":"<URL image>","original":"<URL image>"},
                {"small":"<URL image>" ,"large":"<URL image>","medium":"<URL image>","original":"<URL image>"}
              ],
              "default_code":"SKUPRODUCT",
              "url_key":"my-product-url",
              "name":"Great product",
              "id": 208
            },
            "price_unit":60,
            "product_uom_qty":2.0,
            "price_subtotal":100,
            "discount":0,"price_subtotal_gross":120,
            "is_delivery":false,
            "id": 3008
          }
        ]
      }

*Display lines in template*

  .. code-block:: liquid

      <table>
      {% for line in store.cart.lines %}
        <tr>
          <td>{{line.product.name}}</td>
          <td>{{line.product_uom_qty}}</td>
          <td>{{line.price_subtotal | money}}</td>
        </tr>
      {% endfor %}
      </table>


Actions
==========
-------------------
Add product to cart
-------------------

*Controller* invader/cart/add_item

.. csv-table:: HTML form input
  :header: "Input name", "Value", "Desciption"
  :widths: 15, 15, 70

  "invader_success_url", "string", "URL in case of success"
  "invader_error_url", "string", "URL in case of error"
  "product_id", "integer", "``ObjectID`` of product added to cart "
  "item_qty", "integer", "Product quantity added to cart"


HTML form to add product in cart
  .. code-block:: html

    <form method="POST" action="invader/cart/add_item">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />
      <input type="hidden" name="product_id" value="<product.ObjectID>" />
      <input type="text" name="item_qty" value="1" />
      ...
      <input type="submit" value="Add product"/>
    </form>

-------------------
Update quantity
-------------------
This action is usally used in cart item list.

*Controller* invader/cart/update_item

.. csv-table:: HTML form input
  :header: "Input name", "Value", "Desciption"
  :widths: 15, 15, 70

  "invader_success_url", "string", "URL in case of success"
  "invader_error_url", "string", "URL in case of error"
  "item_id", "integer", "cart line ID"
  "item_qty", "integer", "Product quantity"


HTML form to change product quantity
  .. code-block:: html

    <form method="POST" action="invader/cart/update_item">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />
      <input type="hidden" name="item_id" value="<Cart line ID>" />
      <input type="text" name="item_qty" value="1" />
      ...
      <input type="submit" value="Change quantity"/>
    </form>

------------------------
Remove product to cart
------------------------

.. csv-table:: HTML form input
  :header: "Input name", "Value", "Desciption"
  :widths: 15, 15, 70

  "invader_success_url", "string", "URL in case of success"
  "invader_error_url", "string", "URL in case of error"
  "item_id", "integer", "cart line ID"


HTML form to remove cart line
  .. code-block:: html

    <form method="POST" action="invader/cart/delete_item">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />
      <input type="hidden" name="item_id" value="<Cart line ID>" />
      ...
      <input type="submit" value="Remove line"/>
    </form>
