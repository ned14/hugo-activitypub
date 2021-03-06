Static Website ActivityPub
==========================

.. |travis| image:: https://travis-ci.org/ned14/static-website-activitypub.svg?branch=master
    :align: middle
    :target: https://travis-ci.org/ned14/static-website-activitypub

\(C) 2019 Niall Douglas http://www.nedprod.com/

PyPI: https://pypi.python.org/pypi/static-website-activitypub Github: https://github.com/ned14/static-website-activitypub

Travis master branch all tests passing for Python v3: |travis|

**Note that this project is currently in alpha, and should not be used by other people.**

Implemented:

- [X] Static website HTTP server
- [X] ``/.well-known/webfinger``
- [X] ``/actors/preferredUsername``
- [ ] GET ``/actors/preferredUsername/inbox`` (returns no posts, we do not implement subscriptions)
- [ ] POST ``/actors/preferredUsername/inbox`` (returns failure, we do not implement subscriptions)
- [ ] GET ``/actors/preferredUsername/outbox`` (list all messages you have ever posted, paginated)
- [ ] POST ``/actors/preferredUsername/outbox`` (add a new message)
- [ ] AJAX based HTML page implementing POST to outbox
- [ ] Publish to PyPI

Herein is a CherryPy-based Python web server which implements the ActivityPub
(https://www.w3.org/TR/activitypub/) client-to-server REST API for static
website generators such as https://gohugo.io/ and https://jekyllrb.com/.
It permits applications which can speak the client-to-server ActivityPub
API, such as some mobile phone apps, to add posts to the static website
by writing the post into a post directory, and then to call the static
website generator to regenerate the static website.

An AJAX based HTML page is optionally also provided which can enable mobile
devices to add posts without requiring the installation of an app.

Requirements
------------
1. A working static website whose generator is supported by this service.
Currently the only post format supported is ``yaml-front-matter`` where
posts have a YAML front matter enclosed by ``---```, followed by the post
content which will be in either Markdown or HTML. This suits https://gohugo.io/
and https://jekyllrb.com/ just fine. If this doesn't suit your static
website generator, pull requests adding support for others are welcome.

2. Your static website does not have a ``.well-known`` directory or file,
as this is used is by the ActivityPub implementation.

Instructions
------------
1. Generate a default ``static-website-activitypub.yaml`` file to somewhere using:

    static-website-activitypub -w static-website-activitypub.yaml \
        --serve-directory path/to/your/public/html/directory \
        --posts-directory path/to/your/blog/posts/source/directory \
        --regenerate-command "shell script for regenerating the website" \
        --user-account username@domain.com \
        --website https://www.domain.com

Customise the yaml script if you need to.

2. Run ``static-website-activitypub -c path/to/your/static-website-activitypub.yaml``,
or you can specify all the configuation options on the command line, or by
environment variables (see output from ``--help``).

3. Deploy to production using any of the mechanisms listed at
http://docs.cherrypy.org/en/latest/deploy.html, which includes the
forking model, systemd socket activation, supervisord, wsgi amongst others.
Whilst you *can* serve your static website to HTTP exclusively using
this service, it is more scalable to serve the website using nginx etc
directly, and only proxy the ActivityPub services to this service.

