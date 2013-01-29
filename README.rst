Flask Debug-toolbar
===================

This is a port of the excellent `django-debug-toolbar <https://github.com/django-debug-toolbar/django-debug-toolbar>`_
for Flask applications.


Important Note
--------------

The SQLAlchemy Panel is depand on Flask-SQLAlchemy(mostly the `get_debug_queries` function). Flask-SQLAlchemy uses SQLAlchemy's `ConnectionProxy` and it's already been **deprecated**. It becomes complicated to fix that panel. Since I only need simple debug info and most of my database query happend in APIs(that's why I've added the debug data into json response), I'm going to implement it without Debug-toolbar, using SQLAlchemy's new `ConnectionEvents`.

SQLAlchemy的调试功能需要项目数据交互使用Flask-SQLAlchemy插件，Flask-SQLAlchemy提供了一个 `get_debug_queries` 方法获取数据库的请求时间等数据，但SQLAlchemy现在已经不支持插件中使用的 `ConnectionProxy` 方式了，所以要修复这个SQLAlchemy的调试功能变得较为的麻烦。考虑到现在项目的数据库操作大多发生在API接口中，我决定用SQLAlchemy的 `ConnectionEvents` 在项目中直接实现数据库的调试功能。


Installation
------------

Installing is simple with pip::

    $ pip install flask-debugtoolbar


Usage
-----

Setting up the debug toolbar is simple::

    from flask import Flask
    from flask_debugtoolbar import DebugToolbarExtension

    app = Flask(__name__)

    # the toolbar is only enabled in debug mode:
    app.debug = True

    # set a 'SECRET_KEY' to enable the Flask session cookies
    app.config['SECRET_KEY'] = '<replace with a secret key>'

    toolbar = DebugToolbarExtension(app)


The toolbar will automatically be injected into Jinja templates when debug mode is on.
In production, setting ``app.debug = False`` will disable the toolbar.

See the `documentation`_ for more information.

.. _documentation: http://flask-debugtoolbar.readthedocs.org
