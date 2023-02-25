# Week 1 — App Containerization

Docker Hub
- It is a container registery
- Free to use
- It helps to store images

Create Docker file
- ls (to check the list)
- cd backend-flask (to go into backend-flask)
- pip3 install -r requirements.txt (to install pip)
- python3 -m flask run --host=0.0.0.0 --port=4567 (run host)
- check the port 4567 is there and make it public
- export FRONTEND_URL="*" (export frontend file)
- export BACKEND_URL="*" (export backend file)
- click on the link and add /api/activities//home - (on home page you will see jason code)
- unset BACKEND_URL (unset the exported file)
- unset FRONTEND_URL (unset the exported file)
- $env | grep _URL (This command is used to check if there is any _URL file or not)
- cd .. ( to go into main directory)

Build Container
- docker build -t backend-flask ./backend-flask
- To check docker images
  - click on docked icon or
  - type "docker images" on terminal for more details
- To get help or understand the commands to build docker
  - docker build --help

Run Container
- docker run --rm -p 4567:4567 -it backend-flask
- after running the container if need to kill the command use - ctrl+c
- port will stop running and when you run the command again it will start running
- When clicking on the port link it will show an error because we need to add an environment
- To add an environment
  - FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
  - click on docker icon - > Container -> Running Container - > backend-flask - > right click to view logs - > right click and click on attach shell
  - To stop container select stop container
 
Set environment variables
- To know and understand the command to run docker
  - docker run --help
- docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
- docker ps or ps -a (to check container id or images)
- --rm (when you stop running - the container will be removed)

Containerize frontend
- cd frontend-react-js
- npm i
- Create docker file in frontend-react-js and copy paste content
- Create docker-compose.yml file at the root of the project and copy paste content
- docker compose up or docker-compose up or right click on docker-compose.yml and select compose up ( to compose docker) 




