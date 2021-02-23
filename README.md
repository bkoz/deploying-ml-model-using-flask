# Deploying a simple ml model using flask on OpenShift

Deploying a Youtube Spam comment Detector

## Deploying on OpenShift4

```
$ oc login https://openshift.api.cluster.mydomain.com:6443
$ oc new-project my-predictor
$ oc new-app python~https://github.com/bkoz/deploying-ml-model-using-flask.git
```

The OpenShift build will take a few minutes then it can be exposed to external traffic.

Watch the build.

```
$ oc logs bc/deploying-ml-model-using-flask --follow
```

Expose the service and visit the route.

```
$ oc expose service deploying-ml-model-using-flask
$ oc get routes --output=custom-columns=HOST/PORT:.spec.host --no-headers
```

Visit the route using a web browser.

Tests using `curl`

```
$ curl -d comment="You've won" -X POST ${route}/predict
$ curl -d comment="Good" -X POST http://${route}/predict
```
