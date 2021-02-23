# Deploying a simple sklearn model using flask on OpenShift

Deploying a Youtube Spam comment Detector

## Deploying on OpenShift4

Create a [developer sandbox account](https://developers.redhat.com/developer-sandbox).

```
$ oc login https://api.sandbox.xxxx.p1.openshiftapps.com:6443
$ oc project mylogin-dev
$ oc new-app python~https://github.com/bkoz/deploying-ml-model-using-flask.git
```

The OpenShift build will take a few minutes then it can be exposed to external traffic.

Wait for the build to finish.

```
$ oc logs bc/deploying-ml-model-using-flask --follow

...
Writing manifest to image destination
Storing signatures
Successfully pushed image-registry.openshift-image-registry.svc:5000/mylogin-dev/deploying-ml-model-using-flask@sha256:bcbf158dac0c48cf0e69a24c4686e9fa676fc2d39cb2711a4421e03029f6e083
Push successful
```

Expose the service and visit the route.

```
$ oc expose service deploying-ml-model-using-flask
$ oc get routes --output=custom-columns=HOST/PORT:.spec.host --no-headers
```

Visit the route using a web browser and make a prediction.

Tests using `curl`

```
$ curl -d comment="You've won" -X POST ${route}/predict
$ curl -d comment="Good" -X POST ${route}/predict
```
