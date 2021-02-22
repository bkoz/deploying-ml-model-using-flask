# deploying-ml-model-using-flask
Deploying Machine Learning Project on Youtube Spam comment Detector Using Flask

## Deploying on OpenShift4

```
$ oc login https://openshift.api.cluster.mydomain.com:6443
$ oc new-app python~https://github.com/bkoz/deploying-ml-model-using-flask.git
```

The OpenShift build will take a few minutes.

```
$ oc expose service deploying-ml-model-using-flask
```

Visit the route using a web browser.

Tests using `curl`

```
$ curl -d comment="You've won" -X POST http://127.0.0.1:5000/predict
$ curl -d comment="Bob" -X POST http://127.0.0.1:5000/predict
```
