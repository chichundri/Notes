#Kubernetes
Single pod can have single/multiple containers
#container - package your application and isolate from host. 
  e.g google kubernetes,docker container.
#kubernetes - 
  1.  open source container management tool which automates container deployment,container scaling and load balancing.
  2.  developed by Google in golang
  3.  can group 'n' no. of group in single unit.
  4.  manages multiple docker containers

#Features
  1.  Binpackaging.
  2.  service discovery & load balancing.
  3.  storage orchestration
  4.  self healing
  5.  batch execution
  6.  secret & configuration management
  7.  horizontal scaling
  8.  automatic rollout & rolebacks
  9.  Configuration management

Kubernetes is not
  -Docker, Docker is containerization platform and kubernetes is container management platform. Docker swarm does container management
    similar to kubernetes.
Kubernetes is 
  -container orchestration and nothing but
  -Robust and reliable.
  -best solution for scaling up containers
  -backed by huge community.

#Kubernete vs docker swarm
  1.  kubernetes installation and conf. is complicated than docker swarm
  2.  kubernete GUI available,docker swarm GUI not available.
  3.  K8S scaling up is slow but gurantees stronger cluster state,where as docker swarm is faster but cluster strenth is not robust.
  4.  K8S load balancing is manual, where is load balancing in DS is prebuit.
  5.  Data volumes cannot shared outside POD,where as it is possible in Docker swarm 

One pod can run multiple container in kubernetes.

#Working of k8s- 
kubernetes master - controls the cluster and manages the nodes in it.
Nodes - host the container inside them, containers are inside separate POD's
POD - logical collection of containers which needs to interact with each other for an application.

---------------------------------------------------------------------------------
Spring Cloud
#Spring cloud Features
1.  Distributed/Versioned configuration
2.  Service registration & discovery
3.  Routing
4.  Service-to-service calls
5.  Load-balancing
6.  Circuit breakers
7.  Global locks
8.  Distributed messaging
9.  ZUUL - server side load balancing/API gateway

#Common Runtime Services & Libraries - Runtime containers, libraries and services that power microservices  

Eureka - service discovery & registration  
Archaius - distributed configuration  
Ribbon - inter-process and service communication  
Hystrix - is provided to isolate latency and fault tolerance at runtime  
Karyon & Governator - JVM container service  
Prana - Non JVM runtimes  
Zuul - integrates Hystrix, Eureka, and Ribbon as part of its IPC capabilities.  
Fenzo - Scheduler & resource management  
Prometheus - automated monitoring and alerting  
  Prometheus server - has 3 parts   
  - Storage: stores metric data like current CPU usage, no of exceptions in app, no of request  
  - Data Retrieval worker: responsible to pull data from application/target resources like linux server,apache server, DB  
  Target serviec needs to expose endpoint host:9090/metrics or else use extra component *Exporter*
  - Http Server: receives query, display data on grafana or other data visualization tool  

sleuth - generate and assign unique id with each request.  
zipkin - distributed tracing, runs query  

*Istio is service mesh*  

*service mesh is pattern/paradigm, istio is it's implementation*

Service Mesh - Introduces sidecar to extract all non business logic from microservices and add to sidecar.  
Manages communication between services.Control plane introduces envoy proxy in front of every microservice.  
core feature: traffic splitting(canary deployment) 
1. Control plane: istiod(Pilot, Gally, Citadel)
2. Data plane: envoy proxies

VirtualService: routes traffic to host.  
DestinationRule: define policies on traffic  

Istio ingress gateway - alternative to nginx controller,runs a pod in cluster and acts as loadbalancer, and routes traffic using virtualservices

ingress gateway - incoming traffic
egress gateway - exiting traffic

Flow: request -> Istio gateway -> virtual service -> envoy proxy -> container of microservice

* CMD vs ENTRYPOINT?  
CMD - allows to override the command  
ENTRYPOINT - do not allow to override command instead it appends command



Hystrix - is a latency and fault tolerance library designed to isolate points of access to remote systems, services and 3rd party libraries, stop cascading failure and enable resilience in complex distributed systems where failure is inevitable.
1. Fallback method
2. circuit breaker

#Cloud Bus - Suppose one central property file changes then all microservices needs to restart to solve this cloud bus introduced.
Spring Cloud Bus provides feature to refresh configurations across multiple instances using rabbit MQ publisher
#Spring Cloud Data Flow - is a toolkit to build real-time data integration and data processing pipelines by establishing message flows between Spring Boot applications that could be deployed on top of different runtimes.
#Ribbon - client side load balancing
#Eureka - is the Netflix Service Discovery Server and Client. Eureka Server is using Spring Cloud


-----------------------------
Cloud computing service models - IaaS | PaaS | SaaS

Cloud- huge space online, collection of servers, orchestration
Cloud computing - store remote server,process from anywhere
Cloud services - 
Service models - IaaS | PaaS | SaaS
SaaS - on demand service,independant platform,no need to install on PC,resources managed by vendor.e.g gmail
PaaS - Programming languages + OS + Server + Database, Provides encapsulation, Build compile and run programs, users manage data and app
       resources.
IaaS - provides computing architecture and infrastructure, Data storage + virtualization + servers + networking, vendor manage above 
       resources, users handle data and middleware
cloud providers - rackspace,vmware,DigitalOcean,aws,terremark,joyent,google cloud platform,microsoft azure,IBM cloud.

Kubernetes - deployment(initialization,recovery),scaling(nodes,ram,services,load-balancers),monitoring(health check,manage ssl certificate)


----------------------------------------------------------------
#OAuth
It is an open standard for token-based authentication and authorization on the Internet. It allows an end user's account information to be used by third-party services, such as Facebook, without exposing the user's password.When using OAuth2, grant type is the way an application gets the access token.
Following are the grant types according to OAuth2 specification-
1.  Authorization code grant
2.  Implicit grant
3.  Resource owner credentials grant
4.  Client credentials grant - involves machine to machine authentication.
5.  Refresh token grant

Oauth consists of following actors - 
1.  Resource owners (User) - An entity capable of granting access to a protected resource. When the resource owner is a person, it is
    referred to as an end-user.
2.  Client Application - The machine that needs to be authenticated.
3.  Authorization Server - The server issuing access tokens to the client after successfully authenticating the resource owner and 
    obtaining authorization.
4.  Resource Server - The resource server is the OAuth 2.0 term for your API server. The resource server handles authenticated requests   
    after the application has obtained an access token.
    

* Orchestration engine -   
1. kubernetes
2. docker swarm
3. apache mesos
4. aws ECS


    
    
    
    
    
    





