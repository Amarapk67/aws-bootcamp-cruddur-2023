# Week 2 — Distributed Tracing HomeWork


Instrument our backend flask application to use Open Telemetry (OTEL) with
Honeycomb.io as the provider
Run queries to explore traces within Honeycomb.io
Instrument AWS X-Ray into backend flask application
Configure and provision X-Ray daemon within docker-compose and send data back to X-Ray API
Observe X-Ray traces within the AWS Console
Integrate Rollbar for Error Logging
Trigger an error an observe an error with Rollbar
Install WatchTower and write a custom logger to send application log data to - CloudWatch Log group




# Week 2 — Distributed Tracing

## Summary

-Use Open Telemetry (OTEL) to instrument backend flask application

-Use Honeycomb.io as provider for OTEL and explore traces with queries

-Instrument AWS X-Ray into backend flask application

-Configure and provision X-Ray daemon within docker-compose to send data back to X-Ray API

-Observe X-Ray traces within the AWS Console

-Integrate Rollbar for Error Logging and observe errors

-Install WatchTower and write a custom logger to send application log data to CloudWatch Log group

## HoneyComb


## X-Ray

### Instrument AWS X-Ray for Flask

```sh
export AWS_REGION="ca-central-1"
gp env AWS_REGION="ca-central-1"
```

Add to the `requirements.txt`

```py
aws-xray-sdk
```

Install pythonpendencies

```sh
pip install -r requirements.txt
```

Install pythonpendencies

```sh
pip install -r requirements.txt
```
Add to `app.py`

```py
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.ext.flask.middleware import XRayMiddleware

xray_url = os.getenv("AWS_XRAY_URL")
xray_recorder.configure(service='Cruddur', dynamic_naming=xray_url)
XRayMiddleware(app, xray_recorder)
```

### Setup AWS X-Ray Resources



## CloudWatch Logs


Add to the `requirements.txt`

```
watchtower
```

```sh
pip install -r requirements.txt
```


In `app.py`

```
import watchtower
import logging
from time import strftime
```

```py
# Configuring Logger to Use CloudWatch
LOGGER = logging.getLogger(__name__)
LOGGER.setLevel(logging.DEBUG)
console_handler = logging.StreamHandler()
cw_handler = watchtower.CloudWatchLogHandler(log_group='cruddur')
LOGGER.addHandler(console_handler)
LOGGER.addHandler(cw_handler)
LOGGER.info("some message")
```

```py
@app.after_request
def after_request(response):
    timestamp = strftime('[%Y-%b-%d %H:%M]')
    LOGGER.error('%s %s %s %s %s %s', timestamp, request.remote_addr, request.method, request.scheme, request.full_path, response.status)
    return response
```

We'll log something in an API endpoint
```py
LOGGER.info('Hello Cloudwatch! from  /api/activities/home')
```

Set the env var in your backend-flask for `docker-compose.yml`

```yml
      AWS_DEFAULT_REGION: "${AWS_DEFAULT_REGION}"
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
```

> passing AWS_REGION doesn't seems to get picked up by boto3 so pass default region instead


## Rollbar

https://rollbar.com/

Create a new project in Rollbar called `Cruddur`

Add to `requirements.txt`


```
blinker
rollbar
```

Install deps

```sh
pip install -r requirements.txt
```

We need to set our access token

```sh
export ROLLBAR_ACCESS_TOKEN=""
gp env ROLLBAR_ACCESS_TOKEN=""
```

Add to backend-flask for `docker-compose.yml`

```yml
ROLLBAR_ACCESS_TOKEN: "${ROLLBAR_ACCESS_TOKEN}"
```

Import for Rollbar

```py
import rollbar
import rollbar.contrib.flask
from flask import got_request_exception
```

```py
rollbar_access_token = os.getenv('ROLLBAR_ACCESS_TOKEN')
@app.before_first_request
def init_rollbar():
    """init rollbar module"""
    rollbar.init(
        # access token
        rollbar_access_token,
        # environment name
        'production',
        # server root directory, makes tracebacks prettier
        root=os.path.dirname(os.path.realpath(__file__)),
        # flask already sets up logging
        allow_logging_basic_config=False)

    # send exceptions from `app` to rollbar, using flask's signal system.
    got_request_exception.connect(rollbar.contrib.flask.report_exception, app)
```

We'll add an endpoint just for testing rollbar to `app.py`

```py
@app.route('/rollbar/test')
def rollbar_test():
    rollbar.report_message('Hello World!', 'warning')
    return "Hello World!"
```


[Rollbar Flask Example](https://github.com/rollbar/rollbar-flask-example/blob/master/hello.py)


# Observability VS Monitoring 

## Loggin sucks
- Time consuming
- tons of data w/o context for why of the security event
- Niddle in the haystack
- monolith vs services vs microservices
- modern apps are distributed
- many more haystacks cone and more needles
- Increase alert fatigue for soc teams and App teams (SRE, DevOps etc)

## Why Observability
- posibility for decrease alert fatigue or security operation team
- Visibility of end2end of logs, metricks and tracing
- Troubleshoot and resolve things quickly w/o too much money
- understand application health
- accelerate collaboration b/t team
- RTeduce overall operational cost
- Increase custoemr satisfaction

## Observability vs Monitoring
- Observavalities has 3 traces
-   - Metrics
-   - traces - 
-   - logs

AWS Observability services
- AWS CloudWatch Logs - uncovering emergent and unpredictable behaviours
- AWS CloudWatch Metrics - indentifying trend, mathematical medeling and prediction
- AWS Xray traces - provides viosibility into both the path traversed by a request as well as the structure




# FreeTier for Observability: Logging - AWS Bootcamp Week 2 - Honeycomb, Rollbar, AWS X-Ray and AWS Cloudwatch Logs pricing considerations
- HoneyComb - free account ( 20 million event free - under freetier)
- Rollbar - error logging service
-   freetier ( 5k event per month and 30 day retentions)
- AWS Services
-   Xray 
-     - 100,000 AWS xray traces per month
-  CloudWatch Log
-   - 10 customer metrics and alarms
-   - 5 gb of log of data inectiona nd data archiving
-   - 1, 000,000 API Request
-   - 3 dashboards with upto 50 metrics each month 
-   - to store in s3, there will be charges involved












