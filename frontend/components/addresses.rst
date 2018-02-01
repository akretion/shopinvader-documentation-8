.. _addresses:

=========
Addresses
=========
``store.addresses`` provide login user's addresses.

``Address`` object represent an customer address (for shipping or billing) or
user data profile. Some attribute are only available for "profile" type address.

``with_scope``, ``paginate`` liquid tag are available with ``store.addresses``.

A customer have only one "profile" type address.
It isn't possible to update ``address_type`` value or remove address
with "profile" type.

------------------------
Attributes
------------------------

The ``store.addresses`` item has the following attributes:

.. csv-table:: address attributes
  :header: "Name", "Type", "Desciption"
  :widths: 15, 15, 70

  "id", "Integer", "Product's ID"
  "address_type", "String", "profile for user profile data or address for shipping or invoice address"
  "city", "String", "City"
  "country", "Object", "Object with country id and name"
  "display_name", "String", "Address name (firstname and lastname)"
  "is_company", "Boolean", ""
  "name", "String", "Address name"
  "opt_in", "Boolean", ""
  "opt_out", "Boolean", ""
  "phone", "String", "Phone number"
  "ref", "String", ""
  "state", "String", "State ID (US Addresses)"
  "street", "String", "Address"
  "street2", "String", "Address"
  "vat", "String", "VAT identification number (for company profile)"
  "zip", "String", "Zip code"



Example :
  Display available shipping or billing addresses

  .. code-block:: liquid

    {% with_scope  address_type: "address" %}
      {% for address in store.addresses %}
        <address>
          <b>{{address.display_name}}</b><br>
          {{address.street}}<br>
          {{address.street2}}<br>
          {{address.zip}} - {{address.city}}<br>
          {{address.country_id.name}}<br>
        </address>
      {% endfor %}
    {% endwith_scope %}


  Display login user profile data

  .. code-block:: liquid

    {% with_scope  address_type: "profile" %}
      {% assign profile = store.addresses.first %}
      <address>
        <b>{{profile.display_name}}</b><br>
        {{profile.street}}<br>
        {{profile.street2}}<br>
        {{profile.zip}} - {{profile.city}}<br>
        {{profile.country_id.name}}<br>
      </address>
    {% endwith_scope %}


Actions
=======

--------------------------------
Add  shipping or billing address
--------------------------------

*Controller* invader/address/create

HTML form to add address
  .. code-block:: html

    <form method="POST" action="invader/address/create">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />
      <input type="hidden" name="item_id" value="<Cart line ID>" />
      <div>
        <label>
          Name <input type="text" name="name" value="" placeholder="Name"/>
        </label>
      </div>
      <div>
        <label>
          Street <input type="text" name="street" value="" placeholder="Street"/>
        </label>
      </div>
      <div>
        <label>
          Street <input type="text" name="street2" value="" placeholder="Street complement"/>
        </label>
      </div>
      <div>
        <label>
          ZIP Code <input type="text" name="zip" value="" placeholder="ZIP"/>
        </label>
      </div>
      <div>
        <label>
          City <input type="text" name="city" value="" placeholder="City"/>
        </label>
      </div>
      <div>
        <label>
          City
          <select name="country">
          {% for country in store.available_countries %}
            <option value="{{country.id}}">
              {{country.name}}
            </option>
          {% endfor %}
          </select>
        </label>
      </div>
      <div>
        <label>
          Phone <input type="text" name="phone" value="" placeholder="Phone"/>
        </label>
      </div>
      <input type="submit" value="Create"/>
    </form>


--------------------------------
Update address
--------------------------------

*Controller* invader/address/<address id>/update

HTML form to update address
  .. code-block:: html

    <form method="POST" action="invader/address/<address id>/update">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />
      <input type="hidden" name="item_id" value="<Cart line ID>" />
      <div>
        <label>
          Name <input type="text" name="name" value="" placeholder="Name"/>
        </label>
      </div>
      <div>
        <label>
          Street <input type="text" name="street" value="" placeholder="Street"/>
        </label>
      </div>
      <div>
        <label>
          Street <input type="text" name="street2" value="" placeholder="Street complement"/>
        </label>
      </div>
      <div>
        <label>
          ZIP Code <input type="text" name="zip" value="" placeholder="ZIP"/>
        </label>
      </div>
      <div>
        <label>
          City <input type="text" name="city" value="" placeholder="City"/>
        </label>
      </div>
      <div>
        <label>
          City
          <select name="country">
          {% for country in store.available_countries %}
            <option value="{{country.id}}">
              {{country.name}}
            </option>
          {% endfor %}
          </select>
        </label>
      </div>
      <div>
        <label>
          Phone <input type="text" name="phone" value="" placeholder="Phone"/>
        </label>
      </div>
      <input type="submit" value="Update"/>
    </form>

--------------------------------
Remove address
--------------------------------

*Controller* invader/address/<address id>/remove

HTML form to remove address
  .. code-block:: html

    <form method="POST" action="invader/address/<address id>/remove">
      <input type="hidden" name="invader_success_url" value="<URL success page>" />
      <input type="hidden" name="invader_error_url" value="<URL error page>" />
      ...
      <input type="submit" value="Remove"/>
    </form>
