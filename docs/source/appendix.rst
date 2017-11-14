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

********
Appendix
********
This appendix houses the primary Cookiecutter project template that drove
the implementation decisions made in the reference implementation referenced
in this guide::

   {
       "name": "python-project-skeleton-template",
       "cookiecutter_version": "2.0.0",
       "_inception": "Transformed by cctconvert 1.0.1 Fri Nov  3 20:17:29 2017",
       "description": "Cookiecutter template for a general purpose Python project skeleton",
       "authors": ["E.R. Uber"],
       "license": "MIT",
       "keywords": ["cookiecutter", "python", "project", "template", "skeleton"],
       "url": "https://github.com/eruber/python-project-skeleton",
       "variables": [
           {
               "name": "author_name",
               "default": "E.R. Uber",
               "description": "Identify the author of this project.",
               "prompt": "Enter the author's name",
               "type": "string"
           },
           {
               "name": "author_email",
               "default": "eruber@gmail.com",
               "prompt": "Enter the author's email address",
               "type": "string"
           },
           {
               "name": "project_name",
               "default": "Project Skeleton",
               "prompt": "Enter a short, space delimited, name for the project",
               "type": "string"
           },
           {
               "name": "project_version",
               "default": "0.0.1",
               "description": "Enter the project's semantic version number (see: semver.org).",
               "prompt": "A semantic version number is of the basic form: MAJOR.MINOR.PATCHLEVEL",
               "validation": "^([0-9]|[1-9]+[0-9]*)\\.([0-9]|[1-9]+[0-9]*)\\.([0-9]|[1-9]+[0-9]*)(-)?(-[0-9A-Za-z-\\.]*)*(\\+)?(\\+[0-9A-Za-z-\\.]*)*$",
               "validation_msg": "Follow the form X.Y.Z where X, Y, and Z are non-negative integers, and MUST NOT contain leading zeroes.",
               "type": "string"
           },
           {
               "name": "project_dist",
               "default": "{{ cookiecutter.project_name.lower().replace(' ', '-') }}-{{ cookiecutter.project_version }}",
               "prompt_user": false,
               "type": "string"
           },
           {
               "name": "project_repo",
               "default": "python-{{ cookiecutter.project_name.lower().replace(' ', '-') }}",
               "prompt": "Enter the project's repository name",
               "type": "string"
           },
           {
               "name": "project_pkg",
               "default": "{{ cookiecutter.project_repo.replace('-', '_') }}",
               "prompt": "Enter the project's Python package name",
               "type": "string"
           },
           {
               "name": "project_description",
               "default": "A general purpose Python project skeleton",
               "prompt": "Enter a short description of the project",
               "type": "string"
           },
           {
               "name": "project_license",
               "default": "Apache2",
               "prompt": "Select the project's Open Source License",
               "type": "string",
               "choices": [
                   "Apache2",
                   "BSD3",
                   "ISC",
                   "MIT",
                   "GNU-GPL-v3"
               ]
           },
           {
               "name": "project_cmdline_interface",
               "default": "none",
               "description": "Select a Command Line Interface for the project.",
               "prompt": "If the project will have no Command Line Interface, select none",
               "type": "string",
               "choices": [
                   "none",
                   "click"
               ]
           },
           {
               "name": "project_graphical_inteface",
               "default": "none",
               "description": "Select a Graphical User Interface for the project.",
               "prompt": "If the project will have no Graphical User Interface, select none",
               "type": "string",
               "choices": [
                   "none",
                   "tk",
                   "wxwidgets",
                   "kivy"
               ]
           },
           {
               "name": "project_shell_interface",
               "default": "none",
               "description": "Select a Shell Interface for the project.",
               "prompt": "If the project will have no Shell Interface, select none",
               "type": "string",
               "choices": [
                   "none",
                   "cmd",
                   "shellocity"
               ]
           },
           {
               "name": "project_machine_interface",
               "default": "none",
               "description": "Select a Machine Interface for the project.",
               "prompt": "If the project will have no Machine Interface, select none",
               "type": "string",
               "choices": [
                   "none",
                   "api"
               ]
           },
           {
               "name": "project_configuration_enabled",
               "default": true,
               "prompt": "Will this project require a configuration file?",
               "type": "yes_no",
               "if_no_skip_to": "project_uses_existing_logging_facilities"
           },
           {
               "name": "project_config_format",
               "default": "toml",
               "prompt": "Select a configuration file format.",
               "type": "string",
               "choices": [
                   "toml",
                   "yaml",
                   "json",
                   "ini"
               ]
           },
           {
               "name": "project_uses_existing_logging_facilities",
               "default": false,
               "prompt": "Will this project use existing external logging facilities?",
               "type": "yes_no",
               "if_yes_skip_to": "github_username"
           },
           {
               "name": "project_logging_enabled",
               "default": true,
               "prompt": "Will this project provide its own logging facilities?",
               "type": "yes_no",
               "if_no_skip_to": "github_username"
           },
           {
               "name": "project_console_logging_enabled",
               "default": true,
               "prompt": "Will the project's logging facilities include logging to the console?",
               "type": "yes_no",
               "if_no_skip_to": "project_file_logging_enabled",
               "do_if": "{{cookiecutter.project_logging_enabled == True}}"
           },
           {
               "name": "project_console_logging_level",
               "default": "WARN",
               "prompt": "Select the minimum logging level to log to the console.",
               "type": "string",
               "choices": [
                   "WARN",
                   "INFO",
                   "DEBUG",
                   "ERROR"
               ],
               "do_if": "{{cookiecutter.project_logging_enabled == True}}"
           },
           {
               "name": "project_file_logging_enabled",
               "default": true,
               "prompt": "Will the project's logging facilities include logging to a file?",
               "type": "yes_no",
               "if_no_skip_to": "github_username",
               "do_if": "{{cookiecutter.project_logging_enabled == True}}"
           },
           {
               "name": "project_file_logging_level",
               "default": "DEBUG",
               "prompt": "Select the minimum logging level to log to a file",
               "type": "string",
               "choices": [
                   "DEBUG",
                   "INFO",
                   "WARN",
                   "ERROR"
               ],
               "do_if": "{{cookiecutter.project_logging_enabled == True}}"
           },
           {
               "name": "project_file_logging_type",
               "default": "log_to_single_file",
               "prompt": "Select what type of file logging should be used",
               "type": "string",
               "choices": [
                   "log_to_single_file",
                   "log_to_rotating_file"
               ],
               "do_if": "{{cookiecutter.project_logging_enabled == True}}"
           },
           {
               "name": "github_username",
               "default": "eruber",
               "prompt": "Enter your GitHub User Name",
               "type": "string"
           },
           {
               "name": "test_framework",
               "default": "pytest",
               "description": "Select what type of test framework to use.",
               "prompt": "Selecting none will generate no test framework support",
               "type": "string",
               "choices": [
                   "pytest",
                   "none"
               ]
           },
           {
               "name": "test_coverage_enabled",
               "default": true,
               "prompt": "Will this project's testing report on test coverage?",
               "type": "yes_no"
           },
           {
               "name": "ci_travis_enabled",
               "default": true,
               "prompt": "Will this project use Continuous Integration facilities provided by Travis?",
               "type": "yes_no"
           },
           {
               "name": "ci_appveyor_enabled",
               "prompt": "Will this project use Continuous Integration facilities provided by AppVeyor?",
               "default": true,
               "type": "yes_no"
           },
           {
               "name": "project_coding_standards",
               "default": "flake8",
               "description": "Select a coding standards support tool.",
               "prompt": "Selecing none will have the effect of running no code quality scans",
               "type": "string",
               "choices": [
                   "flake8",
                   "pylama",
                   "none"]
           },
           {
               "name": "project_complexity_enabled",
               "prompt": "Should a pytest plugin to run McCabe Code Complexity Checker be added to this project?",
               "default": true,
               "type": "yes_no",
               "do_if": "{{ cookiecutter.test_framework == 'pytest' }}"
           },
           {
               "name": "deploy_pypi_enabled",
               "default": true,
               "prompt": "Will this project ultimately be deloyed to Python's Package Index site?",
               "type": "yes_no"
           },
           {
               "name": "deploy_readthedocs_enabled",
               "default": true,
               "prompt": "Will this project's documentation ultimately be deployed to ReadTheDocs.org?",
               "type": "yes_no"
           },
           {
               "name": "_derived",
               "type": "json",
               "default": {
                   "author": "{{ cookiecutter.author_name }} <{{ cookiecutter.author_email }}>",
                   "incept_date": "{% now 'local', '%c' %}",
                   "project_file_logging_rotating_file_count": "5",
                   "github": {
                       "url": "https://github.com/{{ cookiecutter.github_username }}/{{ cookiecutter.project_repo }}"
                   },
                   "ci": {
                       "travis": {
                           "username": "{{ cookiecutter.github_username }}",
                           "url": "https://travis-ci.org/{{ cookiecutter.travis_username }}/{{ cookiecutter.project_repo }}"
                       },
                       "appveyor": {
                           "username": "{{ cookiecutter.github_username }}",
                           "url": "https://travis-ci.org/{{ cookiecutter.travis_username }}/{{ cookiecutter.project_repo }}"
                       }
                   },
                   "deploy": {
                       "pypi": {
                           "username": "{{ cookiecutter.github_username }}",
                           "url": "https://pypi.python.org/pypi"
                       },
                       "readthedocs": {
                           "username": "{{ cookiecutter.github_username }}",
                           "url_project": "https://readthedocs.org/projects/{{ cookiecutter.project_repo }}/",
                           "url_docs": "http://{{ cookiecutter.project_repo }}.readthedocs.io/en/latest/"
                       }
                   }
               },
               "prompt_user": false
           }
       ]
   }

