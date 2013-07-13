Advanced Topics
===============

URL escaping
-------------

URL escaping is basically managed by PyHole. Example:

    >>> from pyhole import PyHole
    >>> proxy = PyHole('http://domain.tld/rest_api')
    >>> proxy['text and spaces']
    http://domain.tld/rest_api/text%20and%20spaces
    >>> proxy(param='some/path')
    http://domain.tld/rest_api?param=some%2Fpath

Forcing trailing slash
----------------------

Force trailing slash is off by default not to confuse a simple use like this:

    >>> from pyhole import PyHole
    >>> proxy = PyHole('http://domain.tld/rest_api')
    >>> proxy['just_run_this.php']
    http://domain.tld/rest_api/just_run_this.php

Turning it on 

    >>> from pyhole import PyHole
    >>> proxy = PyHole('http://domain.tld/rest_api', force_slash=True)


will cause a bad effect in this case

    >>> proxy['just_run_this.php']
    http://domain.tld/rest_api/just_run_this.php/

From the other hand, following `this url design philosophy`_ requires to use ``force_slash=True``

    >>> proxy.this.looks.just.nice
    http://domain.tld/rest_api/this/looks/just/nice/


.. _`this url design philosophy`: https://docs.djangoproject.com/en/dev/misc/design-philosophies/?from=olddocs#definitive-urls


YAML support
------------

Whenever data returned from API is in yaml format, PyHole can un yaml it on every request. 

.. autoclass:: pyhole.yamled.PyHoleYamled
    :members:

Just import PyHoleYamled instead of PyHole

    >>> from pyhole.yamled import PyHoleYamled

the pyhole api is the same, only on every post() and get() data will be yaml.load().

JSON support
------------

Whenever data returned from API is in json format, PyHole can un json it on every request. 

.. autoclass:: pyhole.jsoned.PyHoleJsoned
    :members:

Just import PyHoleJsoned instead of PyHole

    >>> from pyhole.jsoned import PyHoleJsoned

the pyhole api is the same, only on every post() and get() data will be json.loads().

Creating custom response format wrapper
---------------------------------------

It is very easy to create a custom wrapper, like e.g. PyHoleYamled. The only thing is to inherit from PyHole and override method 

.. automethod:: pyhole.PyHole.get_response_wrapper

The source code of PyHoleYamled is as simple as that::

    from pyhole import PyHole
    import yaml

    class PyHoleYamled(PyHole):
        '''PyHole support for yaml that process every response with yaml.load function'''
        def get_response_wrapper(self):
            return yaml.load





Cookies support
---------------

By default PyHole accepts and sends cookies. Cookies are stored only for subsequent calls on the same proxy object.

urllib opener
-------------

Standard urllib opener used in PyHole is ``urllib2.build_opener(urllib2.HTTPCookieProcessor())``. This object is returned by method

.. automethod:: pyhole.PyHole.get_opener

If you need to customize it inherit and override this method or use opener parameter for ``PyHole.__init__``.
