# express-openapi-validator-example

Provides a simple example demonstrating how [express-openapi-validator](https://github.com/cdimascio/express-openapi-validator) can be used to automatically validate api requests.

## Setup the example project

```shell
# 1. clone this repo
git clone https://github.com/cdimascio/express-openapi-validator-example

# 2. install dependencies
npm install
```

## Run it

Start the Api server

```shell
npm start
```

## Try it

Try the out the following requests.

The [express-openapi-validator](https://github.com/cdimascio/express-openapi-validator) automatically validates each request against an [openapi 3 specification](openapi.yaml). If a request is does not match the spec, [express-openapi-validator](https://github.com/cdimascio/express-openapi-validator) automatically returns an appropriate error response.

### Validate a query parameter with a value constraint

```shell
ccurl -s http://localhost:3000/v1/pets/as |jq
{
  "errors": [
    {
      "path": ".params.id",
      "message": "should be integer",
      "errorCode": "type.openapi.validation"
    }
  ]
}
```

### Validate a query parameter with a range constraint

```shell
curl -s http://localhost:3000/v1/pets\?limit\=1 |jq
{
  "errors": [
    {
      "path": ".query.limit",
      "message": "should be >= 5",
      "errorCode": "minimum.openapi.validation"
    },
    {
      "path": "test",
      "message": "should have required property 'test'",
      "errorCode": "required.openapi.validation"
    }
  ]
}
```

### Validate the query parameter's value type

```shell
curl -s --request POST \
  --url http://localhost:3000/v1/pets \
  --header 'content-type: application/xml' \
  --data '{
        "name": "test"
}' |jq
  "message": "unsupported media type application/xml",
  "errors": [
    {
      "path": "/v1/pets",
      "message": "unsupported media type application/xml"
    }
  ]
}
```

### Validate a POST body to ensure required parameters are present

```shell
λ  curl -s --request POST \
  --url http://localhost:3000/v1/pets \
  --header 'content-type: application/json' \
  --data '{
}'|jq
{
  "message": "request.body should have required property 'name'",
  "errors": [
    {
      "path": "name",
      "message": "should have required property 'name'",
      "errorCode": "required.openapi.validation"
    }
  ]
}
```

### File upload example

```shell
curl -XPOST http://localhost:3000/v1/pets/10/photos -F file=@app.js|jq
{
  "files_metadata": [
    {
      "originalname": "app.js",
      "encoding": "7bit",
      "mimetype": "application/octet-stream"
    }
  ]
}
```

### ...and much more. Try it out!

## Fetch the spec

curl http://localhost:3000/spec
