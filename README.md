# webpack-udev-server
A universal dev-server for [webpack].

For universal javascript applications, this server adds the ability to hot-reload the rendering code on the server-side in addition to `webpack-dev-server`'s ability to do the same for the client-side.

## Usage

Some runtime changes to your code are required for using the universal dev server.

In your `server.js` file the `assets` event is fired whenever the location of and information about the client side assets change. This should be used to adjust the URL in your `<script>` tags.

```javascript
process.on('assets', ([url, stats]) => {
	// Do stuff with asset updates
});

// Listen.
server.listen(process.env['PORT']);
```

Your server webpack configuration file should include a reference to the hot runtime for the dev server.

```javascript
{
	entry: {
		server: [
			'webpack-udev-server/hot/dev-server'
			'webpack/hot/signal',
			'./server.js'
		]
	}
}
```

All that's left is to run the dev server using both the server and client webpack configuration files.

```sh
# Specify the path to the server and client webpack config files.
webpack-udev-server --config client.js --config server.js
```

You can also run the server programmatically.

```javascript
import udev from 'webpack-udev-server';

import assets from 'some-webpack-client-config.js';
import render from 'some-webpack-server-config.js';

const server = udev.createServer({
	assets: assets,
	render: render
});

server.listen(8080, () => {
	console.log('dev server ready: ', this.address().port);
});
```


[webpack]: http://www.google.com
