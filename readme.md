# recipe-app-api
Recipe API Project

## Local setup
- `docker build .`
- `docker compose build`


- linting is running a tool that checks your code formatting
- highlights errors, typos, formatting issues
- install flake8 package
- run it through docker compose
  - `docker compose run --rm app sh -c "flake8"`
- list of lint errors
- go from bottom up, So that the line numbers in the linting output are not affected
- Django test suite
- setup tests per django app
- run tests through docker compose
- docker compose run --rm app sh -c "python manage.py test"
- only run developement.dev.txt only on our local development server

- `docker compose run --rm app sh -c "flake8"`
- `docker compose run --rm app sh -c "django-admin startproject app .`
- `docker compose up`
- browser url 127.0.0.1:8000
- Ctrl-c to stop server

## 22. What is github actions
- automation tool
- similar to travis-ci gitlab ci/cd jenkins
- run jobs anytime your code changes
- handle deployment, to handle code linting run unit tests
- push trigger
- trigger push to github -> run unit tests -> result success/fail
- --- signifiies that the file is yaml format
```
on: [push]
```
- run something when a commit is pushed to the repository
- the trigger


- github actions
  - actions are basically just a docker container and a docker configuration that's set up to perform a certain task
  - examples
```
- name: Login to Docker Hub
  uses: docker/login-action@v1
  with:
    username: ${{ secrets.DOcKERHUB_USER }}
    password: ${{ secrets.DOCKERHUB_TOKEN }}
```
  - create actions or reuse actions created for particular jobs
  - action is basically just a docker container and a docker configuration that's setup to perform a certain task
  - eg.docker/login-action@v1
  - officially provided in the docker repository
  - next step checkout
  - executed in order
  - what the checkout does
    - provided by github automatically
    - checks code out inside action jobs
    - by default code is not checked out inside the job that we're running
    - sometimes don;t need to checkout code
  - with is for variables

- dockerhub token
- one token per service do not share it with multiple services
- 

