# End-to-end developer experience

## Tilt

You will find the hands-on [here](https://github.com/windmilleng/tilt/tree/master/integration/oneup).

This is an advanced Tiltfiles which uses live updates as well as ACR for builds:

```
# -*- mode: Python -*-

include('../Tiltfile')

# add your registry
default_registry(read_json('tilt_option.json', {})
  .get('default_registry', 'nicoacr1.azurecr.io'))

# add your k8s context
allow_k8s_contexts('aks-test-01')

# add your registry
custom_build(
  'oneup',
  'az acr build -r nicoacr1 -t $EXPECTED_REF .',
  ['.'],
  live_update=[
    fall_back_on(['package.json', 'package-lock.json']),
    sync('.', '/app'),
  ],
  skips_local_docker=True,
)

k8s_yaml('oneup.yaml')
k8s_resource('oneup', port_forwards=8100)
```

## Telepresence

Send a HTTP query to Kubernetes Service called 'myservice' listening on port 8080:

```
telepresence --run curl http://myservice:8080/
```

Replace an existing Deployment 'myserver' listening on port 9090 with a local process listening on port 9090:

```
telepresence --swap-deployment myserver --expose 9090 --run python3 -m http.server 9090
```

Run a Docker container instead of a local process:

```
telepresence --swap-deployment myserver --expose 80 --docker-run -i -t nginx:latest
```
