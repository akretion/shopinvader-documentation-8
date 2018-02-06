===================
What is Shopinvader
===================

Shopinvader is an open-source e-commerce platform  based on Odoo and `LocomotiveCMS <https://www.locomotivecms.com/>`_
Shopinvader provide Odoo’s data (products, categories, customer, cart...) directly into your frontend on Locomotive CMS.


--------------
LocomotiveCMS
--------------

Locomotive is an Open Source CMS that makes it super easy to develop and design exactly what your clients need.
LocomotiveCMS 3 is used for frontend part and manage website content.

.. note::
  Need more information about LocomotiveCMS ?
   `See the documentation <https://locomotive-v3.readme.io/docs>`_.

-------
Algolia
-------
Shopinvader use Algolia to store some data.

An Odoo module push JSON product data for product linked to backend in Algolia
database thanks to API HTTP request.
Algolia is a powerful third party service used to store and manage JSON product data, this services provide a good
faceting service and a powerful search engine. This solution helps you to
establish quickly a user friendly search engine to improve customer
user experience.

With shopinvader, Algolia assumed to role of product’s database and search engine.
All products are available via the store.products variable. Each store.product get make a algolia API request to get product data.
Paginate and with_scope LocomotiveCMS’s tags can be use to paginate result or filtering product.


************
Features
************

Sales
=====
store.sales contains a collections of sale order to get previous sales order of
a logged customer. You must have an active customer session to read store.sales.

This variable is commonely used for customer account pages to provide order
status, parcel tracking, claims....

Attibutes
---------
.. csv-table:: store.products attributes
  :header: "Name", "Type", "Desciption"
  :widths: 15, 15, 70

  "objectID", "Integer", "Product's ID"
