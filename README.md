# Artillery Load Test Example

## Installation

```
npm install -g artillery@latest
```

## Writing a test Script
### Load Phases
Next, you'll need to determine the load phases for the test by setting config.phases. For each load phase, you'll define how many virtual users you want to generate and how frequently you want this traffic to send requests to your backend. You can set up one or more load phases, each with a different number of users and duration.

For this performance test, we'll start define three load phases:

```
config:
  target: "https://example.com/api"
  phases:
    - duration: 60
      arrivalRate: 5
      name: Warm up
    - duration: 120
      arrivalRate: 5
      rampTo: 50
      name: Ramp up load
    - duration: 600
      arrivalRate: 50
      name: Sustained load
```

The first phase is a slow ramp-up phase to warm up the backend. This phase will send five virtual users to your backend every second for 60 seconds.

The second phase that follows will start with five virtual users and gradually send more users every second for the next two minutes, peaking at 50 virtual users at the end of the phase.

The final phase simulates a sustained spike of 50 virtual users every second for the next ten minutes. This phase is meant to stress test your backend to check the system's sustainability over a more extended period.

[Source](https://www.artillery.io/docs/guides/getting-started/writing-your-first-test)

## Run a test locally

```
artillery run scripts/simple.yml
```

## Run a test from AWS Lambda

```
AWS_SDK_LOAD_CONFIG=1 artillery run --platform aws:lambda --platform-opt region=eu-central-1 --count 25 scripts/simple.yml
```

### Save report as JSON
```
AWS_SDK_LOAD_CONFIG=1 artillery run --platform aws:lambda --platform-opt region=eu-central-1 --count 25 --output test-run-report.json scripts/simple.yml
```