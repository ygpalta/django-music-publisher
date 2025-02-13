Installation and Upgrading
****************************************

Code repository for DMP can be found at https://github.com/matijakolaric-com/django-music-publisher.

Installation
++++++++++++++++++++++++++++++++++++++++++++++++++++

DMP (Django-Music-Publisher) is based on Django Web Framework (https://djangoproject.org), and requires
Python 3 (https://python.org). It can be installed to a PC, but installing int into a cloud is highly recommended.

There are two providers where installation is semi-automated:

Digital Ocean
----------------------


.. raw:: html

    <a href="https://cloud.digitalocean.com/apps/new?repo=https%3A%2F%2Fgithub.com%2Fmatijakolaric-com%2Fdjango-music-publisher%2Ftree%2Fmaster&refcode=b05ea0e8ec84" target="_blank">
        <img src="https://www.deploytodo.com/do-btn-blue.svg" alt="Deploy to DO">
    </a>

..
    `This wizard <https://dmp.matijakolaric.com/install/>`_ will help you in deploying
    DMP.
    
    .. figure:: /images/pre_wizard.png
       :width: 100%
    
    In the last step, you will be asked where you want to deploy it. Below are the options.
    
    Heroku
    ======================================================
    
    Deployment
    --------------------
    
    This is the simplest option, and hosting starts from $16 per month (enough for most small publishers).
    The whole process takes under 5 minutes, and other than entering the data about the publisher and initial 
    password, it is all menus and next-next-next when using the
    `wizard <https://dmp.matijakolaric.com/install/>`_.
    
    Upgrading
    -------------------
    
    While installation to Heroku is really simple, updating requires some technical knowledge. The simplest way to update is to install `Heroku CLI (command line interface) <https://devcenter.heroku.com/articles/heroku-cli>`_. It can be installed on Windows, Mac and Linux.
    
    Then you log in, clone the repository, enter the folder, add a new remote and push:
    
    .. code-block:: bash
    
       $ heroku login
       $ git clone https://github.com/matijakolaric-com/django-music-publisher.git
       $ cd django-music-publisher/
       django-music-publisher$ heroku git:remote --app yourapp 
       django-music-publisher$ git push heroku master
       
    If you are upgrading from a version older than 20.7, you may need to delete an old buildpack, which can be found in Heroku dashboard in the ``Settings`` tab.
    
    Custom installation
    ++++++++++++++++++++++++++++++++++++++++++++++++++++++

DMP - Django-Music-Publisher is based on Django, which can be installed on Windows,
Mac and Linux PCs and servers. For more information, consult the official
`Deploying Django <https://docs.djangoproject.com/en/3.0/howto/deployment/>`_ documentation.

If you plan to use Django-Music-Publisher as one of the apps in your 
Django project, there is nothing special about it::

    pip install --upgrade django_music_publisher

Add ``music_publisher.apps.MusicPublisherConfig`` to ``INSTALLED_APPS``. Almost everything goes
through the Django Admin. The only exception is royalty calculation, which has to be added to
``urls.py``

.. code:: python

    from music_publisher.royalty_calculation import RoyaltyCalculationView

    urlpatterns = [
        ...
        path('royalty_calculation/', RoyaltyCalculationView.as_view(), name='royalty_calculation'),
    ]

There are several required `settings`_.

.. _settings:

Settings
===================================

Secret key
-----------------------------------

Django requires ``SECRET_KEY`` to be set.

Publisher-related settings
-----------------------------------

* ``PUBLISHER_NAME`` - Name of the publisher using Django-Music-Publisher, required
* ``PUBLISHER_IPI_BASE`` - Publisher's IPI *Base* Number, rarely used
* ``PUBLISHER_IPI_NAME`` - Publisher's IPI *Name* Number, required
* ``PUBLISHER_CODE`` - Publisher's CWR Delivery code, defaults to ``000``, which is not accepted by CMOs, but may be accepted by (sub-)publishers.
* ``PUBLISHER_SOCIETY_PR`` - Publisher's performance collecting society (PRO) numeric code, required
* ``PUBLISHER_SOCIETY_MR`` - Publisher's mechanical collecting society (MRO) numeric code
* ``PUBLISHER_SOCIETY_SR`` - Publisher's synchronization collecting society numeric code, rarely used

For the list of codes, please have a look at societies.csv file in the music_publisher
folder of the code repository.

Agreement-related settings
-----------------------------------

* ``PUBLISHING_AGREEMENT_PUBLISHER_PR`` - Performance share transferred to the publisher, default is '0.5' (50%)
* ``PUBLISHING_AGREEMENT_PUBLISHER_MR`` - Mechanical share transferred to the publisher, default is '1.0' (100%)
* ``PUBLISHING_AGREEMENT_PUBLISHER_SR`` - Synchronization share transferred to the publisher, default is '1.0' (100%)

S3 storage
------------------------------------

Recommended S3 provider is `Digital Ocean <https://m.do.co/c/b05ea0e8ec84>`_, it is simpler to set up and more affordable 
than AWS. They call S3 *Spaces*. 

For Digital Ocean, you need to set up only four config (environment) variables.

.. figure:: /images/installation_do_f1.png
   :width: 100%

* ``S3_REGION`` (alias for ``AWS_S3_REGION_NAME``) and ``S3_BUCKET`` 
  (alias for ``AWS_STORAGE_BUCKET_NAME``), you get them when you set up your *Spaces*,
  and

.. figure:: /images/installation_do_f2.png
   :width: 100%

* ``S3_ID`` (alias for ``AWS_ACCESS_KEY_ID``) and
  ``S3_SECRET`` (alias for ``AWS_SECRET_ACCESS_KEY``), you get them when you generate 
  your *Spaces* API key.

If you want to use AWS or some other S3 provider, the full list of settings is 
available 
`here <https://django-storages.readthedocs.io/en/latest/backends/amazon-S3.html>`_.


Other options
------------------------------------

* ``OPTION_FORCE_CASE`` - available options are ``upper``, ``title`` and ``smart``, 
  converting nearly all strings to UPPER CASE or Title Case or just UPPERCASE fields 
  to Title Case, respectively.

* ``OPTION_FILES`` - enables support for file uploads (audio files and images), using 
  local file storage (PC & VPS)

