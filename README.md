<!-- Delete on release branches -->

<p align="center">
<img src='https://s3-us-west-2.amazonaws.com/cortex-public/logo.png' height='75'>
<br><br>
<b>A production grade deployment platform for machine learning pipelines</b><br>
<a href="https://docs.cortex.dev/install">install</a> • <a href="https://docs.cortex.dev">documentation</a> • <a href="https://github.com/cortexlabs/cortex/tree/0.20/examples">examples</a> • <a href="https://angel.co/cortex-labs-inc/jobs">we're hiring</a> • <a href="https://gitter.im/cortexlabs/cortex">chat with us</a>
</p>

<!-- Delete on release branches -->
<!-- CORTEX_VERSION_README_MINOR -->


## Deploy machine learning to production

Cortex makes it easy to run inference at production scale. It abstracts all of the underlying infrastructure needed for running machine learning in production, providing a set of high-level tools for deploying, managing, and scaling inference pipelines.

Some of Cortex's core features include:

### Cluster management
The Cortex cluster is an easy-to-use, highly configurable Kubernetes cluster provisioned for inference. The Cortex cluster runs on your AWS account, and enables you to:

- Run inference on EC2 instances
- Save money by running on spot instances
- Specify compute resources
- Configure cluster-wide networking and autoscaling

```bash
$ cortex cluster up

￮ configuring networking ✓
￮ configuring autoscaling ✓
￮ configuring logging ✓
￮ configuring metrics ✓

cortex is ready!
```

### Deployment automation
On deploy, Cortex automatically packages and containerizes your inference pipeline and its dependencies, deploying your pipeline as an API on the cluster. With a Cortex deployment, you can: 

- Deploy realtime or batch APIs
- Scale to handle production traffic with request-based autoscaling
- Configure traffic splitting for A/B deployments
- Manage dependencies for reproducible deployments
- Update APIs with no downtime
- Test APIs locally before deploying to production
- Customize networking and resources on the API level

```bash
$ cortex deploy

creating text-generator api

status    last-update    latency
live      0ms            82ms

endpoint: https://example.com/text-generator
```

### Observability and monitoring
Cortex automatically configures CloudWatch and sets up monitoring for your pipelines, allowing you to:  

- Monitor all deployed APIs from one place
- Stream logs directly from CloudWatch
- Reproduce deployments and roll APIs back
- Integrate with 3rd party analytics platforms
- Customize prediction tracking

```bash
$ cortex get

env     api                status     replicas   last update

local   text-generator     updating   1          5s
aws     text-generator     live       10         1h
aws     image-classifier   live       10         2h
aws     object-detector    live       10         3h
```

### Widespread integrations
Cortex is designed to be a modular part of your ML stack. As such, you can:

- Import models from any framework (PyTorch, TensorFlow, ONNX, and more)
- Connect with any training platform
- Trigger deployments as part of your CI/CD pipeline
- Integrate any tool (model servers, realtime feature stores, etc.) in your inference pipeline

## Quickstart
Once you've [installed Cortex](https://docs.cortex.dev/install), setting up a deployment is straightforward:

### 1. Configure your cluster
Cortex configures the cluster according to a YAML manifest in your project directory titled `cluster.yaml`. (Note: Without this manifest, Cortex will spin up a cluster according to its defaults). This manifest provides you with several optional knobs to customize your cluster:

```yaml

# cluster.yaml

cluster_name: cortex
region: us-east-1
bucket: s3://models
instance_type: m5.large
min_instances: 1
max_instances: 5
instance_volume_size: 50
instance_volume_type: gp2
api_gateway: public
spot: true
```

You can read the full documentation for `cluster.yaml` [here](https://docs.cortex.dev/cluster-management/config). Once `cluster.yaml` is defined, you can run `$ cortex cluser up` from the project directory and spin up your cluster.

### 2. Write your inference serving code
Cortex inference APIs are written in Python according to Cortex's [Predictor Interface](https://docs.cortex.dev/deployments/realtime-api/predictors). Predictors are simple, flexible Python classes that run an `init()` method upon intialization, and serve requests via a `predict()` method. Cortex also exposes hooks for pre and post-processing, as well built-in clients for TensorFlow Serving and ONNX Runtime:

```python
# predictor.py

from transformers import pipeline

class PythonPredictor:
  def __init__(self, config):
    self.model = pipeline(task="text-generation")

  def predict(self, payload):
    return self.model(payload["text"])[0]
```

Additionally, you need to create a `requirements.txt` file to specify any dependencies. For an example, see [any of our examples.](https://github.com/cortexlabs/cortex/tree/0.21/examples/pytorch/text-generator)

### 3. Define your inference API
Just as each cluster has a YAML manifest, each API has one as well. Within `cortex.yaml`, you can specify basic things ilke the name of your API and the location of your Predictor script, but you can also customize your APIs compute resources, autoscaling behavior, prediction tracking, and more. You can see the [full documentation here.](https://docs.cortex.dev/deployments/realtime-api/api-configuration)

```yaml

# cortex.yaml

- name: text-generator
  predictor:
    path: predictor.py
  networking:
    api_gateway: public
  compute:
    gpu: 1
  autoscaling:
    min_replicas: 3
```

### 4. Deploy to production
Finally, from your project directory, run `$ cortex deploy` to deploy to production. You can check on your API by running `$ cortex get API-NAME`. For more details about deployment and monitoring, [see the docs.](https://docs.cortex.dev/deployments/realtime-api/prediction-monitoring)

## Install and get started
Ready to get started? Install Cortex by running the command below:

<!-- CORTEX_VERSION_README_MINOR -->
```bash
$ bash -c "$(curl -sS https://raw.githubusercontent.com/cortexlabs/cortex/0.20/get-cli.sh)"
```
