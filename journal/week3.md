# Week 3 — Decentralized Authentication

### Cognito User Pool
AWS -> Cognito -> Create User pool -> default (Cognito user pool) -> Select Cognito User pool Signin options (Username, email) -> Next
-> Password Policy -> Select cognito defaults/customs
-> MFA -> choos according to use(Recommended MFA have charges to send sms) - >No MFA selected
-> User account recovery -> select email only -> Next

-> Self service signup
    -> Enable Self registration
    -> Allow cognito send msg to verify and confirm
          -> Send email, verify email address
    -> Verify attribute changes -> Keep original attribute value - Email address
    -> Required attributes -> select Name or you can add required attributes
    -> custom attributes ->you can choose to add custom attributes

-> Configure msg delivery
    -> You can select send email with Amazon SEs Recommonded
    -> Select send email with cognito(Only 50 email)
    -> Next

->Integrate your app
    -> User Pool name - Crudder-user-pool
    -?Never use the cognito Hosted UI for real project

-> Initial app client
    -> Public Client
    -> App client name - Crudder
    -> Client secret
        -> We are not doing any authorization so no need for that
        -> Next 
        -> Create user pool
-> Create User
    -> Select email -> add username and pass
    -> Create user

Front-react -Js -> npm i aws amplify - save
Front-react-js -> src -> App.js
Copy paste Configure Amplify

Docker Compose yml

Frontend-react-js:
  environment
  "
  "
  React.App_AWS Project Region
  "
  "
  " -user pools Id."us-east-1_8PQ4SOHbE
  App client id

  Copy Paste conditionally show components based on logged in or logged out inside Homepagefeed.js

  Profileinfo.js
      -> Replace import cookies from 'Js-cookie' with import {path} from 'aws-amplift';

  Signin page
      copy from code in vs
      change username to email

  -> To change force-change password

CLI:- 
aws cognito-idp admin-set-user-password\
    --user-pool-id <user-pool-id>\
    --username<username>\
    --password<password>\
    --permanent

-> Sign up page (copy from vs code)
   confirmation page (copy from vs code)

-> Homepagefeedpage.js( Copy paste)

-> Requirements.txt
    -> Add ->Flask-Awscognito
    -> cd backend-flask
    -> pip install -r requirements.txt

    cognito_jwt_token.py
    docker-compose.yml
    app.py
    homeactivities.js
    profileinfo.js
    
    
    
  
    

```sh
