---
title: Docker Enterprise アーキテクチャー
description: Learn about the architecture of Docker Enterprise  and how it delivers high availability for your workloads.
keywords: Docker Enterprise, UCP, DTR,  architecture, orchestration, Kubernetes, Swarm, cluster, high availability
---

{% comment %}
Docker Enterprise enables deploying your workloads for high
availability (HA) onto the orchestrator of your choice. Docker Enterprise system
components can run on multiple manager nodes in the cluster, and if one manager
node fails, another takes its place automatically, without impact to the
cluster.
{% endcomment %}
Docker Enterprise enables deploying your workloads for high
availability (HA) onto the orchestrator of your choice. Docker Enterprise system
components can run on multiple manager nodes in the cluster, and if one manager
node fails, another takes its place automatically, without impact to the
cluster.

{% comment %}
## Choose your orchestrator
{% endcomment %}
{: #choose-your-orchestrator }
## オーケストレーターの選択

{% comment %}
Docker Enterprise provides access to the full API sets of three popular orchestrators:
{% endcomment %}
Docker Enterprise provides access to the full API sets of three popular orchestrators:

{% comment %}
- Kubernetes: Full YAML object support
- SwarmKit: Service-centric, Compose file version 3
- "Classic" Swarm: Container-centric, Compose file version 2
{% endcomment %}
- Kubernetes: Full YAML object support
- SwarmKit: Service-centric, Compose file version 3
- "Classic" Swarm: Container-centric, Compose file version 2

![](images/docker-ee-architecture-1.svg){: .with-border}

{% comment %}
Docker Enterprise proxies the underlying API of each orchestrator, giving you access
to all of the capabilities of each orchestrator, along with the benefits of
Docker Enterprise, like role-based access control and Docker Content Trust.
{% endcomment %}
Docker Enterprise proxies the underlying API of each orchestrator, giving you access
to all of the capabilities of each orchestrator, along with the benefits of
Docker Enterprise, like role-based access control and Docker Content Trust.

{% comment %}
## Docker Enterprise components
{% endcomment %}
{: #docker-Enterprise-components }
## Docker Enterprise コンポーネント

{% comment %}
Docker Enterprise has three major components, which together enable a full software
supply chain, from image creation, to secure image storage, to secure image
deployment.
{% endcomment %}
Docker Enterprise has three major components, which together enable a full software
supply chain, from image creation, to secure image storage, to secure image
deployment.

{% comment %}
- **Docker Engine - Enterprise**: The commercially supported Docker engine for creating
  images and running them in Docker containers.
{% endcomment %}
- **Docker Engine - Enterprise**: The commercially supported Docker engine for creating
  images and running them in Docker containers.

{% comment %}
- **Docker Trusted Registry (DTR)**: The production-grade image storage solution
  from Docker.
{% endcomment %}
- **Docker Trusted Registry (DTR)**: The production-grade image storage solution
  from Docker.

  {% comment %}
  DTR is designed to scale horizontally as your usage increases.
  You can add more replicas to make DTR scale to your demand and for high
  availability.
  {% endcomment %}
  DTR is designed to scale horizontally as your usage increases.
  You can add more replicas to make DTR scale to your demand and for high
  availability.

  {% comment %}
  All DTR replicas run the same set of services, and changes to
  their configuration are propagated automatically to other replicas.
  {% endcomment %}
  All DTR replicas run the same set of services, and changes to
  their configuration are propagated automatically to other replicas.

{% comment %}
- **Universal Control Plane (UCP)**: Deploys applications from images, by
  managing orchestrators, like Kubernetes and Swarm.
{% endcomment %}
- **Universal Control Plane (UCP)**: Deploys applications from images, by
  managing orchestrators, like Kubernetes and Swarm.

  {% comment %}
  UCP is designed for high availability (HA). You can join multiple UCP manager
  nodes to the cluster, and if one manager node fails, another takes its place
  automatically without impact to the cluster.
  {% endcomment %}
  UCP is designed for high availability (HA). You can join multiple UCP manager
  nodes to the cluster, and if one manager node fails, another takes its place
  automatically without impact to the cluster.

  {% comment %}
  Changes to the configuration of one UCP manager node are propagated
  automatically to other nodes.
  {% endcomment %}
  Changes to the configuration of one UCP manager node are propagated
  automatically to other nodes.

![](images/docker-ee-architecture.svg){: .with-border}

### Universal Control Plane (UCP)

{% comment %}
Docker UCP is a containerized application that runs on [Docker Engine - Enterprise](../engine/index.md)
and extends its functionality to make it easier to deploy, configure, and
monitor your applications at scale.
{% endcomment %}
Docker UCP is a containerized application that runs on [Docker Engine - Enterprise](../engine/index.md)
and extends its functionality to make it easier to deploy, configure, and
monitor your applications at scale.

{% comment %}
Docker UCP provides a web interface and a CLI for deploying images from Kubernetes
YAML or Compose files. Once your workload is deployed, UCP enables monitoring
containers and pods across your Docker cluster.
{% endcomment %}
Docker UCP provides a web interface and a CLI for deploying images from Kubernetes
YAML or Compose files. Once your workload is deployed, UCP enables monitoring
containers and pods across your Docker cluster.

{% comment %}
UCP also secures Docker with role-based access control so that only authorized
users can make changes and deploy applications to your cluster.
{% endcomment %}
UCP also secures Docker with role-based access control so that only authorized
users can make changes and deploy applications to your cluster.

![](/ee/ucp/images/ucp-architecture-1.svg){: .with-border}

{% comment %}
Once a UCP instance is deployed, you don't interact with Docker Engine - Enterprise
directly. Instead, you interact with UCP. Since UCP exposes the standard
Docker API and the full Kubernetes API transparently, you can use the tools
you already know and love, like `kubectl`, the Docker CLI client, and Docker
Compose.
[Learn about UCP architecture](ucp/ucp-architecture.md).
{% endcomment %}
Once a UCP instance is deployed, you don't interact with Docker Engine - Enterprise
directly. Instead, you interact with UCP. Since UCP exposes the standard
Docker API and the full Kubernetes API transparently, you can use the tools
you already know and love, like `kubectl`, the Docker CLI client, and Docker
Compose.
[Learn about UCP architecture](ucp/ucp-architecture.md).

![](/ee/ucp/images/ucp-architecture-2.svg){: .with-border}

### Docker Trusted Registry (DTR)

{% comment %}
Docker Trusted Registry (DTR) is a containerized application that runs on a
Docker UCP cluster.
{% endcomment %}
Docker Trusted Registry (DTR) is a containerized application that runs on a
Docker UCP cluster.

![](/ee/dtr/images/architecture-1.svg){: .with-border}

{% comment %}
Once you have DTR deployed, you use your Docker CLI client to login, push, and
pull images.
{% endcomment %}
Once you have DTR deployed, you use your Docker CLI client to login, push, and
pull images.

{% comment %}
For high-availability, you can deploy multiple DTR replicas, one on each UCP
worker node.
{% endcomment %}
For high-availability, you can deploy multiple DTR replicas, one on each UCP
worker node.

![](/ee/dtr/images/architecture-2.svg){: .with-border}

{% comment %}
All DTR replicas run the same set of services, and changes to their configuration
are automatically propagated to other replicas.
[Learn about DTR architecture](dtr/architecture.md).
{% endcomment %}
All DTR replicas run the same set of services, and changes to their configuration
are automatically propagated to other replicas.
[Learn about DTR architecture](dtr/architecture.md).

{% comment %}
## Where to go next
{% endcomment %}
{: #where-to-go-next }
## Where to go next

{% comment %}
- [UCP architecture](ucp/ucp-architecture.md)
- [DTR architecture](dtr/architecture.md)
{% endcomment %}
- [UCP アーキテクチャー](ucp/ucp-architecture.md)
- [DTR アーキテクチャー](dtr/architecture.md)
