## Running Containers

### Introducing Containers

#### Introducing Container Technology

-   Software applications typically depend on other libraries, configuration files, or services provided by their runtime environment.
-   Traditionally, the runtime environment for a software application is installed in an operating system running on a physical host or virtual machine.
-   Any application dependencies are installed along with that operating system on the host.
    <br>

-   In Red Hat Enterprise Linux, packaging systems like RPM are used to help manage application dependencies.
-   When we install the `httpd` package, the RPM system ensures that the correct libraries and other dependencies for that package are also installed.
    <br>

-   The major drawback to traditionally deployed software applications is that these dependencies are entangled with the runtime environment.
-   An application might require versions of supporting software that are older or newer than the software provided with the operating system.
-   Similarly, two applications on the same system might require different versions of the same software that are incompatible with each other.
    <br>

-   One way to resolve these conflicts is to package and deploy the application as a container.
-   A **container** is a set of one or more processes that are isolated from the rest of the system.
    <br>

-   Think of a physical shipping container.
-   A shipping container is a standard way to package and ship goods.
-   It is labeled, loaded, unloaded, and transported from one location to another as a single box.
-   The container's contents are isolated from the contents of other containers so that they do not affect each other.
    <br>

-   Software containers are a way to package applications to simplify deployment and management.

#### Comparing Containers to Virtual Machines

-   Containers provide many of the same benefits as virtual machines, such as security, storage, and network isolation.
    <br>

-   Both technologies isolate their application libraries and runtime resources from the host operating system or hypervisor and vice versa.
    ![virtualization_vs_containers](./images/virtualization-vs-containers.png)
    <br>

-   Containers and Virtual Machines are different in the way they interact with hardware and the underlying operating system.
    <br>

-   **Virtualization :**

    -   Enables multiple operating systems to run simultaneously on a single hardware platform.
    -   Uses a **hypervisor** to divide hardware into multiple virtual hardware systems, allowing multiple operating systems to run side by side.
    -   Requires a complete operating system environment to support the application.
        <br>

-   Compare and contrast this with **Containers**, which :

    -   Run directly on the operating system, sharing hardware and OS resources across all containers on the system. This enables applications to stay lightweight and run swiftly in parallel.
    -   Share the same operating system kernel, isolate the containerized application processes from the rest of the system, and use any software compatible with that kernel.
    -   Require far fewer hardware resources than virtual machines, which also makes them quick to start and stop and reduces storage requirements.
        <br>

-   **Note**
    -   Some applications might not be suitable to run as a container. For example, applications accessing low-level hardware information might need more direct hardware access than containers generally provide.

#### Exploring the Implementation of Containers

-   Red Hat Enterprise Linux implements containers using core technologies such as :

    -   **Control Groups** (cgroups) for resource management.
    -   **Namespaces** for process isolation.
    -   **SELinux** and **Seccomp** (Secure Computing mode) to enforce security boundaries.

#### Planning for Containers

-   Containers are an efficient way to provide reusability and portability of hosted applications.
-   They can be easily moved from one environment to another, such as from development to production.
-   We can save multiple versions of a container and quickly access each one as needed.
    <br>

-   Containers are typically temporary, or ephemeral.
-   We can permanently save data generated by a running container in persistent storage, but the containers themselves usually run when needed, and then stop and are removed.
-   A new container process is started the next time that particular container is needed.

#### Running Containers from Container Images

-   Containers are run from container images. Container images serve as blueprints for creating containers.
    <br>

-   Container images package an application together with all its dependencies, such as :

    -   System libraries
    -   Programming language runtimes
    -   Programming language libraries
    -   Configuration settings
    -   Static data files

-   Container images are unchangeable, or immutable, files that include all the required code and dependencies to run a container.
    <br>

-   Container images are built according to specifications, such as the **Open Container Initiative (OCI)** image format specification.
-   These specifications define the format for container images, as well as the metadata about the container host operating systems and hardware architectures that the image supports.

#### Designing Container-based Architectures

-   We could install a complex software application made up of multiple services in a single container.
-   For example, we might have a web server that needs to use a database and a messaging system. But using one container for multiple services is hard to manage.
    <br>

-   A better design runs each component, the web server, the database, and the messaging system, in separate containers.
-   This way, updates and maintenance to individual application components do not affect other components or the application stack.

#### Managing Containers with Podman

-   A good way to start learning about containers is to work with individual containers on a single server acting as a container host.
-   Red Hat Enterprise Linux provides a set of container tools that we can use to do this, including :
    -   `podman`, which directly manages containers and container images.
    -   `skopeo`, which we can use to inspect, copy, delete, and sign images.
    -   `buildah`, which we can use to create new container images.
-   These tools are compatible with the **Open Container Initiative (OCI)**.
-   They can be used to manage any Linux containers created by OCI-compatible container engines, such as Docker.
-   These tools are specifically designed to run containers under Red Hat Enterprise Linux on a single-node container host.
    <br>

-   In this chapter, we will use `podman` and `skopeo` commands to run and manage containers and existing container images.

#### Running Rootless Containers

-   On the container host, we can run containers as the root user or as a regular, unprivileged user.
-   Containers run by non-privileged users are called **rootless containers**.
    <br>

-   Rootless containers are more secure, but have some restrictions.
-   For example, rootless containers cannot publish their network services through the container host's privileged ports (those below port 1024).
    <br>

-   We can run containers directly as root, if necessary, but this somewhat weakens the security of the system if a bug allows an attacker to compromise the container.

#### Managing Containers at Scale

-   New applications increasingly implement functional components using containers.
-   Those containers provide services that other parts of the application consume.
-   In an organization, managing a growing number of containers may quickly become an overwhelming task.
    <br>

-   Deploying containers at scale in production requires an environment that can adapt to some of the following challenges :

    -   The platform must ensure the availability of containers that provide essential services to customers.
    -   The environment must respond to application usage spikes by increasing or decreasing the running containers and load balancing the traffic.
    -   The platform should detect the failure of a container or a host and react accordingly.
    -   Developers might need an automated workflow to transparently and securely deliver new application versions to customers.
        <br>

-   **Kubernetes** is an orchestration service that makes it easier for us to deploy, manage, and scale container-based applications across a cluster of container hosts.
-   It helps manage DNS updates when we start new containers.
-   It helps redirect traffic to our containers using a load balancer, which also allows us to scale up and down the number of containers providing a service manually or automatically.
-   It also supports user-defined health checks to monitor our containers and to restart them if they fail.
    <br>

-   Red Hat provides a distribution of Kubernetes called **Red Hat OpenShift**.
-   OpenShift is a set of modular components and services built on top of the Kubernetes infrastructure.
-   It adds additional features, such as remote web-based management, multitenancy, monitoring and auditing, application life cycle management, and self-service instances for developers, among others.

-   **Note**

    -   In the enterprise, individual containers are not generally run from the command line.
    -   Instead, it is preferable to run containers in production using a Kubernetes-based platform, such as Red Hat OpenShift.
        <br>

    -   However, we might need to use commands to work with containers and images manually or at a small scale.
    -   To do this, we can install a set of container tools on a Red Hat Enterprise Linux 8 system.
        <br>
    -   This chapter focuses on this use case to help us better understand the core concepts behind containers, how they work, and how they can be useful.