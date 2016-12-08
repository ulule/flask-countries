================
Flask Countries
================

A Flask extension that provides country choices for use with forms, flag
icons static files.

Installation
============

1. ``pip install flask-countries``

For more accurate sorting of translated country names, install the optional
pyuca_ package.

.. _pyuca: https://pypi.python.org/pypi/pyuca/


The ``Country`` object
----------------------

An object used to represent a country, instanciated with a two character
country code.

It can be compared to other objects as if it was a string containing the
country code and when evaluated as text, returns the country code.

name
  Contains the full country name.

flag
  Contains a URL to the flag.

unicode_flag
  A unicode glyph for the flag for this country. Currently well-supported in
  iOS and OS X. See https://en.wikipedia.org/wiki/Regional_Indicator_Symbol
  for details.

alpha3
  The three letter country code for this country.

numeric
  The numeric country code for this country (as an integer).

numeric_padded
  The numeric country code as a three character 0-padded string.



Get the countries from Python
=============================

Use the ``flask_countries.countries`` object instance as an iterator of ISO
3166-1 country codes and names (sorted by name).

For example::

    >>> from flask_countries import countries
    >>> dict(countries)['NZ']
    'New Zealand'

    >>> for code, name in list(countries)[:3]:
    ...     print("{name} ({code})".format(name=name, code=code))
    ...
    Afghanistan (AF)
    Åland Islands (AX)
    Albania (AL)

Country names are translated using Babel's standard ``ugettext``.
If you would like to help by adding a translation, please visit
https://www.transifex.com/projects/p/django-countries/


Customization
=============

Customize the country list
--------------------------

Country names are taken from the official ISO 3166-1 list. If your project
requires the use of alternative names, the inclusion or exclusion of specific
countries then use the ``COUNTRIES_OVERRIDE`` setting.

A dictionary of names to override the defaults.

Note that you will need to handle translation of customised country names.

Setting a country's name to ``None`` will exclude it from the country list.
For example::

    COUNTRIES_OVERRIDE = {
        'NZ': _('Middle Earth'),
        'AU': None
    }

If you have a specific list of countries that should be used, use
``COUNTRIES_ONLY``::

    COUNTRIES_ONLY = ['NZ', 'AU']

or to specify your own country names, use a dictionary or two-tuple list
(string items will use the standard country name)::

    COUNTRIES_ONLY = [
        'US',
        'UK'
        ('NZ', _('Middle Earth')),
        ('AU', _('Desert')),
    ]


Show certain countries first
----------------------------

Provide a list of country codes as the ``COUNTRIES_FIRST`` setting and they
will be shown first in the countries list (in the order specified) before all
the alphanumerically sorted countries.

If you want to sort these initial countries too, set the
``COUNTRIES_FIRST_SORT`` setting to ``True``.

By default, these initial countries are not repeated again in the
alphanumerically sorted list. If you would like them to be repeated, set the
``COUNTRIES_FIRST_REPEAT`` setting to ``True``.

Finally, you can optionally separate these 'first' countries with an empty
choice by providing the choice label as the ``COUNTRIES_FIRST_BREAK`` setting.


Customize the flag URL
----------------------

The ``COUNTRIES_FLAG_URL`` setting can be used to set the url for the flag
image assets. It defaults to::

    COUNTRIES_FLAG_URL = 'flags/{code}.gif'

The URL can be relative to the STATIC_URL setting, or an absolute URL.

The location is parsed using Python's string formatting and is passed the
following arguments:

    * code
    * code_upper

For example: ``COUNTRIES_FLAG_URL = 'flags/16x10/{code_upper}.png'``

No checking is done to ensure that a static flag actually exists.


Single field customization
--------------------------

To customize an individual field, rather than rely on project level settings,
create a ``Countries`` subclass which overrides settings.

To override a setting, give the class an attribute matching the lowercased
setting without the ``COUNTRIES_`` prefix.

REST output format
------------------

By default, the field will output just the country code. If you would rather
have more verbose output, instantiate the field with ``country_dict=True``,
which will result in the field having the following output structure::

    {"code": "NZ", "name": "New Zealand"}

Either the code or this dict output structure are acceptable as input
irregardless of the ``country_dict`` argument's value.
