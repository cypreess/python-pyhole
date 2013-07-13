.. PyHole documentation master file, created by
   sphinx-quickstart on Sat May 28 11:05:03 2011.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to PyHole!
==================
PyHole provides an easy to use generic REST API client. It has object oriented style of calling and supports GET and POST  connection methods. Connections are based on urllib_ and supports all protocols supported by urllib_. 

.. _urllib: http://docs.python.org/library/urllib.html

With PyHole you can create a call to any legacy REST API just in seconds!

Quickstart
----------

A simple example will illustrate the powerful idea of PyHole calls:

  >>> from pyhole import PyHole
  >>> proxy = PyHole('http://domain.ltd/rest_api')
  >>> proxy.category[123]('get_items', item_id=34)
  http://domain.ltd/rest_api/category/123/get_items?item_id=34
  >>> proxy.category[123]('get_item', item_id=34).get()
  #this will actually make HTTP GET Request on the given URL and prints the response.

Lazy evaluation of a proxy object ensures that a real HTTP connection is not made until ``get()`` or ``post()`` method is called.

Contents
=========
.. toctree::
   :maxdepth: 2
   
   api
   advanced
   credits

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
