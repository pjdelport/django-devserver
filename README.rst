A drop in replacement for Django's built-in runserver command. Features an extendable interface for handling things such as real-time logging.

Included modules:

* SQLRealTime: Outputs queries as they happen to the terminal, including time taken.
* CacheSummary: Outputs a summary of your cache calls at the end of the request.

Installation
------------

To install the latest stable version::

	pip install git+git://github.com/dcramer/django-devserver#egg=django-devserver


You will need to include ``devserver`` in your ``INSTALLED_APPS``::

	INSTALLED_APPS = (
	    'devserver',
	    ...
	)

You may also specify additional modules to load via the ``DEVSERVER_MODULES`` setting::

	DEVSERVER_MODULES = (
	    'devserver.modules.sql.SQLRealTimeModule',
	    'devserver.modules.cache.CacheSummaryModule',
	)

Usage
-----

Once installed, using the new runserver replacement is easy::

	python manage.py rundevserver

Building Modules
----------------

Building modules in devserver is quite simple. In fact, it resembles the middleware API almost identically.

Let's take a sample module, which simple tells us when a request has started, and when it has finished::

	from devserver.modules import DevServerModule
	
	class UselessModule(DevServerModule):
	    logger_name = 'useless'
	    
	    def process_init(self):
	        self.logger.info('Request started')
	    
	    def process_complete(self):
	        self.logger.info('Request ended')
