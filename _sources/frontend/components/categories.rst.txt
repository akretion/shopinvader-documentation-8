.. _categories:

==========
Categories
==========


You can get categories via ``store.categories`` global variable.
This variable is a collection of categories stored in Algolia.


---------
Attibutes
---------
.. csv-table:: store.categories item attributes
  :header: "Name", "Type", "Desciption"
  :widths: 15, 15, 70

  "description", "String", "Category's description"
  "childs", "Array", "Sub categories"
  "filters", "Array", "Categories faceting filters, list of product's attribute"
  "level", "String", "category's level in categories tree"
  "id", "Integer", "Category ID"
  "images", "Array", "URL image, 4 image sizes are available for earch item (small, large, medium, original)"
  "objectID", "Integer", "Category ID"
  "meta_description", "String", "meta description tag value."
  "meta_keywords", "String", "meta keyword tag value."
  "name", "String", "Category name"
  "parent", "Array", "List of parent categories "
  "redirect_url_key", "Array", "List URL redirection (HTTP status 301)"
  "seo_title", "String", "Category page Title , title tag value"
  "short_description", "String", ""
	"subtitle", "String", "Category short description"
	"url_key", "String", "Category relative URL"


Example :
  Display categories list.

  .. code-block:: liquid

    <ul>
      {% for category in store.categories %}
        <li>
          <a href="{% path_to category %}">
            {{category.name}}
          </a>
        </li>
      {% endfor %}
    </ul>
