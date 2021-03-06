Changelog
---------

2.4.0 (2020-11-04)
~~~~~~~~~~~~~~~~~~

* The given `rows` queryset and filtering were not respected for the "Select all rows" bulk feature. This could produce some pretty bad bugs!

* Support custom bulk post_handlers on lists and not just querysets

* `Table` has a few new members:
    - `initial_rows`: the rows you pass (or that gets created by `auto__model`) is stored unchanged here
    - `sorted_rows`: `initial_rows` + sorting applied
    - `sorted_and_filtered_rows`: `sorted_rows` + filtering applied
    - `visible_rows`: `sorted_and_filtered_rows` + pagination applied
    - `rows`: this is now a property and will map to the old behavior which is the "most applied" member that exists


* Fixed passing dunder paths to `auto__include`. You got a weird crash if the target of the path was a foreign key. There are still issues to be resolved adjacent to this, but the base case now works.

* Fixed the "select all" feature for pages with multiple tables.


2.3.0 (2020-10-30)
~~~~~~~~~~~~~~~~~~

* Every part can now have assets that are added to the assets of the style and included in the head. This is particularly useful for bundling small pieces of javascript or css with the components that need them and thereby gets us closer to being able to write truly self contained "component".  As a proof of concept I did so for the tables javascript parts. The naming takes care of deduplication of assets.

* Only include select2 assets when needed (possible because of the point above)

* Filtering on booleans was very broken. It always returned empty querysets and didn't produce errors when you tried to do stuff like `my_boolean<3`

* It's now possible to configure stuff on the freetext field of a query

* iommi will now grab the root page title from the text from `Header` instances in addition to `Part.title`

* Render date fields as such

* Fixed date and time formatting

* Support for optgroups in forms

* Make it possible to insert fields into the form of a query, and filters into a query

* Differentiate between primary and other actions. This should make iommi pages look more in line with the majority of design systems. If you have a custom style you probably want to add a style definition for `Action.primary`.

* Fixed a case of a silent overwrite that could be surprising. This was found during reading the code and has never happened to us in practice.

* Style fixes for bulma


2.2.0 (2020-10-16)
~~~~~~~~~~~~~~~~~~

* Fix so that style application does not alter definitions destructively. This could lead to some strange behavior if you tried to switch between styles, and it could leak over definitions between things you would not expect.

* The title of `Table` is `None` when there is no model

* Assets as first class concept. You can now insert asset definitions into your style with `assets__js=...` instead of defining a `base_template`. This change also removes the base templates for all the built in styles as they are now obsolete.

* Made it easy to hide the label of a Field by setting `display_name=None`, or `include=False`


2.1.0 (2020-10-07)
~~~~~~~~~~~~~~~~~~

* Internationalization! iommi now has i18n support and ships with English, German and Swedish languages out of the box. We welcome more translations.

* Out of the box support for the Bulma CSS framework

* Make `auto__include` specifications allow foreign key paths

* By default we now grab display_name from the model fields verbose_name (if applicable)

* Sometimes you got reordering of parts when doing a post to a form for example, this is now fixed

* The `traversable` argument to lambdas is now the leaf and not the root. This was a bug.

* Support `reverse_lazy` as url argument to MenuItem

* Two id attributes were rendered on the input tags in forms (thanks Benedikt Grundmann for reporting!)


2.0.1 (2020-09-22)
~~~~~~~~~~~~~~~~~~

* `delete_object__post_handler` accessed `instance.id` which might be valid. It should have accessed `instance.pk` which is always valid.


2.0.0 (2020-09-22)
~~~~~~~~~~~~~~~~~~

* BACKWARDS INCOMPATIBLE: `Style` must now take a `base_template` argument. This replaces the setting `IOMMI_BASE_TEMPLATE`.

* BACKWARDS INCOMPATIBLE: `IOMMI_CONTENT_BLOCK` is removed. Replaced by the `content_block` setting for `Style`.

* Allow table rows to be provided from a generator. (Disabling paginator)

* Added blocks (`iommi_head_contents`, `iommi_top`, and `iommi_bottom`) as useful hook points to add custom data in the templates if you don't need a totally new template but want to just customize a little bit.

* The default sort_key on a Column.foreign_key now looks at the searchable field of the remote field ('name' by default). This means by default sorting will mostly be more what you expect.

* Changed the error from get_search_field() for non-unique name to a warning.

* Removed <table> for layout in query advanced/simple stuff.

* Don't warn for missing register_search_fields when attr=None

* Set admin to bootstrap by default.

* Added form for changing password. Used by the admin but also usable from your code.

* Added form for login. Used by the admin but also usable from your code.

* Fixed foundation styling for query form.

* Introduced `Field.help`. This is the fragment that renders the help text for a `Field`. This means you can now style and customize this part of forms more easily. For example set a CSS class: `Field(help__attrs__class__foo='foo'`.

* Use django default date and time formatting in tables.

* New shortcut for `Table`: `Table.div` for when you want to render a `Table` as a bunch of divs. This is useful because a `Table` is really a view model of a sequence of stuff, not just a `<table>`.

* Possibility to set `Actions.tag` to `None` to not get a wrapping html tag.

* Added `Table.outer` as a tag you can style that encompasses the entire table part.

* Moved `Form.h_tag` rendering inside the form tag to make it stylable as a coherent whole.

* Grab html title from first part if no title is given explicitly. This means you'll get the `<title>` tag filled more often by what you expect automatically.

* `Template` instances are now collected properly by `Part`.

* Read admin config from modules.

* The Admin is now opt in, not opt out.

* The admin is now MUCH prettier and better.

* Actions for `Table` are now rendered above the table by default. Set `actions_below` to `True` to render them the old way.

* Many misc improvements


1.0.3 (2020-08-24)
~~~~~~~~~~~~~~~~~~

* Changed `Table.bulk_form` to `Table.bulk`. The old name was a mistake as the name was always `bulk`. This meant that styling didn't work like you expected and the pick feature also lead you down the wrong path.


1.0.2 (2020-08-21)
~~~~~~~~~~~~~~~~~~

* Support user inputted relative dates/datetimes

* Support more time formats automatically

* Introduced Filter.parse() which is a hook point for handling special parsing in the query language. The query language will no longer try to convert to integers, floats and dates for you. You have to specify a parse() method.

* Added `traversable` key to evaluate parameters. Think of it like something similar to `self`.

* `cell__format` now gets all evaluate parameters like you'd expect

* Filters: If `attr` is `None` but you've specified `value_to_q` then your filter is now included

* Various bug fixes


1.0.1 (2020-06-24)
~~~~~~~~~~~~~~~~~~

* Optimizations

* Use select2 as the default for multi_choice

* Improved usability: Make icon column behavior on falsy values more guessable

* Accidentally changed default style to foundation, change back to bootstrap

* Improved usability: Don't fall back to default template name if the user specified an explicit template name: fail on TemplateNotFound

* Style on root uses correct base template

* Allow model fields called `context`


1.0.0 (2020-06-10)
~~~~~~~~~~~~~~~~~~

* Backwards incompatible: `register_search_fields` replaces `register_name_field`. This new system is a list of field names and not just a single field. There is also new searching and filtering behavior based on this that means you will get better search results

* Backwards incompatible: `field_name` as used by model factories is replaced with `model_field_name`. If you used `register_factory` you will need to change this. The field names on `Column`, `Field` and `Filter` are also renamed.

* Support fields named `keys`, `value` or `items` on Django models

* Added basic styling support for CSS frameworks Water and Foundation

* Fix include to make None mean False

* Change Filter.text to search using icontains instead of iexact by default in the basic search mode

* Change post_validation callback to receive standard evaluate parameters

* Improved help text for queries

* Field.radio was broken in the bootstrap style: it specified the input template as the template for the entire field, so the label got erased


0.7.0 (2020-05-22)
~~~~~~~~~~~~~~~~~~

* Fixed default text argument to Fragment

* Fixed issue where endpoint dispatch parameter was left over in the pagination and sorting links

* Parts that are None should not be collected. This affected the admin where it printed "None" below the "Admin" link.

* Added header for bulk edit form in tables

* Fixed textarea readonly when field is not editable

* Fixed is_paginated function on Paginator

* Add request to evaluate parameters

* Make evaluate and evaluate_recursive match even the `**_` case by default

* No dispatch command on a POST is invalid and will now produce an error

* Lazy bind() on members. This is a performance fix.

* Fixed bug where display_name could not be overridden with a lambda due to incorrect evaluate handling

* Removed Table.rendered_columns container. You have to look at the columns and check if they have render_column=False


0.6.2 (2020-04-22)
~~~~~~~~~~~~~~~~~~

* Fixed data-endpoint attribute on table


0.6.1 (2020-04-21)
~~~~~~~~~~~~~~~~~~

* Fixed tbody endpoint and added a div to make the endpoint easier to use


0.6.0 (2020-04-17)
~~~~~~~~~~~~~~~~~~

* Fixed an issue where fragments couldn't be customized later if built with the `html` builder

* `Action` inherits from `Fragment`. This should be mostly transparent.

* You can now pass multiple argument to `Fragment`/`html.foo()`. So `html.div('foo', 'bar')` is now valid and creates two child nodes `child0` and `child1`

* Uncouple `auto__*` from `row` parameter. `auto__` only suggests a default. This avoids some confusion one could get if mixing `auto__rows`, `auto__models` and `rows` in some ways.

* Fixed setting active on nested submenus where the parent had url None


0.5.0 (2020-04-01)
~~~~~~~~~~~~~~~~~~

* Include iommi/base_bootstrap.html and iommi/base_semantic_ui.html in package, and use them if no base.html is present. This improves the out of the box experience for new projects a lot

* Support mixing of `auto__model`/`auto__row` based columns and declarative columns

* Support attrs__class and attrs__style as callables

* Added support for context namespace on Page, which is passed to the template when rendering (for now only available on the root page)

* Fixed how we set title of bulk edit and delete buttons to make configuration more obvious


0.4.0 (2020-03-30)
~~~~~~~~~~~~~~~~~~

* Fixed rendering of grouped actions for bootstrap

* Respect auto__include order

* boolean_tristate should be the default for the Field of a Column.boolean

* New class Header that is used to automatically get h1/h2/etc tags according to nesting of headers

* Table.rows should be able to be evaluated

* Added feature that you can type 'now' into date/datetime/time fields

* Feature to be able to force rendering of paginator for single page tables

* Paginator fixes: it's now no longer possible to use the Django paginator, but the iommi paginator is more full features in trade.

* Removed jQuery dependency for JS parts

* Big improvements to the Menu component

* filters that have freetext mode now hide their field by default

* Added "pick" in the debug toolbar. This is a feature to quickly find the part of the document you want to configure

* Introduced Form.choice_queryset.extra.create_q_from_value

* Changed so that Query defaults to having the Field included by default

* Renamed BoundRow/bound_row to Cells/cells

* Major improvements to the admin

* Lots and lots of cleanup and bug fixes
