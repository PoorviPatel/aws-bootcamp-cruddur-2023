# Week 1 — App Containerization

### Docker Hub
- It is a container registery
- Free to use
- It helps to store images

### Create Docker file
- Create a file backend-flask/Dockerfile
    ```dockerfile
    FROM python:3.10-slim-buster

    WORKDIR /backend-flask

    COPY requirements.txt requirements.txt
    RUN pip3 install -r requirements.txt

    COPY . .

    ENV FLASK_ENV=development

    EXPOSE ${PORT}
    CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]
    ```
To install pip / requirements.txt file

```sh
pip3 install -r requirements.txt 
```

To run Python

```sh
cd backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..
```

- check the port 4567 is there and make it public
- click on the link and add /api/activities//home - (on home page you will see jason code)

To unset the exported file

```sh
unset BACKEND_URL (unset the exported file)
unset FRONTEND_URL (unset the exported file)
```

To check if the file is there is any _URL file or not

```sh
$env | grep _URL 
```

### Build Container
- docker build -t backend-flask ./backend-flask
- To check docker images
  - click on docked icon or
  - type "docker images" on terminal for more details
- To get help or understand the commands to build docker
  ```sh
  docker build --help
  ```
  
### Run Container
```sh
docker run --rm -p 4567:4567 -it backend-flask
```
- after running the container if need to kill the command use - ctrl+c
- port will stop running and when you run the command again it will start running
- When clicking on the port link it will show an error because we need to add an environment
- To add an environment
```sh
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
```
  - click on docker icon - > Container -> Running Container - > backend-flask - > right click to view logs - > right click and click on attach shell
  - To stop container select stop container

### Set environment variables
  ```sh
  docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
  ```
- To know and understand the command to run docker
  ```sh
  docker run --help
  ```

- docker ps or ps -a (to check container id or images)
- --rm (when you stop running - the container will be removed)

### Containerize frontend

```sh
cd frontend-react-js/
npm i
```

- Create docker file in frontend-react-js and copy paste content:
  ```dockerfile
    FROM node:16.18

    ENV PORT=3000

    COPY . /frontend-react-js
    WORKDIR /frontend-react-js
    RUN npm install
    EXPOSE ${PORT}
    CMD ["npm", "start"]
    ```

- Create docker-compose.yml file at the root of the project and copy paste content:
    ```yaml
    version: "3.8"
    services:
      backend-flask:
        environment:
          FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
          BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
        build: ./backend-flask
        ports:
          - "4567:4567"
        volumes:
          - ./backend-flask:/backend-flask
      frontend-react-js:
        environment:
          REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
        build: ./frontend-react-js
        ports:
          - "3000:3000"
        volumes:
          - ./frontend-react-js:/frontend-react-js

    # the name flag is a hack to change the default prepend folder
    # name when outputting the image names
    networks: 
      internal-network:
        driver: bridge
        name: cruddur
    ```

- docker compose up or docker-compose up or right click on docker-compose.yml and select compose up ( to compose docker) 


### Create Notification

Create openapi and set the paths and name

```sh
/api/activities/notifications:
    get:
      description: 'Return a feed of activity for all of those that I follow'
      tags:
        - activities
      parameters: []
      responses:
        '200':
          description: Returns an array of activities
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Activity'
```

### Define the end point

- Add the code to app.py

```py
@app.route("/api/activities/notifications", methods=['GET'])
def data_notifications():
  data = NotificationsActivities.run()
  return data, 200
```

- Add code to import services

```py
from services.notifications_activities import *
```

- Add a new file in backend-flask -> Services -> notifications_activities.py
- Add the below code to notificatios_activities.py

```py
from datetime import datetime, timedelta, timezone
class NotificationsActivities:
  def run():
    now = datetime.now(timezone.utc).astimezone()
    results = [{
      'uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
      'handle':  'coco',
      'message': 'I am white unicorn',
      'created_at': (now - timedelta(days=2)).isoformat(),
      'expires_at': (now + timedelta(days=5)).isoformat(),
      'likes_count': 5,
      'replies_count': 1,
      'reposts_count': 0,
      'replies': [{
        'uuid': '26e12864-1c26-5c3a-9658-97a10f8fea67',
        'reply_to_activity_uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
        'handle':  'worf',
        'message': 'this post has no honor!',
        'likes_count': 0,
        'replies_count': 0,
        'reposts_count': 0,
        'created_at': (now - timedelta(days=2)).isoformat()
      }],
    }
    ]
    return results
```    

### Frontend Notification endpoint

- Add the below code to Frontend-react-js  ->  src  ->  App.js

```js
import NotificationsFeedPage from './pages/NotificationsFeedPage';

  {
    path: "/notifications",
    element: <NotificationsFeedPage />
  },
```

Create new files under pages
-NotificationsFeedPages.js
-NotificationsFeedPages.css


- Add the code in NotificationsFeedPages.js
```js
import './NotificationsFeedPage.css';
import React from "react";

import DesktopNavigation  from '../components/DesktopNavigation';
import DesktopSidebar     from '../components/DesktopSidebar';
import ActivityFeed from '../components/ActivityFeed';
import ActivityForm from '../components/ActivityForm';
import ReplyForm from '../components/ReplyForm';

// [TODO] Authenication
import Cookies from 'js-cookie'

export default function NotificationsFeedPage() {
  const [activities, setActivities] = React.useState([]);
  const [popped, setPopped] = React.useState(false);
  const [poppedReply, setPoppedReply] = React.useState(false);
  const [replyActivity, setReplyActivity] = React.useState({});
  const [user, setUser] = React.useState(null);
  const dataFetchedRef = React.useRef(false);

  const loadData = async () => {
    try {
      const backend_url = `${process.env.REACT_APP_BACKEND_URL}/api/activities/notifications`
      const res = await fetch(backend_url, {
        method: "GET"
      });
      let resJson = await res.json();
      if (res.status === 200) {
        setActivities(resJson)
      } else {
        console.log(res)
      }
    } catch (err) {
      console.log(err);
    }
  };

  const checkAuth = async () => {
    console.log('checkAuth')
    // [TODO] Authenication
    if (Cookies.get('user.logged_in')) {
      setUser({
        display_name: Cookies.get('user.name'),
        handle: Cookies.get('user.username')
      })
    }
  };

  React.useEffect(()=>{
    //prevents double call
    if (dataFetchedRef.current) return;
    dataFetchedRef.current = true;

    loadData();
    checkAuth();
  }, [])

  return (
    <article>
      <DesktopNavigation user={user} active={'notifications'} setPopped={setPopped} />
      <div className='content'>
        <ActivityForm  
          popped={popped}
          setPopped={setPopped} 
          setActivities={setActivities} 
        />
        <ReplyForm 
          activity={replyActivity} 
          popped={poppedReply} 
          setPopped={setPoppedReply} 
          setActivities={setActivities} 
          activities={activities} 
        />
        <ActivityFeed 
          title="Notifications" 
          setReplyActivity={setReplyActivity} 
          setPopped={setPoppedReply} 
          activities={activities} 
        />
      </div>
      <DesktopSidebar user={user} />
    </article>
  );
}
```
  

### Adding DynamoDB Local and Postgres

Add below code to docker compose file, add below volumes:

### DynamoDB Local

```yaml
services:
  dynamodb-local:
    # https://stackoverflow.com/questions/67533058/persist-local-dynamodb-data-in-volumes-lack-permission-unable-to-open-databa
    # We needed to add user:root to get this working.
    user: root
    command: "-jar DynamoDBLocal.jar -sharedDb -dbPath ./data"
    image: "amazon/dynamodb-local:latest"
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
```

Add below code to docker compose file, add below DynamoDb local code and paste the volumes at the end of the code:

### Postgres

```yaml
services:
  db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    ports:
      - '5432:5432'
    volumes: 
      - db:/var/lib/postgresql/data
volumes:
  db:
    driver: local
```

To install the postgres client into Gitpod

```sh
  - name: postgres
    init: |
      curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc|sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
      echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
      sudo apt update
      sudo apt install -y postgresql-client-13 libpq-dev
```

To check if Postgres is working

```sh
psql --host localhost
```

To set the password for postgres

```sh
psql -upostgres --host localhost
```

## Volumes

directory volume mapping

```yaml
volumes: 
- "./docker/dynamodb:/home/dynamodblocal/data"
```

named volume mapping

```yaml
volumes: 
  - db:/var/lib/postgresql/data

volumes:
  db:
    driver: local
```

### Security

AWS Security manager -> Aws Secret manager
or
Hashicorp Vault

Login to AWS console -> Aws Secrets manager -> saves the secrets
- Choose secret type
- Enter key value
- Enter secret name
- Enter tags
- Configure automatic rotation -> Auto rotation

AWS Inspector / Clair
- To check docker container  images vulneribilities
- AWS -> Inspector 
        - 15 days trial
        - Activate inspector
        - scan -> Vulnerebility 
               -> Instance
               -> Container repository
               -> Lambda function
               -> All findings
               -> ECR
       - There are tutorials to learn
           
Snyk Container Security
- To check Vulnerebilities
- Contain test (Image file name)

Running container in AWS
- AWS ECS
- AWS EKS
- AWS Fargarte
- AWS App Runner
- AWS copilot

