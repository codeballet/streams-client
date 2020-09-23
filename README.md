# Streams app

## Create dummy API server

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
