# Week 2 — Distributed Tracing

## Summary

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

## Rollbar


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












