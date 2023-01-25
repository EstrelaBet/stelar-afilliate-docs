# Stelar Afilliate API

## Create an user

To make any requests, you should first of all create your own user. To create your user, you should call the following route.

```
curl --location --request POST '{{STELAR_API}}/user' \
--header 'Content-Type: application/json' \
--data-raw '{
    "firstName": "Robert",
    "lastName": "Cunha",
    "email": "robert.cunha@estrelabet.com",
    "password": "testePassword"
}'
```

This will allow you to auth and get a token to make other requests.

## Authentication

After the user was created, you can authenticate on the system. Here is no need for the x-api-key.

```
curl --location -g --request POST '{{STELAR_API}}/auth' \
	--header 'Content-Type: application/json' \
	--data-raw '{
			"email": "robert.cunha@estrelabet.com",
			"password": "testePassword"
	}'
```

When you do the request, you should receive a response like this.

```
{
    "data": {
        "bearerToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvYmVydC5jdW5oYUBlc3RyZWxhYmV0LmNvbSIsImRhdGUiOjE2NzI3NjEzMzg5ODEsImlhdCI6MTY3Mjc2MTMzOCwiZXhwIjoxNjcyODQ3NzM4fQ.7cBFFxZuS0cum7f8hzBSMgEZ81-pMRrHJzSoTaH3WFk",
        "expiresIn": 86400
    },
    "message": null,
    "statusCode": 200
}
```

This token is valid for 86400 seconds, wich means one day. After it, you shall authenticante again to generate another bearer token. This token is used to list estelar rounds.

## List rounds

You can list rounds and make some filters. Use the bearer token that the auth route created. On the Authorization header, put the word Bearer and paste the token.

```
curl --location --request GET '{{STELAR_API}}/round' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvYmVydC5jdW5oYUBlc3RyZWxhYmV0LmNvbSIsImRhdGUiOjE2NzI3NjEzMzg5ODEsImlhdCI6MTY3Mjc2MTMzOCwiZXhwIjoxNjcyODQ3NzM4fQ.7cBFFxZuS0cum7f8hzBSMgEZ81-pMRrHJzSoTaH3WFk'
```

The response will be an array with info about the rounds like the example bellow.

```
{
    "data": [
        {
            "_id": "63d01303789fc0fb3e74336e",
            "roundStart": "2023-01-05T13:46:27.787Z",
            "roundEnd": "2023-01-05T13:46:27.787Z",
            "multiplier": 3
        }
    ],
    "message": null,
    "statusCode": 200
}
```

### Query Parameters

The query parameters are optional params that you can use to filter the rounds the way you want. See the description of wich one to know how to filter it in the way you need.

> page: The page you want to get. The first page is 0.

> limit: How many items you want to get. The skip parameter will be calculated from this.

> query: A JSON object that can be used to filter items according to his attributes. Uses the same filters that MongoDb supports.

> sort: A JSON object that can be used to sort items according to his attributes. 1 is ascending and -1 is descending.

### Examples with Query Parameters

```
curl --location --request GET '{{STELAR_API}}/round?page=0&limit=10&query={"multiplier":3}&sort={"roundId":-1}' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvYmVydC5jdW5oYUBlc3RyZWxhYmV0LmNvbSIsImRhdGUiOjE2NzI3NjEzMzg5ODEsImlhdCI6MTY3Mjc2MTMzOCwiZXhwIjoxNjcyODQ3NzM4fQ.7cBFFxZuS0cum7f8hzBSMgEZ81-pMRrHJzSoTaH3WFk'
```
