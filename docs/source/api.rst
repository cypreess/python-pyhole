``PyHole`` proxy object API
===========================


Constructing proxy object
-------------------------

Using PyHole requires a proxy object instantiating. Proxy object is represented by PyHole class. 

.. autoclass:: pyhole.PyHole

``url``
  base url of REST API
``params``
  dict with prepopulated query string for request
``user_agent``
  a string for User-Agent header
``timeout``
  connection timeout in seconds
``opener``
  custom urllib openerl default opener supports Cookies
``force_slash``
  if ``True`` every url will be forced to end with ``/``; default is not to prepend slash
``extra_headers``
  a dict of extra HTTP headers
 
Typically it is only needed to give ``url`` parameter::

    from pyhole import PyHole
    PyHole('http://domain.ltd/rest_api')

Building paths
--------------

API url can be easily build using object notation and PyHole proxy object with:

 * getting attribute  -- ``proxy.some.path``,
 * getting item (dict style) -- ``proxy['some']['path']``,
 * call -- ``proxy('some', 'path')``.

Tree different approaches can be used for creating path with proxy object:

By attribute get
^^^^^^^^^^^^^^^^

Getting an attribute from proxy object, will return proxy clone with attribute appended to a path list.

Example:
    >>> from pyhole import PyHole
    >>> proxy = PyHole('http://domain.ltd/rest_api')
    >>> proxy.one
    http://domain.ltd/rest_api/one
    >>> proxy.one.two
    http://domain.ltd/rest_api/one/two
    >>> proxy.one.two.three
    http://domain.ltd/rest_api/one/two/three


.. warning::

    PyHole uses some attributes for storing internal data. To avoid as much as possible namespace clashing PyHole internal attributes starts with ``_`` underscore sign. You should avoid using this names:
    ``_url``, ``_path``, ``_user_agent``, ``_headers``, ``_timeout``, ``_force_slash``, ``_trailing_slash``, ``_opener``. Instead you can easily use them with two other methods of building url.


By item get
^^^^^^^^^^^

Getting an item from proxy object in dict similar way will also append the key as a path bit.
    
    >>> from pyhole import PyHole
    >>> proxy = PyHole('http://domain.ltd/rest_api')
    >>> proxy['one']
    http://domain.ltd/rest_api/one
    >>> proxy['one']['two']
    http://domain.ltd/rest_api/one/two

Notice that numbers can be also used this way:

    >>> proxy['one']['two'][3]
    http://domain.ltd/rest_api/one/two/3

.. warning::

    Slicing is not supported --e.g. ``proxy[1:4]`` is not supported.


With this method you can also assign a value from a variable as a path bit:
    
    >>> three="third"
    >>> proxy['one']['two'][three]
    http://domain.ltd/rest_api/one/two/third

.. note::
    
    For creating a path to e.g. ``search.php`` because of dot sign you will also need to use this method:
    
    >>> proxy.one['search.php']
    http://domain.ltd/rest_api/one/search.php

By call
^^^^^^^

Calling proxy object with unnamed urguments also appends a path list:

    >>> proxy('one')
    http://domain.ltd/rest_api/one
    >>> proxy('one', 'two')
    http://domain.ltd/rest_api/one/two
    >>> proxy('one', 'two')('three')
    http://domain.ltd/rest_api/one/two/three



Adding query string
-------------------

To add parameters to HTTP request call proxy object with named argument:

    >>> proxy(param1="one")
    http://domain.ltd/rest_api?param1=one
    >>> proxy(param1="one", param2="two")
    http://domain.ltd/rest_api?param2=two&param1=one

.. note::

    Named arguments cannot have names reserved for python syntax keywords, e.g. ``class``. Following 
    example throws ``SyntaxError`` exception

    >>> proxy(class=12)
      File "<stdin>", line 1
        proxy(class=12)
                   ^
    SyntaxError: invalid syntax

To avoid clashing with syntax keywords a dict can be given as an unnamed argument for call:

    >>> proxy({'class':12})
    http://domain.ltd/rest_api?class=12
    >>> proxy({'class':12, 'param1':'one' }, param2='two')
    http://domain.ltd/rest_api?param2=two&param1=one&class=12

Parameters can be also declared in chain:

    >>> proxy.one(p1='v2').two({'p2':'v2'}).tree({'p3':'v3'})
    http://domain.ltd/rest_api/one/two/tree?p2=v2&p3=v3&p1=v2
    

Making HTTP requests
--------------------
PyHole support both GET and POST methods

GET method
^^^^^^^^^^

.. automethod:: pyhole.PyHole.get

Example:

    >>> proxy.one(p1='v2').two({'p2':'v2'}).tree({'p3':'v3'}).get()
    # This will actually fetch result via GET from http://domain.ltd/rest_api/one/two/tree?p2=v2&p3=v3&p1=v2


POST method
^^^^^^^^^^^

.. automethod:: pyhole.PyHole.post

Example:

    >>> proxy.one(p1='v2').two({'p2':'v2'}).tree({'p3':'v3'}).post({'p4': 4, 'p5': 5})
    # This will actually fetch result via POST from http://domain.ltd/rest_api/one/two/tree?p2=v2&p3=v3&p1=v2
    # using POST data {'p4': 4, 'p5': 5}
    
    
PUT method
^^^^^^^^^^

.. automethod:: pyhole.PyHole.put

Example:

    >>> proxy.one(p1='v2').two({'p2':'v2'}).tree({'p3':'v3'}).put({'p4': 4, 'p5': 5})
    # This will actually fetch result via PUT from http://domain.ltd/rest_api/one/two/tree?p2=v2&p3=v3&p1=v2
    # using PUT data {'p4': 4, 'p5': 5}
    
DELETE method
^^^^^^^^^^^^^

.. automethod:: pyhole.PyHole.delete

Example:

    >>> proxy.one(p1='v2').two({'p2':'v2'}).tree({'p3':'v3'}).delete({'p4': 4, 'p5': 5})
    # This will actually fetch result via DELETE from http://domain.ltd/rest_api/one/two/tree?p2=v2&p3=v3&p1=v2
    # using DELETE data {'p4': 4, 'p5': 5}