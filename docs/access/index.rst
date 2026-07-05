###########
Data access
###########

.. important::

   Data Preview 2 has not yet been released. This website is currently under development.


Services, tools, and policies for accessing the alerts and the Prompt data products.

**Data Policy:** Only Rubin data rights holders may have an account in the Rubin Science Platform (RSP) and access to Data Preview 2.
All scientists and students in the US and Chile, plus named members of international in-kind teams, have Rubin data rights.

`Learn more about the Rubin data policy <https://rubinobservatory.org/for-scientists/data-products/data-policy>`_.


Rubin Science Platform (RSP)
============================

All of the data products are available through the Rubin Science Platform (RSP).

To **get started**, follow these `instructions to sign up for an RSP account <https://rsp.lsst.io/guides/getting-started/get-an-account.html>`_, then refer to the `RSP user guides <https://rsp.lsst.io/guides/index.html>`_ and work through the :doc:`/tutorials/index`.

To **get support**, ask questions, and report issues regarding the Rubin data products, services, and tools, post a new topic in the Rubin Community Forum's `Support category <https://community.lsst.org/c/support/6>`_.

A brief overview, and some helpful tips and terminology, of the RSP's data services.

Table Access Protocol (TAP)
---------------------------

The TAP service is a protocol of the IVOA for standardized access to catalog data for discovery, search, and retrieval (`IVOA documentation for TAP <https://www.ivoa.net/documents/TAP/>`_).
The Astronomical Data Query Language (`ADQL <https://www.ivoa.net/documents/latest/ADQL.html>`_) is used by the IVOA to represent astronomy queries posted to Virtual Observatory (VO) services, including TAP.
Catalogs and images can be accessed with the TAP service via the RSP's Portal, Notebook, and API aspects.

.. toctree::
    :maxdepth: 1
    :glob:

    tap_service


Simple Image Access (SIA)
-------------------------

The SIA service is a protocol of the International Virtual Observatory Alliance (`IVOA <https://www.ivoa.net/>`_) for standardized access to image data for discovery, search, and retrieval (`IVOA documentation for SIA <https://www.ivoa.net/documents/SIA/>`_).
All images can be accessed with the SIA service via the RSP's Portal, Notebook, and API aspects.
SIA is an image access service that is broadly-used and is relatively easy to get started with.


Image cutout service
--------------------

Image cutouts are created remotely ("server-side" cutouts) and only the requested subset of pixels and metadata are returned to the user via the `Server-side Operations for Data Access <https://www.ivoa.net/documents/SODA/>`_ (SODA) protocol of the IVOA.
Use of the cutout service is recommended to reduce load on shared bandwidth.


The Butler
----------

Custom Rubin software package for data organization and handling (images and catalogs).
Images and catalogs can be accessed with the Butler via the RSP's Notebook aspect.

.. toctree::
    :maxdepth: 1
    :glob:

    butler_service

