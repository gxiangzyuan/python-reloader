Python Module Reloader for KBEngine 2.5.8
======================

|Build Status| |PyPI Version|

This library implements a dependency-based module reloader for Python in KBE.  Unlike
the builtin `reload()`_ function, this reloader will reload the requested
module *and all other modules that are dependent on that module*.

Usage
-----

The reloader works by tracking dependencies between imported modules.  It must
first be enabled in order to track those dependencies.  The reloader has no
dependency information for modules that were imported before it was enabled or
after it is disabled, so you'll probably want to enable the reloader early in
your application's startup process such as kbmain.py.

.. code-block:: python

    import reloader
    reloader.enable(blacklist=["KBEngine", ])
    import KBEngine
    from KBEDebug import *

To manually reload an entity script, pass it's name to the reloader's ``reload()``
method:
eg. we can do reloading in guiconsole tool.

.. code-block:: python

    import reloader
    reloader.reload("Account")

You can also query a module's dependencies for informational or debugging
purposes:

.. code-block:: python

    reloader.get_dependencies(example)

You can disable the reloader's dependency tracking at any time:

.. code-block:: python

    reloader.disable()

Blacklisting Modules
--------------------

There may be times when you don't want a module and its dependency hierarchy
to be reloaded.  The module might rarely change and be expensive to import.
To support these cases, you can explicitly "blacklist" modules from the
reloading process using the ``blacklist`` argument to ``enable()``.

.. code-block:: python

    reloader.enable(blacklist=['os', 'ConfigParser'])

The blacklist can be any iterable listing the fully-qualified names of modules
that should be ignored.  Note that blacklisted modules will still appear in
the dependency graph for completeness; they will just not be reloaded.

.. _`reload()`: http://docs.python.org/library/functions.html#reload
