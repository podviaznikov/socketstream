0.2.7 / 2011-11-26
==================

* Fixed HTTP API

Work on SocketStream 0.3 is coming along well and we're very excited about the power and flexibility of the new modular design. We're aiming to release the first alpha version by the end of December. Read about the new features and changes coming in SocketStream 0.3 here: https://groups.google.com/forum/#!topic/socketstream/AFwFAPMKzjU


0.2.6 / 2011-11-25
==================

* Node 0.6 compatibility. Your app should work without modification, just pass the full module path to @session.authenticate()
* Updated all npm dependencies to latest versions
* Few minor bug fixes from jjt and 6 (Peter Graham). Thanks!


0.2.5 / 2011-11-03
==================

* Reworked the way sessions are updated and synced for greater reliability under high concurrent load
* Note: As a result @session.channel.list() now requires a callback
* Fixed bug preventing mock HTTP routing (e.g. using Backbone.js) working in production
* Fixed bug preventing .gitignore file been copied into a new project
* Fixed bug preventing @request.post working in some circumstances
* You can now `include` other .jade files in /app/views. Thanks metawhite
* Added new doc on serving multiple sites on one server (Virtual Hosting). Thanks paulbjensen
* Supports SS.config.https.requestCert and SS.config.https.rejectUnauthorized
* Updated Socket.IO to 0.8.6 and Jade and Stylus to latest versions
* Updated bundled jQuery version to 1.6.4


0.2.4 / 2011-10-06
==================

* Important change: URLs which are not recognized as assets to be served or compiled now return the main client by default (instead of a Connect GET error)
* This allows use of mock HTTP routes using HTML5 Pushstate. Checkout the Router included in Backbone.js for a great implementation of this
* You can now call SocketStream programatically from an external script/test with `ss = require('socketstream'); ss.load();`
* Start the server with `ss = require('socketstream'); ss.load(); ss.start.single();` if you're using Cloud9
* Added new much needed documentation on Client-side jQuery templating [here](https://github.com/socketstream/socketstream/blob/master/doc/guide/en/developing/client-side_templates.md)
* Initial examples in README are now in pure JavaScript to lower barrier to entry for new users
* Fixed bug which can appear when loading http.coffee if it requires other files. Thanks nponeccop
* Re-instated bundling of reset.css instead of normalize.css. After trying both I find this easier to work with in practice
* Added and commented on `SS.config.redis.db_index` in newly generated /config/app.coffee file


0.2.3 / 2011-09-27
==================

Major Update!

* SocketStream no longer requires Redis when developing in single-process mode (the default):
* - Redis support is switched on by default (SS.config.redis.enabled = true) so existing apps work just as before
* - New projects have Redis disabled in the config file along with an explanation how to install & enable
* - All features work without Redis enabled except for Users Online (fixable in the future)
* - This change reduces the barriers to entry for new developers and ensures better support for Windows in the future
* Less (.less) CSS files now supported. Less and Stylus support will be moved out into optional modules in 0.3
* Plain (.css) CSS files now supported for the first time, though continue to use .styl if you wish to use @import
* You may now message individual clients (i.e. browser tabs) with SS.publish.socket() - thanks kyokpae!
* Changed console.log.apply to use new cross-browser window.log() wrapper for IE compatibility
* Cleaned-up and refactored plenty of code
* Updated other dependencies, including Jade to 0.16.0
* Errors in client-side files now shown in server-side console
* Added new example app link to Readme - thanks syrio!


0.2.2 / 2011-09-18
==================

* Updated Socket.IO to 0.8.4 (resolves issue of WebSockets not working on Google Chrome due to Google switching to draft 10 of the WebSocket protocol)


0.2.1 / 2011-09-04
==================

* When receiving an event sent to a channel the channel name is now passed to the second argument. Updated docs
* HTTP API now refuses POST data above 8Mb to prevent running out of memory. Limit can be configured with SS.config.api.post_max_size_kb
* We now take advantage of connect.staticCache() in Connect 1.7.0 for increased performance
* Upgraded Socket.IO to 0.8.3
* New projects now include an empty package.json file
* Added the public/assets directory to the default .gitignore file
* Few tweaks to /config/app.coffee and /config/http.coffee


0.2.0 / 2011-08-31
==================

* Upgraded Socket.IO to 0.8.2 to fully support native websockets in Firefox 6 and latest Chrome dev builds
* Now using default Socket.IO fallbacks which use XHR Polling by default. Socket.IO can be configured in /config/app.coffee if you wish
* Browser check module now recognizes FF 6 and no longer shows 'Incompatible Browser' message
* Added SS.require() wrapper to easily share code between /app/server files. See /doc/guide/en/developing/server_code.md
* Moved Quick Chat Demo code into its own files when creating a new project to demo namespacing and make it easier to delete
* Made code more modular so it only loads if required
* Renamed SS.config.users.online config params to SS.config.users_online to match format of other optional modules
* Moved most of the README into /doc/guide/en. These will form pages on our forthcoming website


0.2 beta 2 / 2011-08-25
=======================

* Added a new internal RPC abstraction layer:
* - ZeroMQ is no longer required to use SocketStream 0.2
* - SocketStream will automatically run in a single process (ala SocketStream 0.1) if ZeroMQ is not installed
* - The new single process mode (which can be forced with 'socketstream single') is only ~7% slower than 0.1, not bad considering the new @request object and additional error checking we perform on each request
* - Removed ZeroMQ dependency from package.json to allow compatibility with Cloud9IDE and other platform hosting services which do not allow installing and compilation of C libraries
* - Revised `socketstream help` to show how ZeroMQ and other C dependencies can be optionally installed
* Added new server-side events:
* - 'channel:subscribe' and 'channel:unsubscribe'
* - 'application:exception' allowing you to log any exceptions in your code (or send yourself an email)
* Now supports the Node.js debugger (and hence also the amazing https://github.com/dannycoates/node-inspector). Just type `debug` before the command you wish to run (e.g. `socketstream debug server`). Most useful in single-process mode
* Now uses latest 'zqm' 1.0.2 `npm` when you choose to install ZeroMQ
* SS.users now only loads if the users online functionality is enabled
* ZeroMQ now only uses TCP sockets by default. IPC sockets were too confusing
* Updated many `npm` dependencies with latest versions
* Likely to be the last commit before 0.2.0 is released before the end of the month. Please report any bugs


0.2 beta / 2011-08-20
=====================

* After much experimentation, session data is now cached on front end servers (synced over ZeroMQ) as well as in Redis (allowing your session to hop from one front end server to another). This has brought us the following benefits:
* -  dramatically reduces the load on Redis, increasing response times
* -  gives you access to all session data within server-side events
* -  allows the exports.authentication = true check to function correctly
* -  best of all: allows us to bring back the @session variable without needing to use nasty callbacks! (@getSession will be removed in 0.3)
* Introduced new @request object available to all /app/server methods to access request meta data. See /doc/guide/the_request_object.md
* -  @request.post now returns a new object allowing you to access any POST data sent to the HTTP API
* -  @request.origin shows where the request originated from (currently 'socketio' or 'api')
* You may now optionally rename the @session, @request and @user reserved variables using SS.config.reserved_vars
* Added many more optional callbacks to session methods (such as session.setUserId) to aid fast automated testing
* Moved and updated the 'Session' section from README to its own page in the emerging new user guide /doc/guide/sessions.md
* Documented storing custom session attributes inside @session.attributes in /doc/guide/sessions.md
* Changed reset.css to normalize.css when making a new project (https://raw.github.com/necolas/normalize.css/master/normalize.css)
* Off-loaded much more work onto Socket.IO 0.7, simplifying SocketStream as a result
* Bugfix: 'socketstream console' no longer responds to incoming ZeroMQ messages
* The /config/http.coffee file is now optional
* Started a new page in /doc/guide/rpc_spec.md describing the internal JSON RPC protocol we use
* Internal JSON RPC calls are now versioned to support staggered upgrades in the futures
* Auto browser reload when in development mode is now more reliable but not yet perfect, hence off by default
* Socket.IO server and client upgraded to latest 0.7.9
* File names inside /app/client can now contain dashes/hyphens

0.2.0 is almost complete now. Just some more work left to do around ZeroMQ, Node 0.5 and Authentication. If you don't mind living on the edge, now is a good time to start using 0.2 with your projects. Please let us know if you spot any bugs.


0.2 preview / 2011-08-10
========================

* /app/server methods can now take multiple arguments, though the HTTP API continues to pass multiple params to the first arg as an object
* Changed error messages you get when passing incorrect arguments (and written specs for this in an external project for now)
* Socket.IO server upgraded to 0.7.8
* Socket.IO client upgraded to 0.7.5 (latest). Minified version included for the first time
* Now loads SS.events (server-side) before /app/server code so you may emit your own custom events
* Now works properly with Subversion (no longer tries to load .svn dirs into the API tree)


0.2 preview / 2011-08-07
========================

Major changes since 0.1. See full announcement here: https://github.com/socketstream/socketstream/blob/master/doc/annoucements/0.2.md

All archived history in the 0.1 branch: https://github.com/socketstream/socketstream/blob/0.1/HISTORY.md
