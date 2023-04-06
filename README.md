# go_serverless
test project for serverless go stack (aws)

# Description

set-up similar to what is described in https://www.softkraft.co/aws-lambda-in-golang/ and in [this tutorial](https://youtu.be/jFfo23yIWac?t=20666).

More details and generic documentation can be found [in AWS's docs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-dynamo-db.html), although that is referencing a HTTP API.

This is basically a CRUD API with Lambda and DynamoDB 
                                        ___________________________________________________________
                                       |                          AWS                              |
                         _________     |  __________     ________________________      __________  |
The set-up is basically | clients | -> | | REST API | -> | Golang Lamda Function | -> | DynamoDB | |
                        |_________|    | |__________|    |_______________________|    |__________| |
                                       |___________________________________________________________|

The golang (`amd64` arch) build  needs to be uploaded to the Lambda function and then one needs to set-up Dynamo DB with the name `go_serverless` or whatever value is specified in `main.go` by the `tableName` const.

The Lamda function needs to have the `Simple microservice permission` policy applied to it.

Then the Amazon API Gateway (REST) needs to be set-up. The result of the set-up will be a URL value that is usable by external clients to hit the Lamda function (https://ssh***********c.execute-api.eu-central-1.amazonaws.com/vlad in my case)

One can then test if this all worked with the following curl commands:

* Add new User:

curl --header "Content-Type: application/json" --request POST --data '{"email": "some.user@client.com", "firstName": "Some", "lastName": "User"}' https://ssh***********c..execute-api.eu-central-1.amazonaws.com/vlad

* GET all users

curl -X GET https://ssh***********c..execute-api.eu-central-1.amazonaws.com/vlad

* Get a single user by email

curl -X GET https://ssh***********c..execute-api.eu-central-1.amazonaws.com/vlad\?email\=some.user@client.com

* Update User

curl --header "Content-Type: application/json" --request PUT --data '{"email": "some.user@client.com", "firstName": "Other", "lastName": "User"}' https://ssh***********c..execute-api.eu-central-1.amazonaws.com/vlad

* Delete a user

curl -X DELETE https://ssh***********c..execute-api.eu-central-1.amazonaws.com/vlad\?email\=some.user@client.com



