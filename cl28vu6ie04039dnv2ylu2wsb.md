## Deploy Rust Applications To AWS Lambda

A common behavior that I witness with people making a journey toward proficiency with the Rust programming language is the desire to start using the language for everything. It's true that the language certainly has many merits and seems well adaptable to different application domains, and this reality is only being strengthened by the continuous growth in maturity of the language and its community. However, as a professional, it is still crucially important to have a good sense of what the best tool is for the given job, rather than blindly choosing your favorite. You don't want to introduce more complexity than necessary into a project and you certainly don't want to advocate for a practice that could lead your team to a dead end. Rust may be a great programming language, but one must use discretion when deciding to apply it to a problem.

Today, I don't care about being quite so professional. I'll be the first to admit that I am becoming a prime example of the kind of enthusiast I just finished describing. So in this article, I'm going to indulge that wide-eyed fascination that you and I may share for this shiny programming language. We're going to explore a domain shaped perfectly for — perhaps even intended exclusively for — interpreted runtime languages, and we're going to wedge Rust in there and make it fit too. Today, I'm going to show you an absolute minimal but complete demonstration of deploying Rust as a serverless function, specifically with AWS Lambda.

Before jumping in, it's worth mentioning that my introduction has been perhaps an exaggeration. I have made it sound as if Rust does not belong in the realm of serverless and that this is an unmarked territory, but who am I to say that Rust isn't in fact the perfect tool for serverless? To my knowledge, serverless Rust too has grown in maturity over the last couple of years. What this article is going to present is certainly not any ground-breaking discovery, but instead what I believe should be an excellent resource for people having no idea where to start for deploying Rust in AWS Lambda. So without further ado, let's go ahead and build the project.

### Prerequisites
To follow along with this tutorial, you will need the following:

- AWS Command Line Interface installed and configured with security credentials for - your AWS account/user. At the time of writing, I am using version 2.0.
- NodeJS. At the time of writing, I am using version 16.14.2.
- AWS CDK. At the time of writing, I am using version 2.20.
```
npm i -g aws-cdk
```
- Rust. At the time of writing, I am using version 1.16.

### Creating a CDK Project
The first thing we need to do is create the project. Even though this is supposed to be about our Rust lambda, CDK is going to be at the core of our project and the lambda code will exist within it. It is of course possible to structure things differently, for example by keeping the lambda code and CDK separate, but for this tutorial, we will keep things simple.

In the command line, make a directory for the new project.

```bash
mkdir tutorial-rust-lambda
cd tutorial-rust-lambda
```

Use the CDK CLI to initialize a new project. We will use Typescript.
```bash
cdk init app --language typescript
```

One extra thing I like to do in Typescript projects before getting started is to set an output directory for compiled scripts. In the `tsconfig.json` file, add to the `compilerOptions` object. This will help keep the clutter away from our source files. Also, update the `exclude` field to ignore our output directory.

```diff
20c20,21
<     "typeRoots": ["./node_modules/@types"]
--------
>     "typeRoots": ["./node_modules/@types"],
>     "outDir": "dist"
22c23
<   "exclude": ["cdk.out"]
--------
>   "exclude": ["cdk.out", "dist"]
```

Having created the project, we can now prepare for development by starting up the Typescript compiler watch agent. The project's package.json will come with an NPM run-script for this.

```bash
npm run watch
```

Finally, to work with CDK the way we intend to, you must perform what is called bootstrapping. This is because we will be packaging the lambda as an asset. You can read more about CDK bootstrapping here. Find your AWS account ID and choose an AWS region and then run the following, substituting those values:

```bash
cdk bootstrap aws://{AWS_ACCOUNT_ID}/{AWS_DEFAULT_REGION}
```

### Writing a Lambda Function in Rust
We're going to write an incredibly simple Rust program that will receive an event of an expected format (a JSON string with a "name" field) and respond with "Hello {name}". The code is nearly identical to the [basic example](https://github.com/awslabs/aws-lambda-rust-runtime/blob/main/lambda-runtime/examples/basic.rs) from the [aws-lambda-rust-runtime](https://github.com/awslabs/aws-lambda-rust-runtime) repository.

First, let's set up our Rust project within our CDK project. Again, your lambda code does not need to reside within your CDK project. If your lambda code scales into a large project you could instead package it and publish it to AWS S3 and reference that S3 object from CDK, but for this demo, we are doing things the more straightforward way.

```bash
cargo new lambda/hello
```

Next, we will add some dependencies to the project required to build our Rust code as an AWS Lambda function. Open `lambda/hello/Cargo.toml` in your editor and add the following under `[dependencies]`:

```toml
lambda_runtime = "0.3.0"
log = "0.4.14"
serde = "1.0.126"
simple_logger = "1.11.0"
tokio = "1.6.1"
```

Next, we'll write our `lambda/hello/src/main.rs`.
```rust
// This example requires the following input to succeed:
// { "name": "some name" }

use lambda_runtime::{handler_fn, Context, Error};
use log::LevelFilter;
use serde::{Deserialize, Serialize};
use simple_logger::SimpleLogger;

/// This is also a made-up example. Requests come into the runtime as unicode
/// strings in json format, which can map to any structure that implements `serde::Deserialize`
/// The runtime pays no attention to the contents of the request payload.
#[derive(Deserialize)]
struct Request {
    name: String,
}

/// This is a made-up example of what a response structure may look like.
/// There is no restriction on what it can be. The runtime requires responses
/// to be serialized into json. The runtime pays no attention
/// to the contents of the response payload.
#[derive(Serialize)]
struct Response {
    req_id: String,
    msg: String,
}

#[tokio::main]
async fn main() -> Result<(), Error> {
    // required to enable CloudWatch error logging by the runtime
    // can be replaced with any other method of initializing `log`
    SimpleLogger::new().with_level(LevelFilter::Info).init().unwrap();

    let func = handler_fn(my_handler);
    lambda_runtime::run(func).await?;
    Ok(())
}

pub(crate) async fn my_handler(event: Request, ctx: Context) -> Result<Response, Error> {
    // extract some useful info from the request
    let name = event.name;

    // prepare the response
    let resp = Response {
        req_id: ctx.request_id,
        msg: format!("Hello {}!", name),
    };

    // return `Response` (it will be serialized to JSON automatically by the runtime)
    Ok(resp)
}
```

Finally, try building with cargo to make sure everything works.
```bash
pushd lambda/hello
cargo build
popd
```

Now we can start developing the blueprint for the application's infrastructure. CDK is incredibly powerful and gives you the tools to build complex applications in AWS using high-level constructs, but for our needs, we only want to create a simple lambda function.

We'll start by installing the lambda library dependency. Keep in mind, that you probably don't need to specify the version as I am doing here, but it may help to keep all of your CDK library dependency versions aligned and avoid incompatibilities.

```bash
npm i @aws-cdk/aws-lambda
```

Next, in the file representing our CloudFormation stack `lib/tutorial-rust-lambda-stack.ts`, we're going to instantiate a lambda function.
```ts
import * as cdk from '@aws-cdk/core';
import * as lambda from '@aws-cdk/aws-lambda';

export class TutorialRustLambdaStack extends cdk.Stack {
  constructor(scope: cdk.Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    const target = 'x86_64-unknown-linux-musl';
    const hello = new lambda.Function(this, 'HelloHandler', {
      code: lambda.Code.fromAsset('lambda/hello', {
        bundling: {
          command: [
            'bash', '-c',
            `rustup target add ${target} && cargo build --release --target ${target} && cp target/${target}/release/hello /asset-output/bootstrap`
          ],
          image: cdk.DockerImage.fromRegistry('rust:1.52-slim')
        }
      }),
      functionName: 'hello',
      handler: 'main',
      runtime: lambda.Runtime.PROVIDED_AL2
    });
  }
}
```

Let's unpack some of that because there are some interesting things going on here. When we create the function, we pass on some options to configure it. We name the function with the `functionName` option, we set the handler function with the handler option (this is actually unnecessary for us because of how custom runtimes work), and we specify the runtime with the runtime option.

Typically, you might see something more specific as the runtime, like NODEJS_12_X or GO_1_X, but for a custom runtime, a runtime in which you can provide your own binary, we use the PROVIDED_AL or PROVIDED_AL2. AL here is short for Amazon Linux.

Notice the code option. This option is arguably the most important one, as it lets the developer indicate how CDK provides the actual code or binary for the function to execute. We're using the Code class' static method `fromAsset` to create a bundle with our binary. With this approach, we can specify a path to a directory or zip file. We give the path to our Rust project source root and then pass some additional options. By using the bundling option, we are telling the asset provider to create our asset with a docker container. The bundling options are somewhat powerful, allowing you to specify the image to use, docker entry point, command, volumes, and some other things. We only need to specify the image and command.

We use the official rust image. This will take additional time on your first run since the image must be pulled. For the command, we instruct the docker runtime to install a new rust target (so we can run the binary on Amazon Linux 2), to build the source, and then we copy the binary file to /asset-output/bootstrap. This output file is what the lambda custom runtime expects to exist when it is invoked.

At this point, you should be able to deploy the lambda. This command may take some time, as the docker image must be pulled (first time only), the crates.io index must be updated, and the lambda must be built (only when you make changes). There could be possible optimization for this step by removing the build process from CDK and perhaps supplying your own Dockerfile for CDK to build with the new target added, but I will leave that for you to explore. The deployment will also prompt the user about IAM policy and statement changes. You can accept these and then the deployment should complete and your lambda function will be live.

```bash
cdk deploy
```

Let's go ahead and test the lambda now. We can do this from the command line using the AWS CLI.
```bash
aws lambda invoke --function-name hello --payload '{"name":"Nick"}' output.json
```

Now if you look at the contents of output.json, you should see a nice message.
```bash
cat output.json
# {"req_id":"88226fb0-670c-4f0e-b772-7e677ccec71d","msg":"Hello Nick!"}
```

### Adapting the Lambda into an API Endpoint
To get a glimpse of just how easy CDK makes it to build and connect the components of your application, let's take this lambda one step further by giving it an actual event source. We'll tell CDK to create an API gateway that proxies all requests to the lambda, and we'll simply modify the lambda to respond with some of the data coming in from the request context.

Let's start with the lambda modification. First, we'll need to update our dependencies again inside lambda/hello/Cargo.toml. We can remove the serde dependency and add the following:
```toml
aws_lambda_events = "0.4.0"
http = "0.2.4"
```

We're integrating a nice third party library here, aws-lambda-events, which provides serializable Rust structs for the various AWS event definitions.

Now we can update our lambda code. At the top of lambda/hello/src/main.rs, at some new use statements to bring the API Gateway event definitions into scope, as well as some other things we'll need. Again, you can remove the use serde as well.

```rust
use aws_lambda_events::event::apigw::{ApiGatewayProxyRequest, ApiGatewayProxyResponse};
use aws_lambda_events::encodings::Body;
use http::header::HeaderMap;
```

Remove the struct definitions we previously had for Request and Response, and replace any references to those by ApiGatewayProxyRequest and ApiGatewayProxyResponse.

Finally, update the my_handler function to build an ApiGatewayProxyResponse to return, instead of our old Response. We'll take the URL path from the request context and return that with some text in the response body.

```bash
pub(crate) async fn my_handler(event: ApiGatewayProxyRequest, _ctx: Context) -> Result<ApiGatewayProxyResponse, Error> {
    // extract some useful info from the request
    let path = event.path.unwrap();

    // prepare the response
    let resp = ApiGatewayProxyResponse {
        status_code: 200,
        headers: HeaderMap::new(),
        multi_value_headers: HeaderMap::new(),
        body: Some(Body::Text(format!("Hello from '{}'", path))),
        is_base64_encoded: Some(false),
    };

    // return `Response` (it will be serialized to JSON automatically by the runtime)
    Ok(resp)
}
```

Moving back to Typescript land, we will need to add a new dependency to create API Gateway resources.

```bash
npm i @aws-cdk/aws-apigateway
```

We'll then import that library at the top of our lib/tutorial-rust-lambda-stack.ts.
```ts
import * as apigw from '@aws-cdk/aws-apigateway';
```
And finally, we will instantiate a new API Gateway Lambda Rest API resource after our lambda function definition. We will pass to it our lambda function as the handler.
```ts
const gw = new apigw.LambdaRestApi(this, 'HelloEndpoint', {
  handler: hello
});
```

Once again, we can now deploy our CDK application.
```bash
cdk deploy
```

And this time to test, we will copy the API Gateway endpoint URL from the output of our CDK deployment. At the very end of the CDK command line output, you should see the second last section called Outputs. In that section, there should be an output value for your URL. For example, it should look something like this:

```bash
 ✅  TutorialRustLambdaStack

Outputs:
TutorialRustLambdaStack.HelloEndpointB03699DE = https://0448ubzy8d.execute-api.ca-central-1.amazonaws.com/prod/
```

Copy that URL and then we can make a request to it with any path we want and we should receive a nice response from our lambda.

```bash
curl -sL https://0448ubzy8d.execute-api.ca-central-1.amazonaws.com/prod/the/test/path
# Hello from '/the/test/path'
```

And there we have it. With essentially one more line of code we added an API Gateway to direct traffic to our lambda, a lambda that is running our Rust code. Now go, go off and build incredible serverless application components with the power of Rust! 🦀