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

***************
Format Proposal
***************

The Cookiecutter version 1 template is a simple JSON file defining a dictionary
of key/value pairs that identify variables used in a jinja2 context.

The version 2 template adds additional template **metadata** that exists outside
the jinja2 context, but is expected to support features in the future that
will enhance the overall user experience.

Required Metadata Template Fields
=================================
Currently the minimum number of required metadata fields is three:

* **name**
* **cookiecutter_version**
* **variables**

name Field
----------
The **name** field is a string identifying the name of the template.

For example::

   "name": "the-most-famous-template-of-all",

.. note:: **hackebrot**:
          We can use this for dumping the JSON context for --replay. Currently we
          make a good guess based on the directory name of the cloned git repository,
          which is not great for local templates or relative paths. This could be
          something like an ID.


cookiecutter_version Field
--------------------------
The **cookiecutter_version** field is a string identifying version information

The current implementation assumes this field identifies the minimum version
of Cookiecutter required to process the template, as in::

   "cookiecutter_version": "2.0.0",

There are other meanings of this field to consider, see the note below.

.. note:: **hackebrot**:
          This either indicats the version of cookiecutter that this template requires
          or the version of the spec itself. Not entirely sure what's better in this case
          and if we want to separate them. Going forward this will allow us to exit early
          if the used cookiecutter CLI is not the latest one, but the template depends
          on a new built-in extension or new fields.



variables Field
---------------
The **variables** field is an array of objects implementated as an
array of ordered dictionaries (`OrderedDict`_). Each element of the array,
being an OrderedDict, is a set of key/value pairs associated with a unique
variable that is part of the jinja2 context.

The various required and optional key/value pairs associated with a variable
will be identified in the :ref:`Variables` section later in this document.


.. note:: **hackebrot**:
          The elements of this array represent a single variable, similarly to what you
          currently find in a **cookiecutter.json** file.


The Minium Cookiecutter Template
--------------------------------
Based on what has been disclosed so far, an example of a minium legal
(though relatively useless) Cookiecutter version 2 template would look like
this::

   {
       "name": "template-name",
       "cookiecutter_version": "2.0.0",
       "variables": []
   }


Optional Metadata Template Fields
=================================
The following fields are optional:

* **description**
* **version**
* **authors**
* **license**
* **keywords**
* **url**


description Field (Optional)
----------------------------
The **description** field is a string containing a human readable description
of the template.

.. note:: **hackebrot**:
          This can be used for user facing aspects, like a welcome message when running
          cookiecutter.


version Field (Optional)
------------------------
The **version** field is a string containing a version identifier; ideally
conforming the the Semantic Versioning specification (`semver`_). This version
identifier is used to version control the template.

.. note:: **hackebrot**:
          This will help us generate helpful error messages.


authors Field (Optional)
------------------------
The **authors** is an array of strings that identify the template's
maintainers.

.. note:: **hackebrot**:
          Again this will help users in case they encounter issues. Currently users tend
          to raise issues on the cookiecutter project rather than the template. I would
          like to emphasize that template authors need to make sure that their templates
          work.


license Field (Optional)
------------------------
The **license** field is a string identifying the license for the template code.

.. note:: **hackebrot**:
          The template itself is not runnable software, but contains source code. So I
          would argue that it should specify a license. Obviously this is not binding if
          the repository is missing a LICENSE file or w/e the license in question
          requires. We don't need this for a Minimal Viable Product.


keywords Field (Optional)
-------------------------
The **keywords** field is an array of strings similar in spirit to PyPI
keywords.

.. note:: **hackebrot**:
          Providing keywords in a template makes it easier for tools, such as the new
          Cookiecutter Explorer in Visual Studio or Cibopath, to search for templates.
          Currently users need to go to the template repo and scan through the README or
          even the template code to see if a template uses certain frameworks.


url Field (Optional)
--------------------
The **url** field is a string URL for the template project.

.. note:: **hackebrot**:
          We can use this to point users to the project if they encounter an error. This
          would certainly be optional.


Example Cookiecutter Template
-----------------------------
Below is an example Cookiecutter template showing all the required and optional
metadata fields; note that the variables array is still empty, but not for long::

   {
       "name": "python-project-skeleton-template",
       "cookiecutter_version": "2.0.0",

       "description": "Cookiecutter template for a general purpose Python project skeleton",
       "authors": ["E.R. Uber"],
       "version": "0.3.7",
       "license": "MIT",
       "keywords": ["cookiecutter","python", "project", "template", "skeleton"],
       "url": "http://python-project-skeleton.readthedocs.io/en/latest/index.html",

       "variables": []
   }

.. _Variables:

Variables Array
===============
The **variables** field is an array of ordered dictionaries (`OrderedDict`_).
Each dictionary represents a varible in the jinja2 context.


Required Variable Fields
------------------------
The following fields are **required** to be defined for each variable:

* **name**
* **default**

name Variable Field
^^^^^^^^^^^^^^^^^^^
The **name** variable field is a string defining the name of the variable in
the jinja2 context.

For example::

   {
      "name": "project_repo",
      ...
   }


.. note:: **hackebrot**:
          This is nothing different from what we have in the current
          **cookiecutter.json** as keys. **These must not be templated!**

default Variable Field
^^^^^^^^^^^^^^^^^^^^^^
The **default** variable field can be of any legal default value type and
is the default value of the variable named in the previous section.

The various legal types supported will be addressed in a later section.

For example, the variable named 'project_repo', may have a default value of
"cookiecutter-template-converter" as in::

   {
      "name": "project_repo",
      "default": "cookiecutter-template-converter",
      ...
   }

.. note:: **hackebrot**:
          Again this is what we already have as values. If a default is a string, we must
          assume it is templated, so we render it before prompting the user.


Optional Variable Fields
------------------------
The following variable fields are optional:

* :ref:`type <types>`
* :ref:`description <description>`
* :ref:`prompt <prompt>`
* :ref:`prompt_user <prompt_user>`
* :ref:`hide_input <hide_input>`
* :ref:`choices <choices>`
* :ref:`skip_if <skip_if>`
* :ref:`do_if <do_if>`
* :ref:`if_yes_skip_to <if_yes_skip_to>`
* :ref:`if_no_skip_to <if_no_skip_to>`
* :ref:`validation <validation>`
* :ref:`validation_flags <validation_flags>`
* :ref:`validation_msg <validation_msg>`


.. _types:

type Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The **type** variable field is a string that defines the type of the variable.

The **type** field's default value is: string.

.. note:: **hackebrot**:
          This defaults to string, which reflects the current behaviour (right now we
          cast every value to string, so we can render it). Having a type allows us not
          only to make use of `Click`_ types for prompts, but we can also cast the
          values after they have been rendered.

The reference implementation supports the following default value types:

  -  string
  -  boolean
  -  yes_no
  -  int
  -  json
  -  **float**
  -  **uuid**

.. note:: **eruber**:
          The proof-of-concept proposal omitted types **float** and **uuid**,
          but they were added to the Cookiecutter reference implementation since
          they are both inherently supported by the underlying user prompt
          functionality provided by `Click`_.


.. _description:

description Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **description** variable field is a string used to describe what the
variable means.

The **description** field's default value is: None.


.. note:: **hackebrot**:
          We can show this if the users runs verbose mode, to make it even clearer for
          what a variable is used for and potentially indicate what the requirements for
          a field are.

.. note:: **eruber**:
          It would appear that in Cookiecutter v1.6.0 (upon which the
          reference implementation of Cookiecutter v2 is based) does not
          pass the command line --verbose option to the main cookiecutter API
          call (its just used to control the logging level). So in the
          reference implementation, it is hardwired to True. Thus if a
          description is defined, it will be emitted prior to a user prompt.

          The reason the Cookiecutter reference implementation does pass
          the verbose option into the Cookiecutter API is because the reference
          implementation has a set of implementation guidelines and one of those
          guidelines was NOT to change the Cookiecutter API.


.. _prompt:

prompt Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **prompt** variable field is a string that will be used to prompt the user
for input.

The **prompt** field's default value is rendered by jinja2 as::

  'Please enter a value for "{variable.name}"'

.. note:: **hackebrot**:
          Currently we show variable [default]:, but a template author could provide
          a more friendly message allowing for a better user experience.


.. _prompt_user:

prompt_user Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **prompt_user** variable field is a boolean that if true will show user
prompts; and if false will not prompt the user for input.

The **prompt_user** field's default value is: True

.. note:: **hackebrot**:
          This can be used to hide prompts from a user if the template author wishes to
          use these fields but retrieve the information from somewhere else, for example
          the current year. This is currently supported with a hack by prepending a
          variable name with _.

.. note:: **eruber**:
          The reference implementation also still honors this hack -- a
          variable name prefixed with an underscore does not generate a user
          prompt -- it has the same effect as **"prompt_user" : false** being
          specified in the template.


.. _hide_input:

hide_input Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **hide_input** variable field is a boolean - when specified as true will
allow user input, but will not echo the user's keystrokes back to the console.
This makes it suitable for entering sensitive information like passwords.

The **hide_input** field's default value is: False

.. note:: **eruber**:
          Though not documented in the his pull request write-up, the
          actual `proof-of-concept code for context.py`_ by `hackebrot`_ does
          implement the hide_input field - and thus, so does the Cookiecutter
          reference implementation.


.. _choices:

choices Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **choices** variable field is an array of string, boolean, or number which
lists valid choice values for that variable.

The **choices** field's default value is: []

.. note:: **hackebrot**:
          This is currently supported with lists in cookiecutter.json. However this
          field would be optional for a variable and is different from type in the
          sense that a choice will still be processed to have the specified type when
          stored to the context


.. _skip_if:

skip_if Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **skip_if** variable field is a string that holds conditionals based on
other fields. The conditional logic is rendered by jinja2.

The **skip_if** field's default value is: ''

If the conditional in the **skip_if** string evaluates to True, then this
variable is skipped -- the user will see no prompt to enter data for this
variable.

.. note:: **hackebrot**:
          This one is a bit tricky. In it's current form it would be a string containing
          a jinja2 template. When prompting the user this is rendered and checked for
          equality against "True". This allows us to skip variables based on
          previously entered information.


.. _do_if:

do_if Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **do_if** variable field is a string that holds conditionals based on
other fields. The conditional logic is rendered by jinja2.

The **do_if** field's default value is: ''

If the conditional in the **do_if** string evaluates to True, then this
variable is NOT skipped -- the user will be prompted to enter data for this
variable.

.. note:: **eruber**:
          This field was added to the reference implementation to offer a
          balance to the **skip_if** field -- sometimes its just more
          convenient to express the logic in terms of what variable should be
          processed rather than what variable should be skipped.


.. _if_yes_skip_to:

if_yes_skip_to Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **if_yes_skip_to** variable field is a string that names a variable to
process next if the value of the current variable is True (yes).

This field is used with **yes_no** type variables to allow conditional
processing that can skip multiple variables.

The **if_yes_skip_to** field's default value is: None

.. note:: **eruber**:
          Added to the Cookiecutter reference implementation. Having only a
          **skip_if** mechanism became logically complex when trying to skip
          multiple variables. This field makes skipping over mulitple
          variables very easy.


.. _if_no_skip_to:

if_no_skip_to Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **if_no_skip_to** variable field is a string that names a variable to
process next if the value of the current variable is False (no).

This field is used with **yes_no** type variables to allow conditional
processing that can skip multiple variables.

The **if_no_skip_to** field's default value is: None

.. note:: **eruber**:
          Added to the Cookiecutter reference implementation as a logical
          balance to the **if_yes_skip_to** field.


.. _validation:

validation Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **validation** variable field is a string containing a regular expression
used to validate the user input.

The **validation** field's default value is: None

.. note:: **hackebrot**:
          This would allow us to have some additional checks for accepting user input.
          Think of PEP8 compliant names for Python modules. Rather than using a
          post_gen_project hook and abort generation, we could ask the user to try
          entering another value.


.. _validation_flags:

validation_flags Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **validation_flags** variable field is a list of strings. Each item in the
list names a validation flag that can be specified to control the behaviour
of the **validation** field's validation check. Specifying a flag in this list
is equivalent to setting the validation flag to True, not specifying a flag is
equivalent to setting it to False.

The **validation_flags** field's default value is: []

The default value of this variable has no effect on the validation check.

The validation flags supported are:
    * **ascii** - enabling re.ASCII
    * **debug** - enabling re.DEBUG
    * **ignorecase** - enabling re.IGNORECASE
    * **locale** - enabling re.LOCALE
    * **mulitline** - enabling re.MULTILINE
    * **dotall** - enabling re.DOTALL
    * **verbose** - enabling re.VERBOSE

    See: https://docs.python.org/3/library/re.html#re.compile

.. note:: **eruber**:
          This field was added to the Cookiecutter reference implementation
          to complete the **validation** field's functionality.

For example, to perform input vaildation that ignores case and enables
verbose, do this::

  "validation": "SOME-REALLY-MIND-ALTERING-REGULAR-EXPRESSION",
  "validation_flags": ["ignorecase", "verbose"]


.. _validation_msg:

validation_msg Variable Field (Optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **validation_msg** variable field is a string that can be used to specify
a more user friendly message to be issued when input validation fails.

The **validation_msg** field's default value is: None

.. note:: **eruber**:
          This field was added to the Cookiecutter reference implementation
          when it became apparent that the normal validation failure message
          that emits the validation regular expression, can at times, use
          some additional validation input hints -- especially if the
          validation regular expression is complex. See the example below.

For example, to support validation of a semantic version number with all of
its features, the following variable might be defined::

   {
       "name": "project_version",
       "default": "0.0.1",
       "description": "Enter the project's semantic version number (see: semver.org).",
       "prompt": "A semantic version number is of the basic form: MAJOR.MINOR.PATCHLEVEL",
       "validation": "^([0-9]|[1-9]+[0-9]*)\\.([0-9]|[1-9]+[0-9]*)\\.([0-9]|[1-9]+[0-9]*)(-)?(-[0-9A-Za-z-\\.]*)*(\\+)?(\\+[0-9A-Za-z-\\.]*)*$",
       "validation_msg": "Follow the form X.Y.Z where X, Y, and Z are non-negative integers, and MUST NOT contain leading zeroes.",
       "type": "string"
   }

As you can see the validation's regular expression is somewhat daunting, so if
a **validation_msg** is specified it will be issued in addition to the default
validation failure message that emits the regular expression.

A console session that illustrates would look like::

  Enter the project's semantic version number (see: semver.org).
  A semantic version number is of the basic form: MAJOR.MINOR.PATCHLEVEL [0.0.1]: 0.01.001
  Input validation failure against regex: '^([0-9]|[1-9]+[0-9]*)\.([0-9]|[1-9]+[0-9]*)\.([0-9]|[1-9]+[0-9]*)(-)?(-[0-9A-Za-z-\.]*)*(\+)?(\+[0-9A-Za-z-\.]*)*$', try again!
  Follow the form X.Y.Z where X, Y, and Z are non-negative integers, and MUST NOT contain leading zeroes.
  A semantic version number is of the basic form: MAJOR.MINOR.PATCHLEVEL [0.0.1]: 0.1.1


.. _hackebrot: https://github.com/hackebrot
.. _OrderedDict: https://docs.python.org/3.6/library/collections.html#collections.OrderedDict
.. _semver: http://semver.org/
.. _Click: http://click.pocoo.org/6/
.. _proof-of-concept code for context.py: https://github.com/hackebrot/cookiecutter/blob/new-context-format/cookiecutter/context.py
