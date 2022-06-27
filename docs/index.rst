.. Sphinx_proj documentation master file, created by
   sphinx-quickstart on Thu Sep  9 11:34:15 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.
.. _home:

Welcome to GEOAnalytics Canada Documentation!
=============================================

.. image:: images/GEOAnalyticsCanada.svg
   :align: center

|

.. image:: https://img.shields.io/badge/DIVIO-Documentation%20System-blue
   :target: https://documentation.divio.com/

|

Motivation
-----------
Research on climate-change, ecosystem modeling, and environmental and natural resources monitoring is based on the collection, management, analysis, and dissemination of geospatial data.

Satellite Earth Observation (EO) has been transformed by the massive increase in availability of open EO data, which began in 2008 with the opening of the Landsat data archive by the United States. It was given further impetus by the European Commission in making data from the Copernicus series of radar and optical satellites fully free and open since 2014.

On one hand, the increased availability of EO data makes it easier for scientists, businesses, and government decision makers to obtain insights from long, dense time series of multiple EO datasets. However, users cannot find, access, and use this wealth of EO data to its potential by following traditional approaches to download data and analyze it in a local computing environment.

As the volume of satellite EO data continues to grow, new analytical possibilities arise requiring new approaches to data management and processing.

***The solution is to bring the user to the data.***

To demonstrate how cloud computing systems can overcome issues with traditional approaches to satellite EO data analytics, `Hatfield <https://www.hatfieldgroup.com>`_ created the GEOAnalytics Canada Platform. This provides data, tools, and compute resources in the same environment and enables users to gain experience and understand the benefits of working in the cloud.

How to use this documentation
-----------
This documentation is made as a series of Jupyter Notebooks that you can copy (or git clone) to follow-through interactively. To see how to set this up, `follow the instructions here <1_getting_started/00-how-to-use-this-documentation.html>`_

Overview of our Documentation
===============================
The GEOAnalytics Canada documentation is organized into sections consisting of all tutorials and guides, from getting started with the platform, to step-by-step workflow examples. Click the card below that's most relevant to your needs!

.. _cards-clickable:

.. grid:: 3

    .. grid-item-card::
      :link: getting_started_index
      :link-type: ref

      :octicon:`terminal;1em;sd-text-info` **Getting Started**
      ^^^
      **The Basics.** If you're new to GEOAnalytics Canada, start here. These
      tutorials are designed to introduce you to the most important features of GEOAnalytics Canada.


    .. grid-item-card::
      :link: real_world_index
      :link-type: ref

      :octicon:`checklist;1em;sd-text-info` **Real World Examples**
      ^^^
      **Common Use Cases.** Practical single use-case examples of problems typically see in a real world situation.

    .. grid-item-card::
      :link: scientific_workflows_index
      :link-type: ref

      :octicon:`workflow;1em;sd-text-info` **Scientific Workflows**
      ^^^
      **End-to-End Pipelines.** Practical step-by-step example workflows from start to finish that you would typically see in a real world situation.

Already know what you're looking for?
--------------------------------------

Navigate directly to any of our pages using the Table of Contents or the Search function
on the left sidebar.

.. toctree::
   :caption: Contents:
   :maxdepth: 1

   Introduction <self>
   1_getting_started/index
   2_real_world_examples/index
   3_scientific_workflows/index
