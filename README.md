<div align="center">
<p align="center">
<img alt="Logo" src="https://i.ibb.co/JBt4pXn/logo.png" width="20%">
</p>

![download__1_-removebg-preview](https://github.com/user-attachments/assets/4d3f591a-f40d-4468-a70a-fea41aff20b0)
   ![download-removebg-preview](https://github.com/user-attachments/assets/6a8fd40f-5aa4-499d-a04d-dbcb270b1899)




---

### Multi-Cloud GPU Workflow Management Platform

A vendor-neutral, open-source multi-cloud GPU workflow management platform that integrates advanced workflow automation with GPU resource optimization to address ServiceNow limitations and multi-cloud challenges.

  <a href="#">
    <img alt="Tests Passing" src="https://i.ibb.co/vkR5wTt/badge.png">
    
  </a>
</p>
</div>

## Features

**Key Features of the Multi-Cloud GPU Workflow Management Platform:**  
- **Vendor-Neutral & Open-Source:** Flexible, adaptable to any cloud provider.  
- **Microservices Architecture:** Modular, scalable infrastructure for efficient deployment.  
- **Advanced Workflow Automation:** Low-code/no-code builder with AI-driven optimization.  
- **Seamless Multi-Cloud Integration:** Unified GPU resource management across AWS, Azure, and GCP.  
- **GPU Virtualization Framework:** Lightweight, cross-cloud resource sharing with security-first design.  
- **Dynamic Resource Allocation:** ML-powered scheduling for cost-performance optimization.  
- **Comprehensive Analytics Engine:** Real-time dashboards, predictive modeling, and performance insights.  
- **Enhanced Customization:** API-first design enabling deep integrations with existing systems.  
- **AI-Powered Data Transformation:** Real-time middleware for seamless legacy system integration.  
- **Cost-Effective Strategy:** Intelligent caching and workload migration to optimize expenses.

## How Does It Work?

### Run Serverless AI Workloads

Add an `endpoint` decorator to your code, and you'll get a load-balanced HTTP endpoint (with auth!) to invoke your code.

You can also run long-running functions with `@function`, deploy task queues using `@task_queue`, and schedule jobs with `@schedule`:

```python
from 7rainbow import endpoint


# This will run on a remote A100-40 in your cluster
@endpoint(cpu=1, memory=128, gpu="A100-40")
def square(i: int):
    return i**2
```

Deploy with a single command:

```
$ 7rainbow deploy app.py:square --name inference
=> Building image
=> Using cached image
=> Deployed 🎉

curl -X POST 'https://inference.beam.cloud/v1' \
-H 'Authorization: Bearer [YOUR_AUTH_TOKEN]' \
-H 'Content-Type: application/json' \
-d '{}'
```

### Run on Bare-Metal Servers Around the World

Connect any GPU to your cluster with one CLI command and a cURL.

```sh
$ 7rainbow machine create --pool lambda-a100-40

=> Created machine with ID: '9541cbd2'. Use the following command to set up the node:

#!/bin/bash
sudo curl -L -o agent https://release.beam.cloud/agent/agent && \
sudo chmod +x agent && \
sudo ./agent --token "AUTH_TOKEN" \
  --machine-id "9541cbd2" \
  --tailscale-url "" \
  --tailscale-auth "AUTH_TOKEN" \
  --pool-name "lambda-a100-40" \
  --provider-name "lambda"
```

You can run this install script on your VM to connect it to your cluster.

### Manage Your CPU or GPU Fleet

Manage your distributed cross-region cluster using a centralized control plane.

```sh
$ 7rainbow   machine list

| ID       | CPU     | Memory     | GPU     | Status     | Pool        |
|----------|---------|------------|---------|------------|-------------|
| edc9c2d2 | 30,000m | 222.16 GiB | A10G    | registered | lambda-a10g |
| d87ad026 | 30,000m | 216.25 GiB | A100-40 | registered | gcp-a100-40 |

```


### Setting Up the Server

k3d is used for local development. You'll need Docker to get started.

To use our fully automated setup, run the `setup` make target.

```bash
make setup
```

#### Local DNS

This is required to use an external file service for mulitpart uploads and range downloads. Its optional for using the subdomain middlware (host-based URLs).

```shell
brew install dnsmasq
echo 'address=/cluster.local/127.0.0.1' >> /opt/homebrew/etc/dnsmasq.conf
sudo bash -c 'mkdir -p /etc/resolver'
sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/cluster.local'
sudo brew services start dnsmasq
```

To use subdomain or host-based URLs, add this to the config and rebuild the Beta9 gateway.

```yaml
gateway:
  invokeURLType: host
```

You should now be able to access your local k3s instance via a domain.

```shell
curl http://beta9-gateway.beta9.svc.cluster.local:1994/api/v1/health
```

### Setting Up the SDK

The SDK is written in Python. You'll need Python 3.8 or higher. Use the `setup-sdk` make target to get started.

```bash
make setup-sdk
```

### Using the SDK

After you've setup the server and SDK, check out the SDK readme [here](sdk/README.md).

## Contributing

We welcome contributions big or small. These are the most helpful things for us:
- Open a PR with a new feature or improvement
