.. image:: https://github.com/LuisAlejandro/pypicontents-build/blob/master/branding/banner.svg

..

    PyPIContents is an application that generates a Module Index from the
    Python Package Index (PyPI) and also from various versions of the Python
    Standard Library.

.. image:: https://img.shields.io/pypi/v/pypicontents.svg
   :target: https://pypi.python.org/pypi/pypicontents
   :alt: PyPI Package

.. image:: https://img.shields.io/github/release/LuisAlejandro/pypicontents.svg
   :target: https://github.com/LuisAlejandro/pypicontents/releases
   :alt: Github Releases

.. image:: https://img.shields.io/github/issues/LuisAlejandro/pypicontents
   :target: https://github.com/LuisAlejandro/pypicontents/issues?q=is%3Aopen
   :alt: Github Issues

.. image:: https://github.com/LuisAlejandro/pypicontents/workflows/Push/badge.svg
   :target: https://github.com/LuisAlejandro/pypicontents/actions?query=workflow%3APush
   :alt: Push

.. image:: https://readthedocs.org/projects/pypicontents/badge/?version=latest
   :target: https://readthedocs.org/projects/pypicontents/?badge=latest
   :alt: Read The Docs

.. image:: https://img.shields.io/discord/809504357359157288.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2
   :target: https://discord.gg/M36s8tTnYS
   :alt: Discord Channel

|
|

.. _different repository: https://github.com/LuisAlejandro/pypicontents
.. _pipsalabim: https://github.com/LuisAlejandro/pipsalabim
.. _full documentation: https://pypicontents.readthedocs.org
.. _Contents: https://www.debian.org/distrib/packages#search_contents

PyPIContents generates a configurable index written in ``JSON`` format that
serves as a database for applications like `pipsalabim`_. It can be configured
to process only a range of packages (by initial letter) and to have
memory, time or log size limits. It basically aims to mimic what the
`Contents`_ file means for a Debian based package repository, but for the
Python Package Index.

This repository stores the index. The actual application lives in a `different
repository`_.

For more information, please read the `full documentation`_.

Getting started
===============

About the Module Index
----------------------

.. _Github Actions: https://github.com/LuisAlejandro/pypicontents/actions
.. _pypi.json.xz: https://github.com/LuisAlejandro/pypicontents/blob/master/pypi.json.xz

In the `pypi.json.xz`_ file you will find a
dictionary with all the packages registered at the main PyPI instance, each one
with the following information::

    {
        "pkg_a": {
            "version": [
                "X.Y.Z"
            ],
            "modules": [
                "module_1",
                "module_2",
                "..."
            ],
            "cmdline": [
                "path_1",
                "path_2",
                "..."
            ]
        },
        "pkg_b": {
             "...": "..."
        },
        "...": {},
        "...": {}
    }

This index is generated using `Github Actions`_. This is done by executing the
``setup.py`` file of each package through a monkeypatch that allows us to read
the parameters that were passed to ``setup()``. Check out
``pypicontents/api/process.py`` for more info.

Use cases
~~~~~~~~~

.. _Pip Sala Bim: https://github.com/LuisAlejandro/pipsalabim

* Search which package (or packages) contain a python module. Useful to
  determine a project's ``requirements.txt`` or ``install_requires``.

::

    import json
    import lzma
    from urllib.request import urlopen
    from pprint import pprint

    pypic = 'https://raw.githubusercontent.com/LuisAlejandro/pypicontents-build/master/pypi.json.xz'

    with urlopen(pypic) as f:
        pypicontents = json.loads(lzma.open(f.read()).read())

    def find_package(contents, module):
        for pkg, data in contents.items():
            for mod in data['modules']:
                if mod == module:
                    yield {pkg: data['modules']}

    # Which package(s) contain the 'django' module?
    # Output:
    pprint(list(find_package(pypicontents, 'django')))

..

    Hint: Check out `Pip Sala Bim`_.

Known Issues
~~~~~~~~~~~~

#. Some packages have partial or totally absent data because of some of these
   reasons:

    #. Some packages depend on other packages outside of ``stdlib``. We try to
       override these imports but if the setup heavily depends on it, it will
       fail anyway.
    #. Some packages are broken and error out when executing ``setup.py``.
    #. Some packages are empty or have no releases.

#. If a package gets updated on PyPI and the change introduces or deletes
   modules, then it won't be reflected until the next index rebuild. You
   should check for the ``version`` field for consistency. Also, if you need a
   more up-to-date index, feel free to download this software and build your
   own index.

Getting help
============

.. _Discord server: https://discord.gg/M36s8tTnYS
.. _StackOverflow: http://stackoverflow.com/questions/ask

If you have any doubts or problems, suscribe to our `Discord server`_ and ask for help. You can also
ask your question on StackOverflow_ (tag it ``candyshop``) or drop me an email at luis@collagelabs.org.

License
=======

.. _AUTHORS.rst: AUTHORS.rst
.. _GPL-3 License: LICENSE

Copyright 2016-2022, PyPIContents Developers (read AUTHORS.rst_ for a full list of copyright holders).

Released under a `GPL-3 License`_.

Made with :heart: and :hamburger:
=================================

.. image:: https://github.com/LuisAlejandro/pypicontents-build/blob/master/branding/author-banner.svg

.. _LuisAlejandroTwitter: https://twitter.com/LuisAlejandro
.. _LuisAlejandroGitHub: https://github.com/LuisAlejandro
.. _luisalejandro.org: https://luisalejandro.org

|

    Web luisalejandro.org_ · GitHub `@LuisAlejandro`__ · Twitter `@LuisAlejandro`__

__ LuisAlejandroGitHub_
__ LuisAlejandroTwitter_
