.. _products:

========
Products
========

Products are saleable item displayed in your shop.


Products can be accessed in all Liquid templates via
``store.products.store.products`` represent a collection of all products
variants avalaible.

This variable is a collection of product stored in Algolia.

``with_scope``, ``paginate`` liquid tag are available with ``store.products``.

.. note::

  In these examples below, ``product`` variable is an occurence
  of ``store.products`` collection.


------------------------
Attributes
------------------------

The ``product`` object has the following attributes:

.. csv-table:: store.products attributes
  :header: "Name", "Type", "Desciption"
  :widths: 15, 15, 70

  "attributes", "Array", "Collection of variant attribute"
  "categories", "Array", "Collection of parents categories"
  "cross_sellings", "Array", "Collection of product ObjectID for cross selling ex: [{id:60}, {id: 78}]."
  "default_code", "string", "Odoo product Internal Reference"
  "description", "String", "Product HTML description"
  "hierarchicalCategories", "Array", "Parent categories list for faceting ex: {lvl0:..., lvl1:...}"
  "id", "Integer", "..."
  "images", "Array", "Collection of URL images, 4 image sizes are available for earch item (small, large, medium, original)"
  "main", "Boolean", "Main variant"
  "medias", "Array", "List of URL for products documentations files"
  "meta_description", "String", "meta description tag value."
  "meta_keywords", "String", "meta keyword tag value."
  "model_name", "String", "Product template name"
  "objectID", "Integer", "Product's ID"
  "price", "Array", "Price. *See price documentation below*"
  "qty_available", "Integer", "Stock quantity saleable"
  "redirect_url_key", "Array", "List URL redirection (HTTP status 301)"
  "rating", "Array", "Rating, *See rating documentation*"
  "relateds", "Array", "Product relateds"
  "seo_title", "String", "Product page Title , title tag value"
  "stock_state", "String", "product stock status"
  "tags", "Array", "Product tag specification"
  "up_sellings", "Array", "Collection of product ObjectID for up selling ex: [{id:60}, {id: 78}]."
  "url_key", "String", "product relative URL"
  "variant_count", "Integer", "number of variant"



Example :
  Display product name list.

  .. code-block:: liquid

    {% for product in store.products %}
      {{product.model_name}}
    {% endfor %}

.. _products_image:


Images
------

Shopinvader Odoo module generate images with 4 differents sizes (small, medium, large, original).
Image attribute of product's data is an array containing all product's images URL with 4 sizes.
Original size is the size of image provided by the user in Odoo.

Example :
  Display medium size image product.


  .. code-block:: liquid

    {% for image in product %}
      <img src={{image.medium}} />
    {% endfor %}


.. _products_price:


Price
-----

Price is an attribute of product objet. Price attribute contains some product
price list. Shopinvader  use the Locomotive CMS parameter
site.metafields.erp.default_price value to get the default pricelist.

Price product's attribute contains automatically the value of the current pricelist.

In case of logged user, Shopinvader get the user price list
(pricelist field of customer content type).


.. csv-table:: Product's price item attributes
  :header: "Name", "Type", "Desciption"
  :widths: 15, 15, 70

  "discount", "Float", "Discount value (Percentage)"
  "original_value", "Float", "Old price value without discount"
  "tax_included", "Boolean", "True => VAT included"
  "value", "Float", "Price value"

Example :
  JSON price data

  .. code-block:: json

    {
      "price": {
        "public_tax_exc": {
          "discount": 20,
          "tax_included": false,
          "value": 1530.37,
          "original_value": 1912.96
        },
        "public_tax_inc": {
          "discount": 20,
          "tax_included": true,
          "value": 1759.92,
          "original_value": 2199.9
        }
      }
    }

  Display product price

  .. code-block:: liquid

    {{product.price.value | money}}


.. _products_rating:


Rating
------
Rating attribute contains reviews list and a summary to get average rating.


Summary
~~~~~~~

  Use to get average rating value.

  .. csv-table:: rating.summary attributes
    :header: "Name", "Type", "Desciption"
    :widths: 15, 15, 70

    "count", "Integer", "Number of rate"
    "average", "Float", "Average rate value"
    "distribution", "Array", "Get rate number for each rate step"

  Example
    JSON summary rating data (extract of product object)

    .. code-block:: json

        {
          "summary": {
            "count": 9,
            "average": 4,
            "distribution": {
              "1": 0,
              "2": 0,
              "3": 1,
              "4": 1,
              "5": 5
            }
          }
        }

    Display rating

    .. code-block:: liquid

      {{product.rating.summary.average}}


Review
~~~~~~~

  Review object is an occurrence of ``product.rating.reviews`` array.

  This object represent a customer review.

  .. csv-table:: Product's price item attributes
    :header: "Name", "Type", "Desciption"
    :widths: 15, 15, 70

    "reviews.comment", "String", "meta description tag value."
    "reviews.rating", "String", "meta keyword tag value."

  Example
      JSON reviews data (extract of product object)

    .. code-block:: json

        {
          "reviews": [
            {
              "comment": "Comment text",
              "rating": 4,
              "product_code": false,
              "nickname": "Customer name",
              "name": "Customer name"
            },
            {
              "comment": "Comment text",
              "rating": 4,
              "product_code": false,
              "nickname": "Customer name",
              "name": "Customer name"
            }
          ],
        }
