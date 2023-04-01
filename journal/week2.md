# Week 2 — Distributed Tracing HomeWork


## Summary

-Use Open Telemetry (OTEL) to instrument backend flask application

-Use Honeycomb.io as provider for OTEL and explore traces with queries
![HoneyComb](https://github.com/Amarapk67/aws-bootcamp-cruddur-2023/blob/main/assests/HoneyComb.png)

-Instrument AWS X-Ray into backend flask application
![XRAY](https://github.com/Amarapk67/aws-bootcamp-cruddur-2023/blob/main/assests/xRay.png)

-Configure and provision X-Ray daemon within docker-compose to send data back to X-Ray API

-Observe X-Ray traces within the AWS Console
![XRAY](https://github.com/Amarapk67/aws-bootcamp-cruddur-2023/blob/main/assests/xRay.png)

-Integrate Rollbar for Error Logging and observe errors
![ROLLBAR](https://github.com/Amarapk67/aws-bootcamp-cruddur-2023/blob/main/assests/Rollbar.png)

-Install WatchTower and write a custom logger to send application log data to CloudWatch Log group
![CloudWatch Log](https://github.com/Amarapk67/aws-bootcamp-cruddur-2023/blob/main/assests/cloudwatch%20log%20on%20the%20backend%20flaskapp.png)




## HoneyComb

When creating a new dataset in Honeycomb it will provide all these installation insturctions



We'll add the following files to our `requirements.txt`

```
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests
```

We'll install these dependencies:

```sh
pip install -r requirements.txt
```

Add to the `app.py`

```py
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
```


```py
# Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)
```

```py
# Initialize automatic instrumentation with Flask
app = Flask(__name__)
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```

Add teh following Env Vars to `backend-flask` in docker compose

```yml
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
OTEL_SERVICE_NAME: "${HONEYCOMB_SERVICE_NAME}"
```

You'll need to grab the API key from your honeycomb account:

```sh
export HONEYCOMB_API_KEY=""
export HONEYCOMB_SERVICE_NAME="Cruddur"
gp env HONEYCOMB_API_KEY=""
gp env HONEYCOMB_SERVICE_NAME="Cruddur"
```

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












