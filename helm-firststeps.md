# Application Deployment with Helm

> Make sure to run Helm 3.x. Helm 2.x commands will be slightly different.

## Get sample code

Go ahead an clone the below repo. It contains an sample web application as well as a Helm chart.

```
git clone https://github.com/whiteducksoftware/sample-mvc.git
```

## Deploy your first Helm release

You will find the Helm chart in the `chart` folder. Get a brief overview of the files before you begin with this hands-on.

# Application deployment

Install the Helm chart using the Helm 3.x cli:

```
helm install sample-mvc . -f values.yaml
```

After the deplyoment is done you can review the deployed resources. No it's time to customize the values ad change some values (you can change the custom message or expose the app with en ingress definition).
Upgrade your deployment with:

```
helm upgrade sample-mvc . -f values.yaml
```

