# express-middleware-openapi-example

## Usage

Setup: Install dependencies

```shell
# clone this repo, then run
npm install

# openapi middleware is under active development, run the following to get the latest
npm uninstall express-middleware-openapi && npm install express-middleware-openapi
```

Start the Api server

```shell
npm start
```

## Try it

The following examples submit invalid response. [express-middleware-openapi](https://github.com/cdimascio/express-middleware-openapi) automatically validates the request given an openapi specification. In this case it uses [this spec](openapi.yaml)

### Validate a query parameter with a value constraint

```shell
curl http://localhost:3000/v1/pets/as |jq
{
  "errors": [
    {
      "path": "id",
      "errorCode": "type.openapi.validation",
      "message": "should be integer",
      "location": "path"
    }
  ]
}
```

### Validate a query parameter with a range constraint

```shell
curl http://localhost:3000/v1/pets?limit=1 |jq
{
  "errors": [
    {
      "path": "limit",
      "errorCode": "minimum.openapi.validation",
      "message": "should be >= 5",
      "location": "query"
    },
    {
      "path": "test",
      "errorCode": "required.openapi.validation",
      "message": "should have required property 'test'",
      "location": "query"
    }
  ]
}
```

### Validate the query parameter's value type

```shell
curl --request POST \
  --url http://localhost:3000/v1/pets \
  --header 'content-type: application/xml' \
  --data '{
        "name": "test"
}' |jq
{
  "errors": [
    {
      "message": "Unsupported Content-Type application/xml"
    }
  ]
}
```

### Validate a POST body to ensure required parameters are present

```shell
λ  my-test curl --request POST \
  --url http://localhost:3000/v1/pets \
  --header 'content-type: application/json' \
  --data '{
}'|jq
  "errors": [
    {
      "path": "name",
      "errorCode": "required.openapi.validation",
      "message": "should have required property 'name'",
      "location": "body"
    }
  ]
}
```

### ...and much more. Try it out!
