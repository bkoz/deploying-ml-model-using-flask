# Deploying a simple sklearn model using flask on OpenShift

Deploying a Youtube Spam comment Detector

## Deploying on OpenShift4

Create a [developer sandbox account](https://developers.redhat.com/developer-sandbox), login to OpenShift and
create a new application using the python builder image.

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
...
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
```

Expected output
```
<!DOCTYPE html>
<html>
<head>
	<title>Result</title>
	<link rel="stylesheet" type="text/css" href="/static/css/style.css">
	<link rel="stylesheet" type="text/css" href="/static/css/bootstrap.css">
</head>
<body>
	<div class="bg-info" align="center">
	<h2>Results</h2>
	</div>
	<div align="center">

		<h2 class="text-danger">Spam</h2>

	</div>
</body>
</html>
```
```
$ curl -d comment="Good" -X POST ${route}/predict
```
```
<!DOCTYPE html>
<html>
<head>
	<title>Result</title>
	<link rel="stylesheet" type="text/css" href="/static/css/style.css">
	<link rel="stylesheet" type="text/css" href="/static/css/bootstrap.css">
</head>
<body>
	<div class="bg-info" align="center">
	<h2>Results</h2>
	</div>
	<div align="center">

		<h2 class="text-success">Not a Spam (It is Ham)</h2>

	</div>
</body>
</html>
```
