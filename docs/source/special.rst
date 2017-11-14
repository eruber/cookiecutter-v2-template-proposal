.. ###########################################################################
   This file contains reStructuredText, please do not edit it unless you are
   familar with reStructuredText markup as well as Sphinx specific markup.

   For information regarding reStructuredText markup see
      http://sphinx.pocoo.org/rest.html

   For information regarding Sphinx specific markup see
      http://sphinx.pocoo.org/markup/index.html

.. ###########################################################################

   Copyright (c) 2017, E.R. Uber

   Authors: E.R. Uber (eruber@gmail.com), Raphael Pierzina (raphael@hackebrot.de)

   License: Apache Software License 2.0 - See LICENSE file in project root

.. ########################## SECTION HEADING REMINDER #######################
   # with overline, for parts
   * with overline, for chapters
   =, for sections
   -, for subsections
   ^, for subsubsections
   ", for paragraphs

.. ---------------------------------------------------------------------------

**************************************
Extra Context Overwrite Considerations
**************************************
This section identifies further functionality in the Cookiecutter reference
implemenation because the new template format requires new solutions in the
area of context overwriting.

Context overwriting occurs when the [EXTRA_CONTEXT] is
specified on the command line as explained in the Cookiecutter docs
section `Injecting Extra Context`_.

.. note:: **eruber**:
          Note that because the new template's jinja2 context is an array of
          OrderedDict elements -- one for each variable in the jinja2 context;
          the EXTRA_CONTEXT specified by the user, must also be an array of
          OrderedDict elements -- one for each variable that the EXTRA_CONTEXT
          wishes to overwrite.


Overwrite Considerations Regarding 'default' & 'choices' Fields
===============================================================
When a variable is defined that has both the **default** and the **choices** fields,
these two fields influence each other. If one of these fields is updated, but
not the other field, then the other field will be automatically updated by the
overwrite logic.

If both fields are updated, then the **default** value will be moved to the first
location of the **choices** field if it exists elsewhere in the list; if the **default**
value is not in the list, it will be added to the first location in the **choices**
list. The overwrite logic will take care of this even though the **extra context**
**choices** list does not explicitly specify this behavior.


Use Case #1 - Update 'default' field - 'choices' gets updated
-------------------------------------------------------------
For example, if **default** and **choices** fields of a **variable** named
"director_name" look like this::

   {
      "name": "director_name",
      ...
      "default": "Allan Smithe",
      "choices": ["Allan Smithe", "Ridley Scott", "Victor Fleming", "John Ford", "John Houston"],
      '''
   }

and the **extra context** dictionary specified by the user looks like this
(injecting an update to the **default** field)::

   'extra_context': [
       {
           'name': 'director_name',
           'default': 'John Ford',
       }
   ]

then the overwrite logic will leave the fields looking like this::

   {
      "name": "director_name",
      ...
      "default": "John Ford",
      "choices", ["John Ford", "Allan Smithe", "Ridley Scott", "Victor Fleming", "John Houston"],
      ...
   }


Use Case #2 - Update 'choices' field - 'default' field gets updated
-------------------------------------------------------------------
For example, if **default** and **choices** fields of a **variable** named
"director_name" look like this::

   {
      "name": "director_name",
      ...
      "default": "Allan Smithe",
      "choices": ["Allan Smithe", "Ridley Scott", "Victor Fleming", "John Ford", "John Houston"],
      ...
   }

and the **extra context** dictionary looks like this (injecting an update to
the **choices** field)::

   'extra_context': [
       {
           'name': 'director_name',
           'choices': ['Ridley Scott', 'Allan Smithe', 'Victor Fleming', 'John Ford', 'John Houston'],
       }
   ]

then the overwrite logic will leave the fields looking like this::

   {
      "name": "director_name",
      ...
      "default": "Ridley Scott",
      "choices": ["Ridley Scott", "Allan Smithe", "Victor Fleming", "John Ford", "John Houston"],
      ...
   }


Use Case #3 - Update both 'choices' & 'default' fields
------------------------------------------------------
For example, if **default** and **choices** fields of a **variable** named
"director_name" look like this::

   {
      "name": "director_name",
      ...
      "default": "Allan Smithe",
      "choices": ["Allan Smithe", "Ridley Scott", "Victor Fleming", "John Ford", "John Houston"],
      ...
   }

and the **extra context** looks like this (injecting updates to both the
**default** and the **choices** fields)::

   'extra_context': [
       {
           'name': 'director_name',
           'default': 'Victor Fleming',
           'choices': ['Ridley Scott', 'Allan Smithe', 'Victor Fleming', 'John Ford', 'John Houston'],
       }
   ]

then the overwrite logic will leave the **choices** and **default**
fields updated as follows::

   {
      "name": "director_name",
      ...
      "default": "Victor Fleming",
      "choices": ["Victor Fleming", "Allan Smithe", "Ridley Scott", "John Ford", "John Houston"],
      ...
   }

Use Case #4 - Update 'default' field, but its not in the 'choices' list
-----------------------------------------------------------------------
For example, if **default** and **choices** fields of a **variable** named
"director_name" look like this::

   {
      "name": "director_name",
      ...
      "default": "Allan Smithe",
      "choices": ["Allan Smithe", "Ridley Scott", "Victor Fleming", "John Ford", "John Houston"],
      ...
   }

and the **extra context** looks like this (injecting a director name that is
not in the **choices** list)::

   'extra_context': [
       {
           'name': 'director_name',
           'default': 'Otto Preminger',
       }
   ]

then the overwrite logic will leave the **choices** and **default**
fields updated as follows::

   {
      "name": "director_name",
      ...
      "default": "Otto Preminger",
      "choices": ["Otto Preminger", "Allan Smithe", "Ridley Scott", "Victor Fleming", "John Ford", "John Houston"],
      ...
   }


Special Overwrite Syntax for Renaming a Variable
================================================
Because the algorithm chosen to find a variable's dictionary entry (in the
variables list of OrderDicts) uses the variable's 'name' field; it could not
be used to simultaneously hold a new 'name' field value.

Therefore the following **extra context** dictionary entry snytax was introduced
to allow the 'name' field of a variable to be changed::

   {
      'name': 'CURRENT_VARIABLE_NAME::NEW_VARIABLE_NAME',
   }

The variable's current name is post-fixed with a double colon (::) followed by
the new name of the variable.

For example, to change a variable's 'name' field from
'director_credit' to 'producer_credit', would require::

   {
      'name': 'director_credit::producer_credit',
   }

The overwrite logic also takes care of updating in other references to the
variable's name that might exists elsewhere in the variable -- for example,
if the variable's name were used in an a **skip_if** field.


Special Overwrite Syntax for Removing a Field from a Variable
=============================================================

It is possible that a previous **extra context** overwrite requires that a
subsequent variable field be removed.

In order to accomplish this a **remove field token** is used in the
**extra context** as follows::

   {
      'name': 'director_cut',
      'skip_if': '<<REMOVE::FIELD>>',
   }

In the example above, the **extra context** overwrite results in the variable
named 'director_cut' having it's 'skip_if' field removed.

Of course the **name** field and the **default** field cannot be removed from
a variable, their existence is mandatory. Any attempt to remove one of these
fields will result in an exception.



.. _User Config: http://cookiecutter.readthedocs.io/en/latest/advanced/user_config.html
.. _Injecting Extra Context: http://cookiecutter.readthedocs.io/en/latest/advanced/injecting_context.html
