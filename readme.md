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

## django test framework
- built into django and comes out of the box
- based on the unittest library
- unittest library comes out of the box with python
- django test framework add additional features to this library
- useful when testing django projets
- test client dummy web browser that you can use to make requests to your project
- allows you to simulate authentication so you can handle authentication by overriding it for your unit tests which is useful when you're running tests because you don't always want to have to handle the log  in and registration rpocess for your tests
- simulate authentication
- comes  wtih database integration creates a temporary database it will automatically clear the data
- django rest framework adds additional testing features 
- rest api
- api test client for api requests
- directory for tests
- placeholder tests.py added to each app when you create a new app in django
- placeholder for adding tests
- create tests/ subdirectory inside your app to split tests up
- keep in mind use only tests.py or tests/ directory not both
  - tests/ directory
  - test modules  must start with test_
  - test directory must contain -__init__.py file to allow django to puck up the tests in the test run

C2-demo-code
  app_one
    __init__.py
    admin.py
    apps.py
    models.py
    tests.py
    views.py
  app_two
    tests/
      __init__.py
      test_module_one.py
      test_module_two.py
    __init__.py
    admin.py
    


### Test database
- test code that uses the DB
- test code that reads and write to database
- seperate database for your tests compared to your actual applcation don't want test data inside the real database
- auto create specifi database for testing
- run tests django clears any data for that test and you'll run the next
- any test you run in the jdango teste will hae a freh data
- happens for every tests individual
- simpletestcase
  - no database integration
  - test code that does not need database
  - non need to clearn or setup database whenever you run
  - save time
- TestCase
  - databse integration
  - test code that needs to read and write to database
  - this is used in the most cases
  - 

```python
# import the type of test with or without database
from django.test import SimpleTestCase - no database
from django.test import - database

# import the module you want to test
from app_two import views
from app_two import views

# define test class
class ViewsTests(SimpleTestCase):
  # Add test method
  # prefix methods by test_ to be picked up by test runner
  def test_make_list_unique(self):
    """Test making a list unique"
    # setup inputs
    sample_items = [1,2,2,3,4,5,5]
    # execute code to be tested
    res = views.remove_duplicates(sample_items)
    self.assertEqual(res, [1,2,3,4,5])
    
    self.assertEqual(res, [1,2,3,4,5])

```
- information when it fails

### Mocking
- override or change behaviour of dependencies for the purpose of your test
- avoid unintended side effects and consequences when you're running your test code
- isolate code being tested
- more reliable and accurate
- why use mocking
  - avoid relying on external services, cannot always guarantee that external services will be available when you run unit test which makes it fragile
  - makes tests unpredictable and inconsistent
  - avoid unintended consequences
  - accidentally sending emails
  - example -> register_user() -> create_in_db() -> send_welcome_email) -> X
  - prevent email being sent
  - ensure sent welcome _email(0 called correctly)
  - speed up tests
  - check_db() -> sleep() 
  - <-
  - continue once online
  - test and check DB funct6ionality
  - don't want to slow down with sleep()
  - replace sleep() with a mock function
- How to mock code
  - use unittest.mock
  - MagicMock/mock - Replace real objects class to replace real objects in your code
  - check whether that object was called correctly and what arguments it was called with
  - patch - overrides code4 for [mocking](mocking)
### testing api
- django rest framework api client
  - based on the django's testclient
- make requests
- check result
- override authentication on the api assuming you are already authenticated
- simplify the test because it means you don't ave to manually handle regist6ration

```python
from django.test import SimpleTestCase
from rest_framework.test import APIClient

class TestViews(SimpleTestCase):
  def test_get_greetings(self):
    """Test getting greetings"""
    client = APIClient()
    res = client.get('/greetings/')
    self.assertEqual(res.status_code, 200)
    self.assertEqual(
      res.data,
      ["Hello!", "Bonjour", "Hola"]
    )
    
```

### Testing common issues
- missing __init__.py in tests/dir
- indentation of test cases
- def method is indented nested in another def test method
- missing test prefix for method
- import error when running tests
  - import error tests module incorrectly imported
  - have both tests/directory and tests.py in my app
  - use either th test directory or the test module
- postgresql
  - popular opensource DB
  - integrates well with django
  - official django documentation
- docker compose 
  - defined with project (re-)usable
  - persistent data using volumes
  - handles network configuration
  - environment variable configuration
  - docker Compose (services)
    - Database(PostgreSQL)
      - postgres application
    - App (Django)
      - django application
    - communicate with each other to write to database
      - setup netw3ork connectivity  
```yaml
services:
  app:
    depends_on:
      - db
  db:
    image: postgres:13-alpine
```
- depends on ensures that one service starts before the other
- make sure the database service start before the app service starts
- later on in the section docker compose will then create a newwork akkkkkkkk
- aws will later l 
- neetwork automatically bettween the different services
- use db or the name of the service as the hostname
- volumes
  - persistent data
  - maps directory in container to local machine
```
db:
  image: postgres:13-alpine
  volumes:
    - dev-db-data:/var/lib/postgresql/data
volumes:
  dev-db-data:
  dev-static-data:
```
  
## Steps for configuring database
- configure django
  - tell django how to connect
- install database adaptor dependencies
  - install the tool django uses to connect
- update python requirements
- django needs to know
  - engine type of dtabase
  - hostname (ip or domain name for database)
  - port 5432
  - databvase Name - name of database inside postgres mysql sqlserver
  - username
  - password
  - authenticate with database
  - defined in settings.py
```
DATABASES = {
  'default': {
    'ENGINE': 'django.db.backends.postgresql',
    'HOST': os.environm.get('DB_HOST'),
    'USER': os.environ.get('DB_USER'),
    'PASSWORD': os.environ.get('DB_PASS')
  }
}
```
- environment variables
  - Pull config values from environment variables
  - easily passed to docker containers - standard way
  - used in local dev or prod
  - single place to configure project
  - do this in our deployed application
  - os.environ.get('DB_HOST')
- psycopg2
- package need in order for django to connect to our database
- database adapter
- most popular postgressql adaptor for python

- installation options
  - not the most simple pacakge in the world couple of different options 
  - install options
    - ok for development
    - not good for production
    - prepackage binary file
  - psycdopg2
  - psycdopg2
    - compiles from source
    - in the backgroud it is being compiled speifically for linu8x and te specific organizaeiot i i ffonld
    - psycopg2-binary ok for dev not good for prod
      - prepackage binary file not optimized for OS
      - performance drawbacks
    - psycopg2 compiles from source
      - without binary
      - installed and compiled from source
      - compiled for the specific operating system it is running on
      - need dependencies to compile
      - need packages
      - on docker it is easy
    - c compiler
    - python3-dev
    - libpq-dev
    - alpine
      - postgresql-client
      - build-base
      - postgresql-dev
      - musl-dev
    - best practise for docker is to cleanup any build dependencies
      - these packages are only needed for installing the packages and not needed for running the packages when running the actual app
      - customise dockerfile to cleanup
    - 
### problems with docker compose
- race conditon for db
- depends_on ensures service starts does not ensure it is working or fully started
  - doesn't ensure application running on service has started as well
  - service has started but application may not be running yet
  - service computer, application on laptop may not be running yet
  - postgres application
- docker services timeline
- database
- django app
- make django wait for db
- custom django management command
  - check for database availability
  - continue when database is ready
- new timeline
- service starting 
  django service starting 
  postgresttarting 
  app starting 
  wait_for5_db
- check if postgres is ready
- database is ready
- continue with django app execution
- running docker compose locally
- running on deployed environment
- depends on how fast is postgres
- 
  
