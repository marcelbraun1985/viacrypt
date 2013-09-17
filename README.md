# ![ViaCRYPT](assets/img/logo.png)

One time read messaging system. You can try it at [http://viacry.pt/](http://viacry.pt/).

Requirements
------------

* [Node.js](http://nodejs.org/)

Install
-------

Dependencies are handled by npm and installed like this:

    npm install

You will also need to install the grunt-cli globally if you don't already have it.

    npm install -g grunt-cli

Configuring
-----------

Configurations are found on config.js, copy and adapt yours from the config.js.sample

An `static` directory is generated from the `assets` and `templates` dirs, and some dependencies
that need to be downloaded. Those tasks are automated with grunt.

To generate the static dir, simply run `grunt` on the project root:

    grunt

The files in `template` are passed through handlebars and given the config.js module as input.
You'll have to run this every time the configuration or templates are altered.

    grunt compile

The files in `assets` are simply copied to `static`. You'll have to run this every time a file
in `assets` is altered or added.

    grunt copy

Some depenencies are downloaded, those are specified on the Gruntfile.js. You'll probably only
have to do this manually if you alter these dependencies.

    grunt curl-dir

For easing development there is a `run` task to automatically rerun a task above when one of
its input files are altered and also run a development server that will restart when needed.

    grunt run


Running
-------

Should be as simple as

    ./bin/viacrypt-server.js

or

    grunt run

for development.

Then checkout `localhost:8001` to see the app.

Translating
-----------

Translations are being done with gettext and translation files should be located on `locale/<LANG>/messages.po`.

The current scheme still needs some polishing, we could compile the .po to .mo and ease the extraction.

Although there is an `xgettext` task, using [handlebars-xgettext](https://github.com/gmarty/handlebars-xgettext)
package yield better results, as the current task will not extract source lines.

To create a new translation one can use `locale/messages.pot` as a template.

There is a script to update the current translations, it is the recommended way right now.

    ./xgettext.sh

We are aware that it only works on POSIX systems and `handlebars-xgettext` and `po2json` are
required to be installed globaly contributions to improve this subsystem are very welcome.


Deploying
---------

### Supervisor

Put the following typically on `/etc/supervisor/conf.d/viacrypt.conf`.

    [program:viacrypt]
    command=/path/to/viacrypt/bin/viacrypt-server.js
    stdout_logfile=/path/to/viacrypt/logs/viacrypt.log

To start it:

    supervisorctl start viacrypt

Supervisor has the advantage of automatic restart of the process on failures.

### Upstart

Put the following typically on `/etc/init/viacrypt.conf`.

    description "ViaCRYPT node.js server"
    start on startup
    stop on shutdown
    exec /path/to/viacrypt/bin/viacrypt-server.js >> /path/to/viacrypt/logs/viacrypt.log

To start it:

    start viacrypt
