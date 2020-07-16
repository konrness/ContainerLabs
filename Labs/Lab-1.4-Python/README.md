# AppDynamics Python Agent 
## Example of Installation in Kubernetes Using InitContainers

The benefit of this agent instrumentation technique is there is no need to modify your app image, or install any software on your k8s nodes.

## Explanation of Example

`python-app.yaml` is the original deployment manifest.

`python-app-instrumented.yaml` is a copy of python-app.yaml with the changes necessary to instrument with AppDynamics

1. Create a Python Agent image
See the [/agent](agent/), which contains the files and Dockerfile necessary to generate a Python agent image suitable for an InitContainer.

1. Add the `agent-startup-config.yaml` file, and edit it for your specific controller configuration

1. Edit the app deployment manifest. Reference `python-app-instrumented.yaml`, and note the modifications made from lines 31 to 62 to:
  * Override the app container's entrypoint to install and start up the AppD agent
  * Set environment variables for the original ENTRYPOINT of the app image, and configure the AppDynamics agent
  * Pull global agent configuration from `agent-startup-config` config map
  * The `appd-agent-python` volume mount, which contains the Python agent files
  * InitContainer which contains the necessary agent files and copies them into the `appd-agent-python` volume mount

1. Run the following commands to setup the Kubernets namespace, set the AppDynamics account key secret, and apply all of the files in this directory:

    kubectl create namespace python-test
    kubectl create sa -n python-test appd-account
    kubectl create secret -n python-test generic appd-secret --from-literal=appd-key=myaccountkey
    kubectl apply -n python-test -f python-ingress.yaml -f agent-startup-config.yaml -f python-app-instrumented.yaml
