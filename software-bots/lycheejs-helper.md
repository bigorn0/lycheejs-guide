
# lychee.js Helper

The `lycheejs-helper` is a low-level bash script
that integrates the typical POSIX environment with
the lychee.js World. It integrates many useful
features for both application interaction and
rapid prototyping.


## Usage

The lychee.js Helper is a bash script, so the
`lycheejs-helper` command needs to be executed within
the Terminal (a bash session).

When given no arguments, it will show you an overview
and examples on specific use cases.





The typical use cases for the `lycheejs-helper` are:

- `boot` and `unboot` of the [lychee.js Harvester](../software-bots/lycheejs-harvester.md).
- `lycheejs://` protocol interaction
- `start`, `stop`, `file` and `edit` project interaction
- `cmd`, `web` for CLI and Browser interaction
- `env:<fertilizer>` to execute code in a specific environment

It has several use cases, such as the `lycheejs://`
protocol interaction, `boot`ing and `unboot`ing
of the `lycheejs-harvester`, 
You can use it, for example, to quickly write
tests or prototypes with the lychee.js APIs
without having to create a full-blown project.


## Environment Interaction (`env:Platform`)

You can use the lychee.js Helper for Rapid Prototyping
purposes. If you want to play around with an API without
wanting to have all the lychee.js Engine Definition
overhead, you can simply create a JS file and let the
helper execute it in the target environment.

```bash
# Reports the Path to a specific Runtime
lycheejs-helper which:node
# Example output: /opt/lycheejs/bin/runtime/node/linux/x86_64/node


# Starts the File in a specific Runtime
lycheejs-helper env:node /path/to/file.js
```


**node Fertilizer Example**

Here's the `node` Fertilizer example for a quick 'n dirty
`lychee.net.Server` instance that can be executed via
`./server.js` directly (due to the shebang) or via
`lycheejs-helper env:node ./server.js`.


```javascript
#!/usr/local/bin/lycheejs-helper env:node


var _root = '/opt/lycheejs';

require(_root + '/libraries/lychee/build/node/core.js')(__dirname);
require(_root + '/libraries/lychee/build/node/dist/index.js');


(function(lychee, global) {

    console.log(lychee.ROOT.lychee);
	console.log(lychee.ROOT.project);


	lychee.inject(lychee.ENVIRONMENTS['/libraries/lychee/dist']);
	lychee.environment.setDebug(true);


	setTimeout(function() {

		var _Server = lychee.import('lychee.net.Server');
		var _JSON   = lychee.import('lychee.codec.JSON');
		var server  = new _Server({
			codec: _JSON,
//			host:  'localhost',
			port:  1337,
			type:  _Server.TYPE.WS
		});

		server.bind('connect', function(remote) {

			console.log('REMOTE CONNECTED', remote.host + ' : ' + remote.port);

		});

		server.connect();

	}.bind(this), 200);

})(lychee, typeof global !== 'undefined' ? global : this);
```


**html Fertilizer Example**

Here's the `html` Fertilizer example for a quick 'n dirty
`lychee.net.Client` instance that can be executed via
`lycheejs-helper env:html ./client.html`.


```html
<script src="file:///opt/lycheejs/libraries/lychee/build/html/core.js"></script>
<script src="file:///opt/lycheejs/libraries/lychee/build/html/dist/index.js"></script>
<script>
(function(lychee, global) {

	lychee.inject(lychee.ENVIRONMENTS['/libraries/lychee/dist']);

	setTimeout(function() {

		var _Client = lychee.import('lychee.net.Client');
		var _JSON   = lychee.import('lychee.codec.JSON');
		var client  = new _Client({
			codec: _JSON,
			host:  'localhost',
			port:  1337,
			type:  _Client.TYPE.WS
		});

		client.bind('connect', function() {
			console.log('CLIENT CONNECTED');
		});

		client.connect();

	}.bind(this), 200);

})(lychee, typeof global !== 'undefined' ? global : this);
</script>
```


## Project Interaction (`start`, `stop`, `file`, `edit`)

The lychee.js Helper can interact with the lychee.js
Projects and Libraries.

```bash
# Stops the project server
lycheejs-helper stop /projects/boilerplate;

# Starts the project server
lycheejs-helper start /projects/boilerplate;

# Opens the File Manager of the OS
lycheejs-helper file /projects/boilerplate;

# Opens the lychee.js Editor
lycheejs-helper edit /projects/boilerplate;
```


## Tool / Web Interaction (`cmd`, `web`)

The lychee.js Helper can interact with other
lycheejs tools that are installed on the system
and the Web Browser.


```bash
# Starts the lycheejs-ranger with a JSON file
lycheejs-helper cmd=lycheejs-ranger?data='{"foo":"bar"}'

# Starts the Web Browser
lycheejs-helper web https://lychee.js.org;
```
