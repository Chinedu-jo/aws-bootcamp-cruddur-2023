# Week 3 — Decentralized Authentication

## User Pool Creation

![initial page](images/)

![Setup page1](images/)

![Setup page2](images/)

![Setup page3](images/)

![Setup page4](images/)

![Setup page5](images/)

![Setup page6](images/)

![Setup page7](images/)

![Setup page8](images/)

![Setup page9](images/)


## AWS Amplify

### Install AWS Amplify Dependencies

```sh
cd frontend-react-js
npm i aws-amplify --save
```

Confirm that the dependency has been added to `package.json`

![Amplify confirmation](images/)

### Configure AWS Amplify

In `app.py` add the necessary libraries

```py
import { Amplify } from 'aws-amplify';

Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_identity_pool_id": process.env.REACT_APP_AWS_COGNITO_IDENTITY_POOL_ID,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_APP_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_AWS_USER_POOLS_WEB_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});
```
#### Add env variables

Add the `env var` to `frontend` service of `docker-compose.yml` file

```yml
REACT_APP_AWS_PROJECT_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_COGNITO_REGION: "${AWS_DEFAULT_REGION}"
REACT_APP_AWS_USER_POOLS_ID: "us-east-1_NtP7bO0hz"
REACT_APP_CLIENT_ID: "60lpcnerjqe6pjqg54gmccjd7i"
```

Retrieving AWS_USER_POOLS_ID
![User Pool ID](images/user-pool-id.png)

Retrieving CLIENT_ID
![Client ID](images/client-id.png)


