# Vector on Cloud Run (pubsub to slack)

## deploy to Cloud Run

1. deploy App

```
export VECTOR_CONFIG_URL=""
export SLACK_WEBHOOK=""
export RUN_INVOKER_SERVICE_ACCOUNT=""

gcloud run deploy vector-to-slack --image=gcr.io/nakatanakatana/vector:0.15.0-debian-remoteconfig \
--platform managed \
--no-allow-unauthenticated \
--ingress internal \
--service-account $RUN_INVOKER_SERVICE_ACCOUNT \
--port 8080 \
--set-env-vars "\
VECTOR_CONFIG_URL=$VECTOR_CONFIG_URL,\
SLACK_WEBHOOK=$SLACK_WEBHOOK"
```

2. create pubsub / subscription
   * see https://cloud.google.com/run/docs/triggering/pubsub-push

## debug

```
docker run -v $PWD/vector.toml:/etc/vector/vector.toml:ro -p 8080:8080 -e "SLACK_WEBHOOK=$SLACK_WEBHOOK" -e timberio/vector:0.15.0-debian
```
