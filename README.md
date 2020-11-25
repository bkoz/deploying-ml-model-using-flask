# deploying-ml-model-using-flask
Deploying Machine Learning Project on Youtube Spam comment Detector Using Flask

Tests using `curl`

```
$ curl -d comment="You've won" -X POST http://127.0.0.1:5000/predict
$ curl -d comment="Bob" -X POST http://127.0.0.1:5000/predict
```
