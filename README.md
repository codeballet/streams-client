# Streams app

The Streams app streams video, similar to services such as Twitch.

The working app consists of four parts:

1. A backend API server (see below for how to set up a dummy API).
2. The web app client 'Streams-Client' (in this repository).
3. An RTMP streaming server.
4. A software that generates the stream, such as [OBS](https://obsproject.com/).

Please note that you need to have all four pieces of software running at the same time for the app to work.

## Create a dummy API server

Create a dummy API server using the JSON Server npm package.
The server strictly follows REST API conventions.

1. In a separate directory, initialise a new NodeJS project with `npm init`.

2. Install the JSON Server package with `npm i json-server`.

3. In the root of the project, create a new file called `db.json`.

4. Enter the following in the `db.json` file:

```
{
  "streams": []
}
```

5. In the `package.json` file, create a new script with the following:

```
"start": "json-server -p 3001 -w db.json"
```

6. To start the API server, call the above script with `npm start`.

The dummy server will now list resources by sending a GET request to:

```
http://localhost:3001/streams
```

## Set up the Streams-Client

The Login / Logout functionality uses Google's Oauth service.

For the Google Oauth service to work, you need to generate a Client ID in your Google developer's console.

Once you have the Client ID, create the file `.env.local` and paste your Client ID with the below environment variable:

```
REACT_APP_GOOGLE_CLIENT_ID=yourGoogleClientID.apps.googleusercontent.com
```

To start the client, run `npm start`.

## Create the RTMP server

For a simple, local RTMP server, follow the below instructions:

1. In a separate directory, initialise a new NodeJS project with `npm init`.

2. Install the [Node-Media-Server](https://github.com/illuspas/Node-Media-Server) with `npm i node-media-server`.

3. At the root of the project, create the file `index.js`.

4. As per the documemtation of [Node-Media-Server](https://github.com/illuspas/Node-Media-Server), paste the following code into the `index.js` folder:

```
const NodeMediaServer = require('node-media-server');

const config = {
  rtmp: {
    port: 1935,
    chunk_size: 60000,
    gop_cache: true,
    ping: 30,
    ping_timeout: 60,
  },
  http: {
    port: 8000,
    allow_origin: '*',
  },
};

var nms = new NodeMediaServer(config);
nms.run();
```

5. In the `package.json` file, create the script ` "start": "node index.js"`.

6. To start the RTMP server, run `npm start`.

## Set up the OBS software

Please consult the [OBS](https://obsproject.com/) documentation for details on how to operate the software and how to set up a Stream.

### Start a new stream

You need to first create a new Stream in the web app, then you need to connect your OBS software to that web app Stream.

#### Start a new Stream in the Web App:

1. Login on the web app Stream-Client.
2. Create a new stream using the web interface.
3. Go to that new stream you just created and take note of the stream ID in the URL for the stream.

The ID of the stream is aquired by copying the last parameter of the URL of the streaming page in the Stream-Client app. For instance, if the URL for the Stream is `http://localhost:3000/streams/1`, then the ID of the stream is "1".

#### OBS software settings:

Once you have created a new Stream in the web app, go to the Settings in your OBS software. Enter the following details into the Settings of the OBS stream you want to transmit:

- Service: Custom...
- Server: rtmp://localhost/live
- Stream Key: [The ID of the stream]

## Licence

MIT
