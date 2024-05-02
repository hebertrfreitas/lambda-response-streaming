# lambda-response-streaming

This project deploys a lambda function (and also a lambda function url) that manages communication with openAI sending prompts and delivering the reponses.
In this use case we use the [lambda response streaming feature](https://docs.aws.amazon.com/lambda/latest/dg/configuration-response-streaming.html ) connected to the [openai streaming feature](https://platform.openai.com/docs/api-reference/streaming) to deliver the response chunks

This project was created using [AWS SAM](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html).


## How to build

In my case I need to specify the manifest file manualy and build before deploy because I have additional dependencies.

```sh
sam build --manifest package.json
```

## How to deploy

### Requirements 
You need aws cli and aws sam instaled and already configured

Next, you build the application

```sh
sam build --manifest package.json
```

and then deploy on aws account

```sh
sam deploy
```

## Invoke aws function url
The lambda deploy exposes and lambda function url that can requested by curl or any other http client.

> Pay attention to the parameter -N, it's required since we are streaming the response, otherwise the curl will buffer the entire response before delivery it

### Via curl
```sh
 curl --request GET https://<url>.lambda-url.<Region>.on.aws/ --user <your_aws_acces_key_id>:<your_aws_secret_access_key> --aws-sigv4 'aws:amz:<Region>:lambda' -N
```
