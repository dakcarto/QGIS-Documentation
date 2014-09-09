.. comment out this Section (by putting '|updatedisclaimer|' on top) if file is not uptodate with release

.. _dev_shop_processing:

*************************************
Workshop - Processing Plugin Provider
*************************************

See `Processing Example Provider`_. The example code is very well documented.

.. _Processing Example Provider:
   https://github.com/qgis/QGIS/tree/master/python/plugins/processing/algs/exampleprovider

Libraries as basis for Processing providers
...........................................

One of the more interesting Processing providers is the `LWGEOM Provider`_ by
Faunalia_. Normally, a provider will communicate with an external GIS tool via
the tool's command line interface (CLI); but, the ``LWGEOM Provider`` works
directly by calling a shared library, :file:`liblwgeom` from the PostGIS_
distribution, via Python's `ctypes module`_.

.. _LWGEOM Provider: http://plugins.qgis.org/plugins/processinglwgeomprovider/
.. _Faunalia: http://www.faunalia.eu
.. _PostGIS: http://postgis.net
.. _ctypes module: https://docs.python.org/2/library/ctypes.html


The plugin/provider is included in the workshop src folder. Specifically, note
how the `LwgeomAlgorithm class`_ not only uses :file:`ctypes` to call functions of the library,
but also how it is used to create C-type structures to facilitate two-way
communication between the algorithm and library.


.. _LwgeomAlgorithm class: https://github.com/faunalia/processinglwgeomprovider/blob/2.0/LwgeomAlgorithm.py
