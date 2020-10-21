<!-- Delete on release branches -->
<img src='https://s3-us-west-2.amazonaws.com/cortex-public/logo.png' height='42'>

<br>

<!-- Delete on release branches -->
<!-- CORTEX_VERSION_README_MINOR -->

[install](https://docs.cortex.dev/install) • [documentation](https://docs.cortex.dev) • [examples](https://github.com/cortexlabs/cortex/tree/0.20/examples) • [we're hiring](https://angel.co/cortex-labs-inc/jobs) • [chat with us](https://gitter.im/cortexlabs/cortex)

<br>

# Machine learning in production

Cortex is a scalable, full stack platform for building machine learning systems in production.

**Run inference at scale** - Request-based autoscaling, load balancing, multi-model caching, GPU/ASIC inference.

**Build complex inference pipelines** - Real-time and batch inference, A/B and multi-armed bandit testing, support for model servers, real-time feature stores, data lakes, and more.

**Maintain complete visibility** - Monitoring, log streaming, reproducible deployments, support for third-party analytics services.

**Integrate with your entire stack** - Hosted on your AWS account, triggerable by CI/CD pipelines, support for all ML frameworks. 

**Enjoy machine learning engineering** - Complete DevOps automation, simple model serving code, streamlined update workflow.

<br>

## How to use Cortex

### Spin up a Cortex cluster

Spin up a cluster configured for inference with a single command.

```bash
$ cortex cluster up

confguring networking ...
configuring logging ...
configuring metrics ...
configuring autoscaling ...

cortex is ready!
```

<br>


### Define an inference pipeline

Define any real-time or batch inference pipeline as simple Python APIs, regardless of framework.

```python
# predictor.py

from transformers import pipeline

class PythonPredictor:
  def __init__(self, config):
    self.model = pipeline(task="text-generation")

  def predict(self, payload):
    return self.model(payload["text"])[0]
```

<br>

### Configure a deployment

Configure request-based autoscaling, monitoring, compute resources, update strategies, and more in readable YAML.

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

<br>

### Deploy to production

Containerize your pipeline, deploy it to your cluster, and expose it behind a load balancer with one command.

```bash
$ cortex deploy

creating text-generator api
```

<br>

### Monitor and manage deployed pipelines

Stream real-time analytics to monitor performance. Update pipelines with zero downtime via rolling updates and live reloading.

```bash
$ cortex get text-generator

endpoint: https://example.com/text-generator

status   last-update   replicas   requests   latency
live     10h           150        1823733    74ms

$ cortex deploy

updating text-generator api
```

<br>


## Get started

<!-- CORTEX_VERSION_README_MINOR -->
```bash
bash -c "$(curl -sS https://raw.githubusercontent.com/cortexlabs/cortex/0.20/get-cli.sh)"
```

<!-- CORTEX_VERSION_README_MINOR -->
See our [installation guide](https://docs.cortex.dev/install), then deploy one of our [examples](https://github.com/cortexlabs/cortex/tree/0.20/examples) or bring your own models to build [realtime APIs](https://docs.cortex.dev/deployments/realtime-api) and [batch APIs](https://docs.cortex.dev/deployments/batch-api).
