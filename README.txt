Select or Other Taxonomy Module
Eli Thorkelson / Scholarly Technologies, Univ. of Chicago / Feb 2012
eli@uchicago.edu

-------------------------------------------------------
Overview
-------------------------------------------------------

This module allows you to edit taxonomy term reference fields on Drupal
7 nodes with a select box / checkbox widget that has an Other option.
You can use this Other option to add new values to the taxonomy field in
question. 

This module requires the Select or Other module.

--------------------------------------------------------
Preface
-------------------------------------------------------

This specific version of the module was modified by Jan Bechstein to
allow appropriate display of taxonomies with hierarchy. It can be
found at 
https://github.com/JanBe/select_or_other_taxonomy

Credit for the original code still goes to Eli Thorkelson.

-------------------------------------------------------
Context
-------------------------------------------------------

Drupal 7 ships with two widget types for taxonomy term reference fields.
One is an autocomplete field that allows users to dynamically add new
terms. Another is a select box or checkbox/radio button widget that
allows users to select from an existing set of terms, but not to add new
terms. This widget set is not really optimal, because there are cases
where users should be able to see the current list of options (as in a
select box) but also be able to add new options (as in the
autocomplete). 

There used to be a module for Drupal 6 called Taxonomy Select which
implemented a select box/checkbox widget with an optional Other field,
in which you could enter arbitrary new taxonomy terms. This module has
not been ported to Drupal 7.

http://drupal.org/project/taxonomy_other

There is, fortunately, a new module, Select or Other, which implements a
Drupal Form API form type that allows displaying a list along with an
"other" field in which new values can be added to the list. This module
does not currently support Taxonomy term reference fields, but it turns
out that the code is easy to adapt to support taxonomies. So this new
module serves to glue together the Select or Other module with the
Taxonomy term reference type.

http://drupal.org/project/select_or_other

-------------------------------------------------------
Use
-------------------------------------------------------
Install the module, along with Select or Other, like any other Drupal
module. Once the module is installed, if you edit a content type (Admin
Menu > Structure > Content Types), and add or edit a new Taxonomy Term
Reference field, this module should make available three new widget types:
"Select (or other) list","Select (or other) check boxes/radio
buttons" and "Select (or other) hierarchical list".
Either of these will show users a widget that shows the
existing set of taxonomy terms as selectable options, while also
allowing them to add new taxonomy terms as needed.

-------------------------------------------------------
Testing
-------------------------------------------------------
I've tested the following cases:
-Select box, allow one value
-Select box, allow indefinitely many values
-Radio buttons, allow a fixed number of values
-Radio buttons, allow indefinitely many values

The one vs. many value settings don't have anything to do with the
select vs radio button settings, so this set of test cases should cover
everything.

-------------------------------------------------------
Technical notes
-------------------------------------------------------
This is written as a custom module that uses two Drupal Core hooks:
  * hook_field_widget_info
  * hook_field_widget_form
Documentation for these hooks is here:
  http://api.drupal.org/api/drupal/modules--field--field.api.php/7
  
The basic idea is that we use these hooks to implement a new Drupal
widget type. The first hook registers our widget type with the system,
and specifies the field types we're compatible with (only
taxonomy_term_reference in this case). The second produces Form
API-compatible widget code to actually display our widget. In this
second function, we also register a validation function,
select_or_other_taxonomy_widget_form_validate. This function validates
the submitted data and rearranges it into a type suitable for insertion
in the taxonomy field (creating a new taxonomy term if necessary). 

Most of the work of writing this code involved investigating the
convoluted array structures that Drupal uses to store forms and taxonomy
terms. I ended up needing a couple of utility functions
(select_or_other_taxonomy_get_options,
select_or_other_taxonomy_get_other_value) just to get our data into and
out of the correct formats.
