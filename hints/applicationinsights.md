# Monitoring and understanding application behaviour

Application Insights is an extensible Application Performance Management (APM) service for web developers on multiple platforms. Use it to monitor your live web application. It will automatically detect performance anomalies

## Creating application insights

After you have set up Application Insights on your project, telemetry data about your app's performance and usage will appear in your project's Application Insights resource in the Azure portal.

<https://docs.microsoft.com/en-us/azure/application-insights/app-insights-create-new-resource>

## Providing the Instrumentation Key to the App

First add the secret, then reference it in the yaml files.

Secret deployment:

```sh
kubectl create secret generic appinsightsecret --from-literal appinsightskey=914223de-e628-4e51-918e-622b105018ff
```

Redeploy `frontend-pod.yaml`:

```yaml
apiVersion: "v1"
kind: Pod
metadata:
  name: calcfrontend
  labels:
    name: calcfrontend
    app: calculator
spec:
  containers:
    - name: calcfrontend
      image: labuser01.azurecr.io/js-calc-frontend:latest
      ports:
        - containerPort: 80
          name: http
          protocol: TCP
      env:
        - name: "ENDPOINT"
          value: "calcbackendsvc"
        - name: "INSTRUMENTATIONKEY"
          valueFrom:
            secretKeyRef:
              name: appinsightsecret
              key: appinsightskey
        - name: "PORT"
          value: "80"
```

Redeploy `backend-pod.yaml`:

```yaml
apiVersion: "v1"
kind: Pod
metadata:
  name: calcbackend
  labels:
    name: calcbackend
    app: calculator
spec:
  containers:
    - name: calcbackend
      image: labuser01.azurecr.io/js-calc-backend:latest
      ports:
        - containerPort: 80
          name: http
          protocol: TCP
      env:
        - name: "ENDPOINT"
          value: "calcbackendsvc"
        - name: "INSTRUMENTATIONKEY"
          valueFrom:
            secretKeyRef:
              name: appinsightsecret
              key: appinsightskey
        - name: "PORT"
          value: "80"
```

## How do I monitor the performance of by app?

Azure Application Insights can alert you to changes in performance or usage metrics in your web app.

<https://docs.microsoft.com/en-us/azure/application-insights/app-insights-web-monitor-performance>

## How do I monitor the availability of my website?

After you've deployed your web app or web site to any server, you can set up tests to monitor its availability and responsiveness.

<https://docs.microsoft.com/en-us/azure/application-insights/app-insights-how-do-i>