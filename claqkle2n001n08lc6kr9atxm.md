# How to Use HTTP2 in Loopback 4 Applications?

HTTP/2 is a major revision of the HTTP network protocol mainly in highlights because it makes our applications faster. Tech giants including Google, Netflix, Twitter already use this protocol.

In this post, I'll be showing how you can utilize `spdy` npm package to run [loopback](https://loopback.io/) applications on this protocol.

Here's what you have to do in your existing app:

### Step1: Install [spdy](https://www.npmjs.com/package/spdy)

```sh
npm i spdy
```

### Step2: Configure `index.ts`

Change your main function in `src/index.ts` with this:

```ts
import spdy from "spdy";

export async function main(options: ApplicationConfig = {}) {
  
  // specify cert and key file paths for SSL
  const serverOptions: spdy.ServerOptions = {
    key: fs.readFileSync(
      path.join(__dirname, '..', 'keys', 'localhost-privkey.pem'),
    ),
    cert: fs.readFileSync(
      path.join(__dirname, '..', 'keys', 'localhost-cert.pem'),
    ),
  };

  // setting listenOnStart to false will not start the default httpServer
  options.rest.listenOnStart = false;

  // Replace YourApplication with your class
  const app = new YourApplication(options);
  await app.boot();
  await app.start();

  // create server
  const server = spdy.createServer(spdyOptions, app.requestHandler);

  // to avoid process exit on warnings
  server.on('warning', console.warn);

  server.listen(3000, () => {
    console.log('Listening on https://localhost:3000/');
  });

  return app;
}
```

All we're doing in the above code is, preventing the default http server from being started and starting the server using spdy with loopback's request handler `app.requestHandler` that will be used for all incoming request.

Check out this [pastebin](https://pastebin.com/raw/F5gD1aF2) containing entire `index.ts` after the changes.

To generate certificate and keys for localhost use:
```sh
openssl req -x509 -newkey rsa:2048 -nodes -sha256 -subj '/CN=localhost' \
  -keyout localhost-privkey.pem -out localhost-cert.pem
```

You may need to allow self-signed certificates in Chrome as well for `/explorer` to work as expected.

And that's it, you can now run your app, and enjoy the power of http2 :)