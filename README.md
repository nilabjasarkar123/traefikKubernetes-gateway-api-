# traefikKubernetes-gateway-api

# <font  size = 9>Report on K8's Gateway API

# <font  size = 8>Nilabja Sarkar(T623)

  

## <font  size = 8>Abstract

<font  size = 4>The Gateway API in Kubernetes is a relatively new concept that enables developers and operators to configure and manage traffic routing for applications running in Kubernetes clusters. The Gateway API is designed to simplify and standardise the configuration and management of traffic routing for Kubernetes-based applications.

The Gateway API solves this problem by providing a standardised, Kubernetes-native way to manage traffic routing. The Gateway API defines a set of custom resources that represent different types of routing objects, such as HTTP routes, TCP routes, and TLS configurations. These resources can be defined and managed using standard Kubernetes tools such as kubectl or YAML files<font>

  
  

## <font  size = 8>Content

  

-  <font  size=4>[Ingress](#ingress)

-  <font  size=4>[Gateway API](#Gateway-API)

-  <font  size=4>[Traefik ](#Traefik)

-  <font  size=4>[Installation](#Installation)

-  <font  size=4>[Traefik Use Cases](#USE-CASES)

    -  <font  size=4>[Setting up of a server](#Setting-up-of-a-server)

    -  <font  size=4>[Deploying a simple Gateway](#Deploying-a-simple-Gateway)

	-  <font  size=4>[HTTP routing](#HTTP-routing)

	-  <font  size=4>[Simple host with paths](#Simple-host-with-paths)

	-  <font  size=4>[Cross Namespace](#Cross-Namespace)

	-  <font  size=4>[TLS with http redirect](#TLS-with-http-redirect)

  

-  <font  size=4>[Limitations of Gateway API](#Limitations-of-Gateway-API)

-  <font  size=4>[CONCLUSION](#CONCLUSION)

-  <font  size=4> [References](#References])

  
  

## <font  size = 8>Ingress

### <font  size = 6>What is an Ingress?

<font  size = 4>Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

  

### <font  size = 6>What is Ingress in Kubernetes?

<font  size = 4>In Kubernetes, an Ingress is an object that allows access to Kubernetes services from outside the Kubernetes cluster. You can configure access by creating a collection of rules that define which inbound connections reach which services.

  

<font  size = 4>An Ingress can be configured to give Services externally-reachable URLs, load balance traffic, terminate SSL/TLS, and offer name-based virtual hosting. Ingress lets you configure an HTTP load balancer for applications running on Kubernetes, represented by one or more Kubernetes internal Services.

  

<font  size = 4>An Ingress controller is responsible for fulfilling the Ingress, usually with a load balancer, though it may also configure your edge router or additional frontends to help handle the traffic.

  

<font  size = 4>The Ingress spec has all the information needed to configure a load balancer or proxy server. It contains a list of rules matched against all incoming requests. Ingress provides routing rules to manage external users’ access to the services in a Kubernetes cluster, typically via HTTPS/HTTP. With Ingress, you can easily set up rules for routing traffic without creating a bunch of Load Balancers or exposing each service on the node. This makes it the best option to use in production environments.

</font>

**![](https://lh3.googleusercontent.com/7UndSzusCJb6KYBLABMc_dUmcyb6AtPsMWWjmfdDKSBD0XNyAB18VbLyur_zbZQ6S0qJes2y8Is2nkHyJvGK2qvvtPC5tEwcGQmPzvwSXGJo0puN0jCXppVjpyET6ta0I95eU_t0xxMMvDEzMMN6LRY)**

**![](https://lh5.googleusercontent.com/8zpt2T9kSd9yExpZWzkTIbiCXs8pCe0m6TSFWfoAOcWes1nhyNuMdaQ7hwGmjIAvAcrcdA6uUTW43fsguaCznauafoadRY9BZ0NjhCaCsncf6VDg-WCBU9D0gAWrfZjfPQol16Fv9cninHI1F9olX5c)**

  

# <font  size = 8>Gateway API

## <font  size = 6>What is gateway API k8s?

<font  size = 4>In Kubernetes (also known as K8s), a Gateway API is a new Kubernetes-native API that allows you to configure load balancing and other networking features in a more declarative way.

  

<font  size = 4>The Gateway API is designed to replace the traditional Kubernetes Ingress controller, which has some limitations in terms of functionality and extensibility. With the Gateway API, you can define custom resources that represent load balancers, ingress controllers, and other types of networking components.

  

<font  size = 4>The Gateway API is built on top of the Kubernetes Custom Resource Definition (CRD) system, which allows you to define your own custom resources and controllers. The Gateway API includes a set of predefined resources and controllers for load balancing, routing, TLS termination, and other networking features.

  

<font  size = 4>By using the Gateway API, you can simplify your Kubernetes networking configuration and make it more flexible and scalable. You can also take advantage of features like the dynamic discovery of backend services, automatic certificate provisioning, and integration with external DNS providers.

  

## <font  size = 8>Gateway API flow

**![](https://lh4.googleusercontent.com/7_qqZUXGbsCtnvZwhYEG7zKvblUV1UzNgspT1nqIOjmgB3zMR-Jmc_MURbZHqfa53mAG6YX9XkQnyvk30NSlIWj7tmZnCH0z6elH4bpJAoVzC8UzI4XL8xBFnjTzMPmZIalRnd7H3OsZjdQOtueAsBw)**

# <font  size = 8>Traefik

## <font  size = 6>Traefik Overview

<font  size = 4>Traefik is a modern HTTP reverse proxy and load balancer that makes deploying microservices easy. Traefik integrates with your existing infrastructure components (Docker, Swarm mode, Kubernetes, Marathon, Consul, Etcd, Rancher, Amazon ECS, ...) and configures itself automatically and dynamically. Pointing Traefik at your orchestrator should be the only configuration step you need.

  

<font  size = 4>Assume that you have deployed a number of microservices using a service registry or an orchestrator (such as Swarm or Kubernetes) (like etcd or consul). Now that users may access these microservices, a reverse proxy is required.

  

  

<font  size = 4>The routes that connect the pathways and subdomains to each microservice must be configured for traditional reverse proxies. The chore of updating the routes gets tiresome in a situation where you frequently add, remove, kill, upgrade, or scale your services.

  

  

<font  size = 4>Traefik can assist you at this point! Without additional input from you, Traefik automatically builds the routes so that your microservices are connected to the outside world by listening to your service registry/orchestrator API.

## <font  size = 8>**Installation**

### <font  size = 6>**Installing the CRDs**

<font  size = 4>On Kubernetes clusters, the Gateway API is not installed by default. Before turning on support in Traefik Proxy for the clusters, make sure the necessary custom resource definitions (CRDs) have been installed.

```

**kubectl apply -k "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v0.4.0"**

```

### <font  size = 6> Install and configure Traefik Proxy to use the Gateway API

<font  size = 4>To install Traefik Proxy v2.6 and have it configured to enable the new provider, the

  

<font  size = 4>best way is to use the official Helm chart:

```

helm repo add traefik https://helm.traefik.io/traefik

helm repo update

helm install traefik --set experimental.kubernetesGateway.enabled=true traefik/traefik

```

## <font  size = 7>USE CASES

### <font  size = 6>**Setting up of a server**

<font  size = 4>Use the bellow command to run the respective yaml files

```

kubectl apply -f

```

  

```

---

apiVersion: apps/v1

kind: Deployment

metadata:

name: whoami

spec:

replicas: 2

selector:

matchLabels:

app: whoami

template:

metadata:

labels:

app: whoami

spec:

containers:

- name: whoami

image: traefik/whoami:v1.6.0

ports:

- containerPort: 80

name: http

  
  

---

apiVersion: v1

kind: Service

metadata:

name: whoami

spec:

externalTrafficPolicy: Local

type: LoadBalancer

externalIPs:

- 192.168.0.17

selector:

app: whoami

ports:

- port: 80

  

```

### <font  size = 6> Deploying a simple Gateway

<font  size = 4>The Gateway is an instance of a logical load balancer. It is modeled after an improbable acme-lb GatewayClass. On port 80, the Gateway watches for HTTP traffic. After being deployed, this specific GatewayClass will automatically assign an IP address that will be displayed in the Gateway status.

  

<font  size = 4>Using ParentRefs, Route resources describe which Gateways they want to attach to. This will enable the Route to receive traffic from the parent Gateway as long as the Gateway permits this attachment (by default, Routes from the same namespace are trusted). The backends to which traffic will be delivered are specified by BackendRefs. There are options for and explanations of more complicated bi-directional matching and permissions in other manuals.

```

apiVersion: gateway.networking.k8s.io/v1alpha2

kind: Gateway

metadata:

name: my-gateway

namespace: gateway-ns

spec:

gatewayClassName: traefik

listeners:

- name: http

protocol: HTTP

port: 80

```

### <font  size = 6>HTTP routing

```

# 02-whoami-httproute.yaml

---

apiVersion: gateway.networking.k8s.io/v1alpha2

kind: HTTPRoute

metadata:

name: http-app-1

namespace: default

spec:

parentRefs:

- name: traefik-gateway

hostnames:

- whoami

rules:

- matches:

- path:

type: Exact

value: /

backendRefs:

- name: whoami

port: 80

weight: 1

```

<font  size = 4>This HTTPRoute will catch requests made to the whoami hostname and forward them to the whoami service you deployed earlier. If you now make a request for that hostname, you will see typical whoami output, which looks something like this:

```

curl --resolve whoami:$GW_PORT:$GW_IP http://whoami:$GW_PORT/

Hostname: whoami-7976846947-rrp6g

IP: 127.0.0.1

IP: 10.0.0.15

RemoteAddr: 10.0.0.68:54762

GET / HTTP/1.1

Host: whoami

User-Agent: curl/7.68.0

Accept: */*

Accept-Encoding: gzip

X-Forwarded-For: 10.0.0.198

X-Forwarded-Host: whoami

X-Forwarded-Port: 80

X-Forwarded-Proto: http

X-Forwarded-Server: traefik-cb955f7d8-lhdjb

X-Real-Ip: 10.0.0.198

```

**![](https://lh4.googleusercontent.com/AhgtvFstTXKVGn8MCkNEG17E6AJPzxcEU_pByRDd9HlcuZ7Vyj4MOaV1J8EE9zExLaqeC6pjcZPdqTp5aJJlD1eIfeH22SSJSAsW92qL47P7FBTL_jnx-LEvl-bJl7Xbj4a7DNOEUKJgTccSIYqlrlI)**

### <font  size=6>Simple host with paths

```

# 03-whoami-httproute-paths.yaml

---

apiVersion: gateway.networking.k8s.io/v1alpha2

kind: HTTPRoute

metadata:

name: http-app-1

namespace: default

spec:

parentRefs:

- name: traefik-gateway

hostnames:

- whoami

rules:

- matches:

- path:

type: Exact

value: /foo

  

```

<font size = 4>With the modified HTTPRoute, you'll see that the previous request now returns a 404 error, while requesting the /foo path suffix returns success:

```

curl --resolve whoami:$GW_PORT:$GW_IP http://whoami:$GW_PORT/foo

```

```

Hostname: whoami-7976846947-rrp6g

IP: 127.0.0.1

IP: 10.0.0.15

RemoteAddr: 10.0.0.68:37682

GET /foo HTTP/1.1

Host: whoami

User-Agent: curl/7.68.0

Accept: */*

Accept-Encoding: gzip

X-Forwarded-For: 10.0.0.198

X-Forwarded-Host: whoami

X-Forwarded-Port: 80

X-Forwarded-Proto: http

X-Forwarded-Server: traefik-cb955f7d8-lhdjb

X-Real-Ip: 10.0.0.198

```

**![](https://lh6.googleusercontent.com/ydqkxykYnwDBoDXpVf1yfGEjKsS6-56ouwkQfD17p3Z_MjqDtLOs-3i01lc4mFYS7cSJd7g0gt4WKXpGLqeeWOIT56V891geTjYx221vouE5SKRfr3T-i8glplii_kGYnR6rq50ravS3RPK6CTt8W6I)**

### <font size = 6> Cross Namespace

<font size = 4>Cross Namespace routing has fundamental support in the Gateway API. When more than one user or team is using the same underlying networking infrastructure, this is helpful since control and configuration must be divided to reduce access and fault zones.

  

<font size = 4>Routes can attach to Gateways across Namespace boundaries, and Gateways can be deployed into many Namespaces. As a result, access and control to various components of the cluster-wide routing configuration can be effectively segmented and applied differentially across Namespaces for Routes and Gateways.

  

<font size = 4>Route Attachment controls how Routes can connect to Gateways across namespace boundaries. This guide explores route attachment and shows how diverse teams can securely share a Gateway.

```

#Traefik-routes.yaml

apiVersion: apps/v1

kind: Deployment

metadata:

name: coffee

namespace: coffee-traefik-ns

spec:

replicas: 1

selector:

matchLabels:

app: coffee

template:

metadata:

labels:

app: coffee

spec:

containers:

- name: coffee

image: nginxdemos/nginx-hello:plain-text

ports:

- containerPort: 8080

---

apiVersion: v1

kind: Service

metadata:

name: coffee

namespace: coffee-traefik-ns

spec:

ports:

- port: 80

targetPort: 8080

protocol: TCP

name: http

selector:

app: coffee

---

apiVersion: apps/v1

kind: Deployment

metadata:

name: tea

namespace: tea-traefik-ns

spec:

replicas: 1

selector:

matchLabels:

app: tea

template:

metadata:

labels:

app: tea

spec:

containers:

- name: tea

image: nginxdemos/nginx-hello:plain-text

ports:

- containerPort: 8080

---

apiVersion: v1

kind: Service

metadata:

name: tea

namespace: tea-traefik-ns

spec:

ports:

- port: 80

targetPort: 8080

protocol: TCP

name: http

selector:

app: tea

```

```

  

#Traefik-cafe-routes.yaml

apiVersion: gateway.networking.k8s.io/v1beta1

kind: HTTPRoute

metadata:

name: coffee

namespace: coffee-traefik-ns

spec:

parentRefs:

- name: my-gateway

namespace: gateway-ns

hostnames:

- "whoami"

rules:

- matches:

- path:

type: PathPrefix

value: /coffee

backendRefs:

- name: coffee

port: 80

---

apiVersion: gateway.networking.k8s.io/v1beta1

kind: HTTPRoute

metadata:

name: tea

namespace: tea-traefik-ns

spec:

parentRefs:

- name: my-gateway

namespace: gateway-ns

hostnames:

- "whoami"

rules:

- matches:

- path:

type: PathPrefix

value: /tea

backendRefs:

- name: tea

port: 80

```

```

root@calsoftlabs:~/nilabja/project/traefik/demo_presentation/k8s-gateway-api# curl --resolve whoami:$GW_PORT:$GW_IP http://whoami:$GW_PORT/coffee

Hostname: whoami-7976846947-rrp6g

IP: 127.0.0.1

IP: 10.0.0.15

RemoteAddr: 10.0.0.198:28633

GET /coffee HTTP/1.1

Host: whoami

User-Agent: curl/7.68.0

Accept: */*

```

```

root@calsoftlabs:~/nilabja/project/traefik/demo_presentation/k8s-gateway-api# curl --resolve whoami:$GW_PORT:$GW_IP http://whoami:$GW_PORT/tea

Hostname: whoami-7976846947-rrp6g

IP: 127.0.0.1

IP: 10.0.0.15

RemoteAddr: 10.0.0.198:15649

GET /tea HTTP/1.1

Host: whoami

User-Agent: curl/7.68.0

Accept: */*

  

```

**![](https://lh5.googleusercontent.com/ZXc220_cneLRjadtNyhoZt7sfuuUKLcwRDYsnztO_tp1MH0FfxIT81Kts5C15laYf3TWyTX9XV3OKJaP5zVbVT81tR7gU1dCeH9g3nn1-XFV6mFIzultFLF2crZ5_Y-6qtUmkCCreFbfNUA5v5C3utk)**

  

### <font size = 6>TLS with http redirect

<font size = 4>We can secure a Kubernetes-based application by generating a secret that includes a TLS (Transport Layer Security) private key and certificate.

  

<font size = 4>Currently, Ingress assumes TLS termination and supports only port 443 of the TLS protocol.

  

<font size = 4>The keys tls. crt and tls. keys, which hold the certificate and private key to use for TLS, must be included in the TLS secret.

  

**![](https://lh3.googleusercontent.com/iK-zlO4T_WqvrW2FhNet20vqvKFSR_WqxUOZpTsuRM8nhty4X8T1dT0mPDncvQH9hfYqOgUXBlzOnfwiwBY-hv_xlXa6VZcPcco34RBpVYd_j8fO-uvTxtfRkBwtkjCI9VI-yWaQP_pMyJLAb5VJS8k)**

```

curl --resolve whoami.com:$GW_HTTP_PORT:$GW_IP http://whoami.com:$GW_HTTP_PORT/coffee --include

  

```

```

curl --resolve whoami.com:$GW_HTTP_PORT:$GW_IP http://whoami.com:$GW_HTTP_PORT/coffee --include

HTTP/1.1 200 OK

Date: Thu, 23 Mar 2023 08:47:05 GMT

Content-Length: 171

Content-Type: text/plain; charset=utf-8

  

Hostname: whoami-7976846947-rrp6g

IP: 127.0.0.1

IP: 10.0.0.15

RemoteAddr: 10.0.0.198:23004

GET /coffee HTTP/1.1

Host: whoami.com

User-Agent: curl/7.68.0

Accept: */*

  

```

```

curl --resolve whoami.com:$GW_HTTP_PORT:$GW_IP http://whoami.com:$GW_HTTP_PORT/tea --include

```

**![](https://lh3.googleusercontent.com/1t2D25F51H2dMSKwfQ0eOMoal0xUNMVhl3ntH9vkEY0UyZRZubBzlm5FNvuAQ9PQ_QpBO0s4jiyuM13hMkOJ_wOj8CAY4qOJGtP-jRVfYE4NrxUn_UnjsIpkT9hQ6VES0G0ZN7wc_bXcaL07EG7UXfc)**

```

curl --resolve whoami.com:$GW_HTTP_PORT:$GW_IP http://whoami.com:$GW_HTTP_PORT/tea --include

HTTP/1.1 200 OK

Date: Thu, 23 Mar 2023 08:56:48 GMT

Content-Length: 168

Content-Type: text/plain; charset=utf-8

  

Hostname: whoami-7976846947-6c5sw

IP: 127.0.0.1

IP: 10.0.1.76

RemoteAddr: 10.0.0.198:47076

GET /tea HTTP/1.1

Host: whoami.com

User-Agent: curl/7.68.0

Accept: */*

```

  

## <font size = 6>Limitations of Gateway API

<font size = 4>The gateway API in Kubernetes is a relatively new feature and while it has a lot of benefits, it also has some limitations. Some of these limitations include:

  

  

1. <font size = 4>**Complexity**:

The gateway API can be quite complex and difficult to configure, especially for users who are not familiar with Kubernetes.

  
  

2. <font size = 4>**Limited support for legacy protocols**:

The gateway API currently only supports modern protocols like HTTP/2 and gRPC. This means that if you have applications that use older protocols like HTTP/1.1 or WebSocket, you may need to use other solutions.

3. <font size = 4>**Limited feature set**:

The gateway API is still a relatively new feature and does not have all the features that users may need. For example, it does not currently support rate limiting or load balancing.

4. <font size = 4>**Resource usage**:

The gateway API can consume significant resources, which may be a concern for users with large clusters.

5. <font size = 4>**Potential security risks**:

The gateway API can be a potential security risk if not properly configured. This is particularly true if the gateway is exposed to the internet, as it can be targeted by attackers.

6. <font size = 4>**Limited backward compatibility**:

The Gateway API may not be fully backward compatible with existing Kubernetes APIs and configurations. This may make it more challenging to migrate to the gateway API for some users.

  
  
  

## CONCLUSION

<font size = 4>In conclusion, the Gateway API for Kubernetes is an important development that enables users to manage and configure the network services in a more efficient and effective way. By providing a unified and consistent interface, it simplifies the management of complex networking setups, making it easier for users to deploy and manage their applications.

  

<font size = 4>The Gateway API offers several benefits, including simplifying the configuration of networking policies, enabling the use of custom resources, and providing a standard interface for service discovery and load balancing. It also supports a wide range of networking technologies, such as TCP, UDP, HTTP, and HTTPS, and is extensible, making it easy to integrate with other tools and platforms.

  

<font size = 4>Overall, the Gateway API is a powerful tool for managing network services in Kubernetes environments, and its adoption is likely to grow as more users recognize its benefits. As Kubernetes continues to evolve, it is likely that the Gateway API will become an even more critical component of the platform, enabling users to build more reliable and scalable applications.

  

<font size = 4>There are some Gateway api integration like Flagger, cert-manager, algo rollouts etc.

  

# # <font size=6>References

1. [https://kubernetes.io/docs/home](https://kubernetes.io/docs/home)/

2. [https://gateway-api.sigs.k8s.io/](https://gateway-api.sigs.k8s.io/)

3. [https://doc.traefik.io/traefik/](https://doc.traefik.io/traefik/) 
