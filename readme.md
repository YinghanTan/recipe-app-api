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



