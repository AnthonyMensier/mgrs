.. _home:

mgrs: Converting to and from MGRS and Decimal Degrees
------------------------------------------------------------------------------

GeoTrans_ provides C code for converting to and from MGRS, but well, it's
C code :).  This is a simple ctypes_ wrapper around two of the MGRS-related
functions in GeoTrans_.

This library has an internal copy of some of the files from GeoTrans_ 2.4.2.

.. _`GeoTrans`: http://earth-info.nga.mil/GandG/geotrans/
.. _`ctypes`: http://docs.python.org/library/ctypes.html



ChangeLog
------------------------------------------------------------------------------

1.4.3

* Wheels
* black, flake8, and isort linters

1.4.2

* GitHub Action builders needed to be changed to push release

1.4.1

* Fix install requirements #34

1.4.0

* Alias and deprecate RTreeError #33

1.3.9

* MGRS now requires packaging library #31
* Fix wheel imports of shared libs

1.3.8

* UTF-8 encoding for all strings.
* Fix #29 implicit tuple on return of UTMToMGRS

1.3.7

* automated building of osx, linux and win64 wheels
* Migrate from TravisCI => GitHub Actions
* Warnings cleanup
* flake8 validation

1.3.6

* required positional argument for wheel.425tags.get_platform()
  https://github.com/hobu/mgrs/pull/24

1.3.4

* Truncate, don't round results https://github.com/hobu/mgrs/pull/15
* Apply license

1.3.3

* SOABI support #10 https://github.com/hobu/mgrs/pull/10
* Clean up some compilation warnings
* Travis builds https://travis-ci.org/hobu/mgrs

1.3.2

* Better Windows support
* Bug fix for 3 digit longitudes

1.3.1

* Python 3.x support
* Allow user to override precision in UTMToMGRS


Usage
------------------------------------------------------------------------------

In a nutshell::

    >>> import mgrs

    >>> latitude = 42.0
    >>> longitude = -93.0

    >>> m = mgrs.MGRS()
    >>> c = m.toMGRS(latitude, longitude)
    >>> c
    '15TWG0000049776'

    >>> d = m.toLatLon(c)
    >>> d
    (41.999997975127997, -93.000000000000014)

    >>> y = '321942.29N'
    >>> yd = m.dmstodd(y)
    32.328414

    >>> d, m, s = m.ddtodms(32.328414)
    >>> d, m, s
    (32.0, 19.0, 42.290400)

You can also control the precision of the MGRS grid with the MGRSPrecision
arguments in .toMGRS().
MGRSPrecision takes an integer as input, ranking from 0 to 5, 5 being the most precise conversion. 

* MGRSPrecision=5: 1m precision (10 digits: 5 for easting and 5 for northing)
* MGRSPrecision=4: 10m precision (8 digits: 4 for easting and 4 for northing)
* MGRSPrecision=3: 100m precision (6 digits: 3 for easting and 3 for northing)
* MGRSPrecision=2: 1km precision (4 digits: 2 for easting and 2 for northing)
* MGRSPrecision=1: 10km precision (2 digits: 1 for easting and 1 for northing)
* MGRSPrecision=0: 100km precision (no digits for easting and northing, just the grid letters)

Example
------------------------------------------------------------------------------

5 vs 0 MGRSPrecision setting::

    >>> m = mgrs.MGRS()
    >>> c = m.toMGRS(latitude, longitude, 5)
    >>> c
    '15TWG0000049776'


    >>> m = mgrs.MGRS()
    >>> c = m.toMGRS(latitude, longitude, 0)
    >>> c
    '15TWG'
