DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
For personal use only · Recruiter's DevOps Interview Series   Page 1 
 
DEVOPS 
INTERVIEW MASTERY GUIDE 
45 Questions with Detailed Answers 
Covering: CI/CD  ·  Kubernetes  ·  Terraform  ·  AWS  ·  Observability  ·  Security  ·  SRE 
 
● Basic  — Foundational 
knowledge 
● Intermediate  — Applied 
knowledge 
● Advanced  — Deep expertise 
 
SECTION 1  CORE DEVOPS CONCEPTS 
Questions 1–8 · Foundations every DevOps engineer must know 
 
Q1 What is DevOps and what problem does it solve? Basic 
ANS DevOps is a cultural and technical movement that breaks down the traditional silos between 
Development (writing code) and Operations (running infrastructure). Before DevOps, 
developers would write code and 'throw it over the wall' to Ops, who were responsible for 
deploying and maintaining it. This caused slow releases, finger-pointing during outages, and 
misaligned incentives. 
DevOps solves this by unifying the entire software delivery lifecycle — from planning and 
coding to deployment, monitoring, and feedback — under shared ownership, shared tools, and 
shared accountability. The goal is to deliver software faster, more reliably, and with greater 
confidence. 
In practice, DevOps manifests as: automated CI/CD pipelines, infrastructure as code, shared 
on-call responsibility, and a culture of blameless post-mortems. 
KEY ✦ Breaks dev/ops silos     ✦ Shared ownership of delivery     ✦ Faster + more reliable releases     ✦ 
Automation-first mindset 
 
Q2 Explain the CI/CD pipeline and its core stages. Basic 
ANS CI/CD stands for Continuous Integration and Continuous Delivery (or Deployment). It is an 
automated pipeline that takes code from a developer's commit all the way to production with 
minimal human intervention. 
Continuous Integration (CI) covers: Source Control (Git commit triggers the pipeline) → Build 
(compile code, resolve dependencies) → Unit & Integration Tests (catch bugs early) → Static 
Analysis & Security Scan (SonarQube, Snyk) → Artifact Creation (Docker image, JAR, binary). 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
Continuous Delivery (CD) covers: Deploy to Staging (mirror of production) → Automated 
Acceptance Tests (Selenium, Cypress) → Manual Approval Gate (optional) → Deploy to 
Production → Post-deployment Health Checks & Smoke Tests. 
Continuous Deployment removes the manual gate — every green build goes to production 
automatically. This requires very high test coverage and feature flag infrastructure. 
KEY 
✦ CI = build + test + scan     
✦ CD = deploy + verify     
Deployment = no manual gate 
✦ Artifact immutability     
✦ Continuous 
Q3 
What is Infrastructure as Code (IaC) and why does it matter? 
Basic 
ANS Infrastructure as Code is the practice of managing and provisioning computing infrastructure 
(servers, networks, databases, load balancers) through machine-readable configuration files 
rather than manual processes or interactive configuration tools. 
Before IaC, infrastructure was managed manually — sysadmins would SSH into servers, run 
commands, and document changes in wikis (if at all). This led to 'snowflake servers' — unique, 
fragile environments that couldn't be reproduced. 
IaC matters because it brings software engineering practices to infrastructure: version control 
(Git history for every infra change), code review (PRs for infrastructure), testing (validate 
configs before apply), and reproducibility (spin up identical environments in minutes). 
Popular IaC tools: Terraform (multi-cloud, declarative), Pulumi (IaC in real programming 
languages), AWS CloudFormation (AWS-native), Ansible (configuration management), 
Chef/Puppet (legacy configuration management). 
KEY 
✦ Infra in version control     
✦ Reproducible environments     
/ Pulumi / CloudFormation 
✦ No more snowflake servers     
✦ Terraform 
Q4 
What is the difference between Continuous Delivery and Continuous 
Deployment? 
Basic 
ANS Continuous Delivery means every code change that passes automated tests is ready to be 
deployed to production — but deployment is triggered manually by a human decision. The 
pipeline ensures the release is always in a deployable state. 
Continuous Deployment goes one step further: every code change that passes all automated 
tests is automatically deployed to production without any human intervention. There is no 
manual approval gate. 
The right choice depends on business context. Regulated industries (finance, healthcare) often 
require Continuous Delivery with sign-off gates. Fast-moving consumer products (SaaS, mobile 
apps) often prefer Continuous Deployment to ship dozens of times per day. 
Both require the same foundational investment: comprehensive automated testing, feature flags 
for safe rollouts, robust monitoring and alerting, and fast rollback capability. 
KEY 
✦ Delivery = manual trigger     
✦ Deployment = automatic trigger     
✦ Feature flags enable safe auto-deploy 
✦ Both require strong test coverage     
Q5 
What is a blue-green deployment and when would you use it? 
Intermediate 
For personal use only · Recruiter's DevOps Interview Series   Page 2 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
ANS Blue-green deployment is a release strategy that maintains two identical production 
environments — Blue (currently live) and Green (new version being released). Traffic is 
switched from Blue to Green at the load balancer level once Green is verified, making the 
cutover near-instantaneous. 
The key advantage is zero-downtime deployments and trivial rollback — if Green has problems, 
you flip the load balancer back to Blue in seconds. No complex rollback procedure, no data 
migration required (assuming stateless services). 
When to use it: for stateless services where you can't afford downtime, for high-traffic 
applications where gradual rollout isn't sufficient, and for applications that are difficult to roll 
back with traditional deployment methods. 
Considerations: costs double the infrastructure during deployment, requires stateless 
application design or careful session management, and database migrations must be 
backward-compatible since both environments share the same database. 
KEY 
✦ Two identical environments     
✦ Instant cutover at load balancer     
cost during release 
✦ Trivial rollback     
✦ Double infra 
Q6 
Explain canary deployments and how they differ from blue-green. 
Intermediate 
ANS A canary deployment gradually shifts a small percentage of production traffic to the new version 
— say 5% — while 95% still goes to the old version. The new version is monitored for errors, 
latency, and business metrics. If healthy, traffic is progressively increased to 20%, 50%, 100%. 
The name comes from the 'canary in a coal mine' analogy — a small group of real users are the 
first to experience issues, protecting the majority. 
Key differences from blue-green: canary is gradual (minutes to hours) vs. blue-green's instant 
cutover. Canary uses real production traffic to validate, reducing risk on high-traffic systems. 
Blue-green is all-or-nothing. 
Best use case for canary: when you want to test new code on a subset of real users before full 
rollout. Often paired with feature flags (e.g., Unleash, LaunchDarkly) for even finer-grained 
control. Kubernetes natively supports canary with weighted service routing. 
KEY 
✦ Gradual traffic shift     
✦ Real user validation     
well with feature flags 
✦ Requires traffic splitting at LB/service mesh     
✦ Pairs 
Q7 
What is a post-mortem and what makes a good one? 
Basic 
ANS A post-mortem (also called an incident review or retrospective) is a structured analysis 
conducted after a production incident or outage. Its purpose is to understand what happened, 
why it happened, and how to prevent it from happening again. 
A blameless post-mortem is the gold standard — pioneered by Google's SRE team. The 
premise is that people are not the problem; systems, processes, and tooling are. Blaming 
individuals causes under-reporting of incidents and fear of innovation. 
A good post-mortem includes: a clear incident timeline (what happened and when), root cause 
analysis (5 Whys technique to find the real cause, not the symptom), contributing factors, 
impact assessment (users affected, revenue lost, SLA breach), and — most critically — 
concrete action items with owners and due dates. 
The action items are what separate good post-mortems from performative ones. Without follow
through, the same incident recurs. Action items should be specific, measurable, and tracked in 
your project management tool. 
For personal use only · Recruiter's DevOps Interview Series   Page 3 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
KEY 
✦ Blameless culture is critical     
✦ 5 Whys for root cause     
Systems fail, not people 
✦ Concrete action items with owners     
✦ 
Q8 
What are the key DevOps metrics and what do they measure? 
Intermediate 
ANS The DORA (DevOps Research and Assessment) metrics are the industry standard for 
measuring DevOps performance, derived from years of research across thousands of 
organisations. 
Deployment Frequency: how often you deploy to production. Elite performers deploy on
demand (multiple times per day). Low performers deploy once a month or less. This measures 
your ability to deliver value continuously. 
Lead Time for Changes: time from code commit to production. Elite: less than one hour. Low: 1
6 months. This measures the speed of your entire delivery pipeline. 
Change Failure Rate: percentage of deployments that cause a production failure requiring 
remediation. Elite: 0-15%. Low: 46-60%. This measures deployment quality and test coverage. 
Mean Time to Recovery (MTTR): how long it takes to recover from a production failure. Elite: 
less than one hour. Low: 1 week to 1 month. This measures your team's resilience and incident 
response capability. 
Teams that excel at all four DORA metrics consistently show better business outcomes — 
higher revenue growth, customer satisfaction, and employee retention. 
KEY 
✦ DORA: 4 key metrics     ✦ Deployment Frequency + Lead Time = speed     ✦ Change Failure Rate + 
MTTR = stability     
✦ Elite = multiple deploys/day, <1hr MTTR 
SECTION 2  CONTAINERISATION & KUBERNETES 
Questions 9–17 · Docker, K8s, Helm & orchestration at scale 
Q9 
What is Docker and how does it differ from a virtual machine? 
Basic 
ANS Docker is a containerisation platform that packages an application and all its dependencies 
(libraries, runtime, config) into a standardised unit called a container. Containers run on any 
host that has Docker installed, solving the 'it works on my machine' problem. 
The key difference from VMs: a Virtual Machine includes a full guest operating system kernel 
on top of a hypervisor layer — typically 1-10 GB per VM, taking minutes to boot. A Docker 
container shares the host OS kernel and only packages the application layer — typically 10-500 
MB, booting in milliseconds. 
This makes containers far more efficient: you can run dozens of containers on hardware that 
would support only a few VMs. However, containers have weaker isolation than VMs since they 
share the kernel — a kernel vulnerability affects all containers on that host. 
Key Docker concepts: Dockerfile (build instructions), Image (immutable snapshot), Container 
(running instance of an image), Registry (Docker Hub, ECR, GCR for storing images), Volume 
(persistent storage), Network (container-to-container communication). 
KEY 
✦ Shares host OS kernel (vs VM's full OS)     
✦ Milliseconds to start (vs minutes)     
Container = running instance     
✦ Image = immutable, 
✦ Weaker isolation than VMs 
For personal use only · Recruiter's DevOps Interview Series   Page 4 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
Q1
0 
What is Kubernetes and what problem does it solve? 
Basic 
ANS Kubernetes (K8s) is an open-source container orchestration platform originally developed by 
Google. It automates the deployment, scaling, health management, networking, and storage of 
containerised applications across a cluster of machines. 
The problem it solves: Docker tells you how to package and run a single container. But in 
production, you need to run hundreds or thousands of containers across many machines, 
automatically restart failed containers, load balance traffic between them, roll out updates 
without downtime, and scale up or down based on demand. Doing this manually is operationally 
impossible at scale. 
Core Kubernetes concepts: Pod (smallest deployable unit, one or more containers), 
Deployment (declarative way to manage pods, handles rolling updates), Service (stable 
network endpoint for a set of pods), Ingress (HTTP/S routing from outside the cluster), 
ConfigMap/Secret (configuration and sensitive data), Namespace (logical cluster isolation). 
Kubernetes is declarative — you describe the desired state in YAML and Kubernetes 
continuously works to make reality match that state. If a pod dies, Kubernetes automatically 
reschedules it. 
KEY 
✦ Container orchestration at scale     ✦ Self-healing: auto-restarts failed pods     ✦ Declarative desired
state model     
✦ Pod → Deployment → Service → Ingress 
Q1
1 
Explain the difference between a Deployment, StatefulSet, and 
DaemonSet in Kubernetes. 
Intermediate 
ANS A Deployment manages stateless applications. Pods are interchangeable — any pod can 
handle any request, and they can be replaced, scaled, or rescheduled to any node without 
issue. Deployments support rolling updates and rollbacks. Use for: web servers, API services, 
microservices. 
A StatefulSet manages stateful applications where pods have a stable, persistent identity. Each 
pod gets a stable hostname (pod-0, pod-1, pod-2), stable persistent storage 
(PersistentVolumeClaim bound to that specific pod), and ordered, graceful deployment and 
scaling. Use for: databases (MySQL, PostgreSQL, Cassandra), message queues (Kafka, 
ZooKeeper), any app that needs stable network identity. 
A DaemonSet ensures that exactly one copy of a pod runs on every node (or a selected subset 
of nodes) in the cluster. When a new node joins the cluster, the DaemonSet automatically 
schedules the pod on it. When a node is removed, the pod is garbage collected. Use for: log 
collectors (Fluentd, Filebeat), monitoring agents (Prometheus Node Exporter, Datadog Agent), 
network plugins (Calico, Weave). 
Summary: Deployment = stateless, any pod is equal. StatefulSet = stateful, pods have identity. 
DaemonSet = one pod per node, cluster-wide agents. 
KEY 
✦ Deployment = stateless, interchangeable pods     
✦ StatefulSet = stable identity + storage     
DaemonSet = one per node     ✦ Choose based on statefulness 
✦ 
Q1
2 
What is a Kubernetes Ingress and how does it differ from a Service? 
Intermediate 
ANS A Kubernetes Service provides a stable internal IP address and DNS name for a set of pods. It 
load-balances traffic between healthy pods. The three main types: ClusterIP (internal only, 
For personal use only · Recruiter's DevOps Interview Series   Page 5 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
default), NodePort (exposes on each node's IP), and LoadBalancer (provisions a cloud load 
balancer with a public IP — expensive if used for every service). 
An Ingress is an API object that manages external HTTP/HTTPS access to services within the 
cluster. It acts as a smart reverse proxy — routing traffic to different services based on 
hostname (api.example.com → api-service) or URL path (/api → api-service, /auth → auth
service). It also handles TLS termination. 
The key difference: a Service is a basic L4 (TCP/UDP) load balancer. An Ingress is an L7 
(HTTP) routing layer that sits in front of multiple services, providing host/path-based routing, 
SSL termination, and often rate limiting and authentication — all with a single external IP/load 
balancer. 
Ingress requires an Ingress Controller to be installed (e.g., nginx-ingress, Traefik, AWS ALB 
Ingress Controller). The Ingress resource is just the rules — the controller is the 
implementation. 
KEY 
✦ Service = L4, stable pod endpoint     
✦ Ingress = L7, host/path routing     
one LB for all     
✦ Requires Ingress Controller (nginx, Traefik) 
✦ Ingress = TLS termination + 
Q1
3 
How does Kubernetes handle pod scheduling? 
Intermediate 
ANS The Kubernetes Scheduler watches for newly created pods that have no node assigned. For 
each pod, it runs a two-phase process: Filtering (find all nodes that meet the pod's 
requirements) and Scoring (rank eligible nodes to find the best one). 
Filtering considers: resource requests (does the node have enough CPU/memory?), node 
affinity/anti-affinity rules, taints and tolerations (nodes can be 'tainted' to repel pods that don't 
explicitly tolerate the taint — useful for dedicated nodes), pod affinity (co-locate pods), pod anti
affinity (spread pods across nodes for HA). 
Scoring ranks nodes by factors like: balanced resource utilisation, data locality, spreading pods 
across failure domains (zones, racks). 
Resource requests vs limits: requests are what Kubernetes uses for scheduling decisions 
(guarantee this pod gets at least X CPU/memory). Limits are the maximum the container can 
use. Setting requests without limits can lead to noisy-neighbour problems. Best practice: always 
set both, and set requests = limits for critical workloads (Guaranteed QoS class). 
KEY 
✦ Filter then Score     
✦ Resource requests drive scheduling     
✦ Affinity rules = co-locate or spread pods 
✦ Taints + Tolerations = dedicated nodes     
Q1
4 
What is a Helm chart and why is it used? 
Intermediate 
ANS Helm is the package manager for Kubernetes. A Helm chart is a collection of YAML template 
files that describe a Kubernetes application. Charts parameterise your manifests with a 
values.yaml file, making it easy to deploy the same application with different configurations 
across environments (dev, staging, production) without duplicating YAML. 
Without Helm, managing a complex application (say, a microservice with a Deployment, 
Service, Ingress, ConfigMap, HPA, ServiceAccount, and NetworkPolicy) means maintaining 7+ 
separate YAML files. For 20 microservices, that's 140+ files, all slightly different per 
environment. 
For personal use only · Recruiter's DevOps Interview Series   Page 6 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
With Helm: one chart, one values file per environment. helm install, helm upgrade, helm 
rollback. Values can override anything — image tag, replica count, resource limits, feature 
flags. 
Helm also manages releases: it tracks what version of a chart is deployed in each namespace, 
supports atomic upgrades (rolls back automatically on failure with --atomic flag), and integrates 
with CI/CD pipelines via helm upgrade --install. 
Helm Hub / Artifact Hub hosts thousands of community charts (PostgreSQL, Redis, Nginx, 
Prometheus) so you rarely write charts from scratch for third-party software. 
KEY 
✦ Kubernetes package manager     
✦ Templated YAML + values.yaml per env     
install/upgrade/rollback     
✦ helm 
✦ Charts for 3rd-party apps on Artifact Hub 
Q1
5 
Explain Kubernetes resource limits, requests, and QoS classes. 
Advanced 
ANS Resource Requests are the minimum resources guaranteed to a container. The scheduler uses 
requests to decide which node can run the pod. If a node has 4 CPU available and your pod 
requests 2 CPU, the scheduler places it there — even if the container rarely uses more than 0.1 
CPU. 
Resource Limits are the maximum resources a container is allowed to use. For CPU, exceeding 
the limit causes throttling (not termination). For memory, exceeding the limit causes the 
container to be OOMKilled (Out of Memory Killed) and restarted. 
Kubernetes assigns one of three QoS classes based on how requests and limits are set: 
Guaranteed (requests == limits for all containers — highest priority, last to be evicted), 
Burstable (limits > requests or only limits set — medium priority), BestEffort (no requests or 
limits set — first to be evicted under memory pressure). 
Best practice: always set both CPU and memory requests and limits. For production critical 
workloads, set requests == limits (Guaranteed QoS). Use a LimitRange object in your 
namespace to set defaults for pods that don't specify resources. Use Vertical Pod Autoscaler 
(VPA) to right-size recommendations based on actual usage. 
KEY 
✦ Requests = scheduling guarantee     ✦ Limits = max (CPU throttle, memory OOMKill)     
> Burstable > BestEffort eviction order     
✦ Guaranteed 
✦ Always set both in production 
Q1
6 
What is a service mesh and when would you use one? 
Advanced 
ANS A service mesh is an infrastructure layer that handles service-to-service communication in a 
microservices architecture. It is implemented as a sidecar proxy injected into every pod 
(typically Envoy proxy), creating a transparent network layer that intercepts all traffic between 
services. 
A service mesh provides: mTLS (mutual TLS for service-to-service encryption and identity), 
advanced traffic management (weighted routing, circuit breaking, retries, timeouts at the mesh 
level), observability (distributed tracing, metrics, and logs for every service call without code 
changes), and policy enforcement (rate limiting, access control between services). 
Popular service meshes: Istio (most feature-rich, complex), Linkerd (lightweight, simpler), 
Consul Connect (HashiCorp ecosystem), AWS App Mesh (managed). 
When to use it: when you have 10+ microservices with complex inter-service communication 
requirements, when you need zero-trust networking (mTLS everywhere), or when you want 
unified observability without instrumenting every service individually. 
For personal use only · Recruiter's DevOps Interview Series   Page 7 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
When NOT to use it: small teams, simple architectures, or when you're just starting with 
Kubernetes. The operational overhead of a service mesh is significant — it adds latency, 
complexity, and requires dedicated expertise. 
KEY 
✦ Sidecar proxy (Envoy) on every pod     
✦ mTLS, traffic management, observability     
vs Linkerd (simple)     
✦ Only justified at scale (10+ services) 
✦ Istio (complex) 
Q1
7 
How does horizontal pod autoscaling (HPA) work in Kubernetes? 
Intermediate 
ANS The Horizontal Pod Autoscaler automatically scales the number of pod replicas in a 
Deployment or StatefulSet based on observed metrics. It runs a control loop (default every 15 
seconds) that compares the current metric value against the target, and adjusts replica count 
accordingly. 
Classic HPA scales on CPU utilisation: if target is 50% CPU and current usage is 80%, HPA 
calculates the desired replicas as: current_replicas × (current_metric / target_metric) = e.g., 4 × 
(80/50) = ~6.4 → rounds up to 7 replicas. 
HPA v2 supports custom metrics (from Prometheus via the custom metrics API) and external 
metrics (queue depth in SQS/RabbitMQ, requests per second from an APM tool). This enables 
much more business-relevant scaling — scale your order processor on queue depth, not CPU. 
HPA has scale-down stabilisation (default 5 minutes) to prevent flapping — it waits before 
scaling down to avoid killing pods during brief traffic dips. Scale-up is intentionally fast (no 
stabilisation by default). 
Pair HPA with Cluster Autoscaler (scales nodes) for full elasticity: HPA scales pods, Cluster 
Autoscaler adds nodes when pods can't be scheduled due to insufficient node capacity. 
KEY 
✦ Control loop every 15s     
✦ Scales on CPU, custom, or external metrics     
5 min default     
✦ HPA (pods) + Cluster Autoscaler (nodes) 
✦ Scale-down stabilisation = 
SECTION 3  CLOUD & INFRASTRUCTURE 
Questions 18–26 · AWS, Terraform, networking & databases 
Q1
8 
What is Terraform and how does the state file work? 
Intermediate 
ANS Terraform is a declarative IaC tool by HashiCorp that allows you to define cloud infrastructure in 
HCL (HashiCorp Configuration Language). You describe what you want (resources, their 
configuration, dependencies), and Terraform figures out how to create, update, or delete 
resources to match that desired state. 
The Terraform workflow: terraform init (download providers and modules) → terraform plan 
(show what will change — the diff between current state and desired state) → terraform apply 
(execute the changes) → terraform destroy (tear down everything). 
The state file (terraform.tfstate) is Terraform's source of truth about what it has created. It maps 
your HCL resources to real-world infrastructure IDs. Without the state file, Terraform doesn't 
know what it owns and would try to recreate everything. 
For personal use only · Recruiter's DevOps Interview Series   Page 8 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
Critical best practices for state: never store state locally in teams — use remote backends (S3 + 
DynamoDB for AWS, GCS for GCP, Terraform Cloud). Enable state locking to prevent 
concurrent applies. Enable state encryption (sensitive values like passwords are stored in 
state). Never edit state manually — use terraform state commands. 
State can be split into multiple files using workspaces or separate state backends per 
environment — this limits the blast radius of a terraform apply gone wrong. 
KEY 
✦ Declarative HCL, provider-agnostic     
✦ State file = source of truth for infra     
locking for teams     
✦ Plan before every apply 
✦ Remote backend + 
Q1
9 
What is the difference between AWS Auto Scaling and Kubernetes 
HPA? 
Intermediate 
ANS AWS Auto Scaling (EC2 Auto Scaling Groups) operates at the VM/instance level. It adds or 
removes entire EC2 instances from a fleet based on CloudWatch metrics (CPU, network, 
custom). Scaling takes 2-5 minutes because launching a new EC2 instance takes time. 
Kubernetes HPA operates at the pod/container level. It adds or removes pods within the 
existing cluster nodes in seconds. It's much faster but bounded by available node capacity — if 
all nodes are full, new pods will be Pending until the Cluster Autoscaler adds nodes. 
In a typical EKS (Kubernetes on AWS) setup, both work together in a layered architecture: HPA 
rapidly scales pods horizontally within available capacity (seconds). Cluster Autoscaler triggers 
when pods can't be scheduled (adds EC2 instances via ASG, ~3-5 min). This gives you fast 
response (pods) with elastic capacity (nodes). 
For serverless container workloads, AWS Fargate + HPA removes the node management 
entirely — Fargate provisions compute per pod, so Cluster Autoscaler isn't needed. 
KEY 
✦ ASG = VM level (2-5 min to scale)     
✦ HPA = pod level (seconds)     
Autoscaler elastic     
✦ Fargate removes node management 
✦ Together: HPA fast + Cluster 
Q2
0 
Explain VPC, subnets, and the difference between public and private 
subnets. 
Basic 
ANS A VPC (Virtual Private Cloud) is a logically isolated section of the cloud where you launch your 
resources. It has its own IP address range (CIDR block, e.g., 10.0.0.0/16), routing tables, 
internet gateways, and security controls. It is your private data centre in the cloud. 
Subnets are subdivisions of a VPC's IP range, tied to a specific Availability Zone. A VPC 
spanning 10.0.0.0/16 might be divided into subnets like 10.0.1.0/24 (AZ-A), 10.0.2.0/24 (AZ-B), 
etc. 
A public subnet has a route to an Internet Gateway in its route table — resources in it can 
send/receive traffic from the internet directly. Public subnets hold: load balancers, NAT 
gateways, bastion hosts. 
A private subnet has NO route to an Internet Gateway. Resources cannot be reached from the 
internet. They can reach the internet outbound only via a NAT Gateway in a public subnet (for 
software updates, API calls). Private subnets hold: application servers, databases, Kubernetes 
worker nodes, internal services. 
Best practice: multi-AZ design with public + private subnets in each AZ. Never put databases in 
public subnets. Use Security Groups and NACLs for fine-grained traffic control. 
KEY 
✦ VPC = isolated network in cloud     
✦ Public subnet = has internet gateway route     
no inbound internet     
✦ Databases always in private subnets 
✦ Private subnet = 
For personal use only · Recruiter's DevOps Interview Series   Page 9 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
Q2
1 
What is a NAT Gateway and why is it needed? 
Basic 
ANS A NAT (Network Address Translation) Gateway allows resources in a private subnet to initiate 
outbound connections to the internet while preventing the internet from initiating connections to 
those resources. 
Without a NAT Gateway, instances in a private subnet are completely cut off from the internet 
— they can't download software packages, reach external APIs, or send data to external 
services. This is too restrictive for most applications. 
How it works: a private instance sends a packet to 8.8.8.8. The packet goes to the NAT 
Gateway (in a public subnet), which replaces the source IP with its own public IP, forwards the 
packet, receives the response, and sends it back to the private instance. The private instance is 
never exposed. 
NAT Gateway vs NAT Instance: AWS-managed NAT Gateway is highly available, scales 
automatically, costs ~$0.045/hour + data processing. A NAT Instance is a self-managed EC2 
instance — cheaper but requires patching, HA setup, and manual scaling. In modern 
architectures, always use NAT Gateway. 
Cost optimization: NAT Gateway charges per GB of data processed. For high-volume scenarios 
(e.g., pulling Docker images, large data transfers), use VPC Endpoints to AWS services (S3, 
ECR) to bypass the NAT Gateway entirely. 
KEY 
✦ Outbound internet for private resources     
✦ Prevents inbound internet connections     
= managed, scalable     
✦ Use VPC Endpoints to reduce NAT costs 
✦ NAT Gateway 
Q2
2 
How does AWS IAM work and what are best practices? 
Intermediate 
ANS AWS IAM (Identity and Access Management) controls who can do what to which AWS 
resources. It has four core components: Users (human identities), Groups (collections of users), 
Roles (assumed by services, EC2 instances, Lambda functions, cross-account access), and 
Policies (JSON documents defining permissions). 
IAM is additive and deny-by-default — no permissions exist unless explicitly granted. An explicit 
Deny always overrides an Allow, even if another policy grants access. 
Best practices: never use root account for day-to-day work (enable MFA, create an admin 
user). Apply the Principle of Least Privilege — grant only the permissions needed, nothing 
more. Use IAM Roles for applications (never long-lived access keys in code). Use Groups to 
manage permissions at scale (don't attach policies directly to users). Rotate access keys 
regularly. Enable CloudTrail for audit logging of all API calls. 
Service control policies (SCPs) in AWS Organizations allow you to set permission guardrails 
across all accounts — e.g., prevent anyone in the organisation from disabling CloudTrail, or 
restrict which regions services can be deployed in. 
For Kubernetes on AWS (EKS), use IRSA (IAM Roles for Service Accounts) to bind IAM roles 
to Kubernetes service accounts — pods get fine-grained AWS permissions without node-level 
IAM roles. 
KEY 
✦ Deny by default, explicit grants     
✦ Roles > access keys for services     
always     
✦ IRSA for Kubernetes on EKS 
✦ Principle of Least Privilege 
For personal use only · Recruiter's DevOps Interview Series   Page 10 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
Q2
3 
What is a load balancer and what are the types in AWS? 
Basic 
ANS A load balancer distributes incoming traffic across multiple targets (EC2 instances, containers, 
IP addresses) to ensure no single target is overwhelmed, improve availability, and enable zero
downtime deployments. 
AWS offers three types under the Elastic Load Balancing service. Application Load Balancer 
(ALB) operates at Layer 7 (HTTP/HTTPS). It routes based on URL path, hostname, headers, 
and query strings. It integrates with ECS, EKS, WAF, and Cognito. Best for web applications, 
microservices, REST APIs. 
Network Load Balancer (NLB) operates at Layer 4 (TCP/UDP/TLS). It handles millions of 
requests per second with extremely low latency (microseconds). It preserves the client IP. Best 
for TCP workloads, gaming, IoT, real-time data streams, and any application that needs ultra
high performance. 
Classic Load Balancer (CLB) is the legacy option supporting both L4 and L7 — AWS 
recommends migrating all CLBs to ALB or NLB. Gateway Load Balancer (GWLB) is a newer 
type for deploying, scaling, and managing third-party virtual network appliances (firewalls, 
IDS/IPS). 
For Kubernetes on EKS: ALB Ingress Controller creates ALBs for Ingress resources. NLB is 
used for Service type=LoadBalancer for high-performance TCP workloads. 
KEY 
✦ ALB = L7 (HTTP), path/host routing     
✦ NLB = L4 (TCP), ultra-low latency     
for TCP/gaming     
✦ Avoid Classic LB (legacy) 
✦ ALB for web/API, NLB 
Q2
4 
What is Terraform's remote state and why is it critical for teams? 
Intermediate 
ANS By default, Terraform stores state locally in a terraform.tfstate file on the machine running 
terraform apply. This works for solo developers but breaks in team environments. 
Remote state stores the state file in a shared backend — common choices are S3 (with 
DynamoDB for locking) on AWS, GCS on GCP, or Terraform Cloud. All team members and 
CI/CD pipelines read and write to the same state. 
State locking prevents concurrent applies from corrupting the state file. With DynamoDB 
locking, when one process runs terraform apply, it writes a lock entry. Any other process 
attempting an apply will fail with a lock error until the first apply completes. 
State encryption is critical because the state file contains sensitive values in plaintext — RDS 
passwords, API keys, TLS certificates. S3 backend supports SSE (Server-Side Encryption) and 
supports KMS customer-managed keys for compliance. 
Terraform workspaces vs separate backends: workspaces use the same backend with a prefix 
(terraform.tfstate.d/staging). Separate backends (separate S3 buckets/prefixes per 
environment) provide stronger isolation and blast radius control. Most teams prefer separate 
backends for prod vs non-prod. 
KEY 
✦ Never local state in teams     
✦ S3 + DynamoDB = remote state + locking     
secrets — encrypt     
✦ Separate backends per environment 
✦ State contains plaintext 
Q2
5 
How does DNS work and what is Route 53 in AWS? 
Basic 
For personal use only · Recruiter's DevOps Interview Series   Page 11 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
ANS DNS (Domain Name System) translates human-readable domain names (api.example.com) 
into IP addresses (52.12.34.56). When your browser requests api.example.com, it queries a 
chain of DNS servers: local cache → recursive resolver (ISP) → root nameserver → TLD 
nameserver (.com) → authoritative nameserver (your DNS provider) → returns the IP. 
DNS record types: A (maps domain to IPv4), AAAA (IPv6), CNAME (alias one domain to 
another), MX (email servers), TXT (verification, SPF, DKIM), NS (nameservers), SOA (zone 
authority), SRV (service location). 
Route 53 is AWS's managed DNS service (also a domain registrar). It is globally distributed 
across 100+ PoPs with 100% SLA. Beyond basic DNS, Route 53 supports: Routing Policies — 
Simple (single resource), Weighted (A/B testing by splitting traffic %, canary deploys), Latency
based (route to lowest-latency region), Failover (active/passive HA — health-check-driven), 
Geolocation (route by user's country/continent), Geoproximity, Multi-value (random across 
healthy records). 
Route 53 Health Checks monitor endpoints and automatically remove unhealthy records from 
DNS responses — enabling DNS-level failover across regions in under 60 seconds. 
KEY 
✦ A/CNAME/MX/TXT key record types     ✦ Route 53: weighted, latency, failover routing     
checks = DNS-level failover     
✦ Health 
✦ DNS TTL controls caching duration 
Q2
6 
What is the difference between SQL and NoSQL databases and when 
to use each? 
Basic 
ANS SQL (relational) databases store data in tables with predefined schemas, use SQL for queries, 
support ACID transactions, and excel at complex joins and relationships. Examples: 
PostgreSQL, MySQL, AWS RDS, Google Cloud SQL. 
NoSQL databases use flexible schemas and various data models: document (MongoDB, 
DynamoDB), key-value (Redis, DynamoDB), column-family (Cassandra, HBase), graph (Neo4j, 
Amazon Neptune). They trade ACID guarantees for horizontal scalability and schema flexibility. 
Use SQL when: data has clear relationships and structure, you need complex queries and joins, 
ACID transactions are required (financial data, inventory), data integrity is paramount. 
Use NoSQL when: you need massive horizontal scale (billions of records), data is unstructured 
or evolves rapidly (user profiles, product catalogs), you need extremely low latency key-value 
lookups (Redis for caching), or you have time-series / event data (Cassandra, InfluxDB). 
In modern architectures, polyglot persistence is common — use PostgreSQL for your core 
transactional data, Redis for caching and session storage, DynamoDB for high-throughput 
event logging, and Elasticsearch for full-text search. Choose the right tool for each access 
pattern. 
KEY 
✦ SQL = ACID, joins, structured schema     
✦ NoSQL = scale, flexible, varied models     
persistence in modern systems     
✦ Polyglot 
✦ Redis for cache, Cassandra for time-series 
SECTION 4  MONITORING, OBSERVABILITY & SECURITY 
Questions 27–36 · Prometheus, tracing, secrets & DevSecOps 
Q2
7 
What is the difference between monitoring and observability? 
Intermediate 
For personal use only · Recruiter's DevOps Interview Series   Page 12 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
ANS Monitoring is the practice of collecting predefined metrics and alerting when they cross 
thresholds. It answers known questions: 'Is CPU above 80%? Is the API returning 500 errors?' 
Monitoring tells you when something is wrong but not necessarily why. 
Observability is the ability to understand the internal state of a system from its external outputs 
(logs, metrics, traces). An observable system lets you ask arbitrary questions about its 
behaviour — even questions you didn't anticipate when building the system. It answers 
unknown unknowns. 
The three pillars of observability are: Logs (discrete events with context — 'user 123 failed 
authentication at 14:32:05'), Metrics (aggregated numerical measurements over time — 'p99 
latency over the last 5 minutes'), and Traces (end-to-end request flows across multiple services 
— showing exactly where a request spent its time). 
In practice: monitoring catches 'the server is down', observability helps you understand 'why did 
this specific user's checkout fail even though the server appeared healthy?' Tools: Prometheus 
+ Grafana (metrics), ELK Stack or Loki (logs), Jaeger or Tempo (traces), Datadog/New Relic 
(all-in-one). 
KEY 
✦ Monitoring = known unknowns     ✦ Observability = unknown unknowns     ✦ Three pillars: Logs, 
Metrics, Traces     
✦ Prometheus + Grafana + Jaeger = full stack 
Q2
8 
How does Prometheus work and what is its data model? 
Intermediate 
ANS Prometheus is an open-source monitoring system originally built at SoundCloud, now a CNCF 
graduated project. It works on a pull model — Prometheus actively scrapes (HTTP GET) 
metrics endpoints from configured targets at regular intervals (default every 15 seconds). 
Targets expose metrics at /metrics in a plain-text format. 
The data model: every time series is uniquely identified by its metric name and a set of key
value labels. For example: http_requests_total{method='GET', status='200', service='checkout'} 
= 42847. Labels enable powerful multi-dimensional filtering and aggregation. 
Prometheus stores data in a local time-series database (TSDB) optimised for high write 
throughput and efficient range queries. For long-term storage at scale, use remote write to 
Thanos, Cortex, or Victoria Metrics. 
PromQL (Prometheus Query Language) is used to query metrics: rate(http_requests_total[5m]) 
gives per-second request rate over 5 minutes. histogram_quantile(0.99, 
rate(http_request_duration_seconds_bucket[5m])) gives p99 latency. 
Alertmanager handles routing, deduplication, grouping, and silencing of alerts. It supports 
sending to Slack, PagerDuty, email, OpsGenie. Alerting rules are defined in YAML and 
evaluated by Prometheus against PromQL expressions. 
KEY 
✦ Pull model — scrapes /metrics endpoints     
✦ Labels = multi-dimensional data model     
querying     
✦ Alertmanager for routing alerts 
✦ PromQL for 
Q2
9 
What is distributed tracing and how does it work? 
Advanced 
ANS Distributed tracing tracks a single request as it flows through multiple microservices, giving you 
a complete picture of its journey — which services it called, how long each took, and where 
failures occurred. This is essential in microservice architectures where a single user action may 
touch 10+ services. 
For personal use only · Recruiter's DevOps Interview Series   Page 13 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
How it works: when a request enters the system, a unique Trace ID is generated. As the 
request passes through each service, each service creates a Span (a named, timed operation) 
with the Trace ID, a Span ID, and the parent Span ID. Services propagate the Trace ID in HTTP 
headers (W3C TraceContext standard: traceparent header) or gRPC metadata. 
All spans are collected by a tracing backend (Jaeger, Zipkin, Tempo) and visualised as a 
waterfall diagram showing: total request duration, time spent in each service, sequential vs. 
parallel calls, and exactly where errors or slowness occurred. 
OpenTelemetry (OTel) is the CNCF standard for instrumentation — a vendor-neutral SDK for 
generating traces, metrics, and logs. Instrument once with OTel, send to any backend (Jaeger, 
Datadog, Honeycomb, Grafana Tempo). This is now the industry default approach. 
Sampling: tracing every request in high-traffic systems is expensive. Use head-based sampling 
(sample at entry point, e.g., 10% of requests) or tail-based sampling (sample based on 
outcomes — always sample errors and slow requests, sample a small % of healthy fast 
requests). 
KEY 
✦ Trace ID propagated across all services     
✦ Span = named operation with parent reference     
✦ 
OpenTelemetry = vendor-neutral instrumentation     ✦ Tail-based sampling for efficient tracing 
Q3
0 
What is the ELK stack and how is it used for log management? 
Intermediate 
ANS ELK stands for Elasticsearch, Logstash, and Kibana — a popular open-source stack for 
centralised log management and analysis. Often extended to the Elastic Stack with Beats 
(lightweight shippers). 
Elasticsearch is a distributed search and analytics engine based on Apache Lucene. It stores 
logs as JSON documents, indexes them for full-text search, and handles queries across 
terabytes of log data. It scales horizontally by adding nodes. 
Logstash is a data processing pipeline — it ingests logs from multiple sources (files, syslog, 
Kafka, Beats), transforms them (parse, filter, enrich, mask sensitive data using grok patterns), 
and ships them to Elasticsearch or other outputs. 
Kibana is the visualisation layer — it provides a web UI for searching logs (Discover), building 
dashboards (visualise metrics over time), creating alerts, and using ML features (anomaly 
detection). 
In modern deployments, Filebeat or Fluentd/Fluent Bit (lighter, Kubernetes-native) replace 
Logstash for log shipping. Loki (by Grafana Labs) is a popular lightweight alternative to 
Elasticsearch for log aggregation — it indexes only metadata (labels), not the full text, making it 
far cheaper to operate in Kubernetes environments. 
KEY 
✦ Elasticsearch (store) + Logstash (process) + Kibana (visualise)     
shipping     
✦ Beats / Fluentd for Kubernetes log 
✦ Loki = lightweight K8s alternative     
✦ grok patterns for log parsing 
Q3
1 
What is DevSecOps and how do you shift security left? 
Intermediate 
ANS DevSecOps integrates security practices into every stage of the DevOps pipeline, rather than 
treating security as a gate at the end of the release process. 'Shifting left' means moving 
security checks earlier in the development lifecycle — to the developer's IDE and the CI 
pipeline — where fixing issues is orders of magnitude cheaper than fixing them in production. 
Shift-left security practices in the CI/CD pipeline: SAST (Static Application Security Testing) — 
tools like SonarQube, Semgrep, Checkmarx scan source code for vulnerabilities without 
For personal use only · Recruiter's DevOps Interview Series   Page 14 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
executing it. SCA (Software Composition Analysis) — tools like Snyk, Dependabot, OWASP 
Dependency Check scan third-party libraries for known CVEs. Container image scanning — 
Trivy, Grype, Clair scan Docker images for OS and package vulnerabilities before pushing to 
registry. Secrets scanning — GitLeaks, TruffleHog prevent credentials from being committed to 
Git. 
Infrastructure security: enforce IaC security policies with tfsec or Checkov (scan Terraform for 
misconfigurations before apply). Use OPA (Open Policy Agent) with Gatekeeper in Kubernetes 
to enforce security policies (e.g., no privileged containers, all images must come from approved 
registries). 
DAST (Dynamic Application Security Testing) runs against a running application — OWASP 
ZAP, Burp Suite. This is the 'right' side of the pipeline but still automated. 
KEY 
✦ Shift security to dev + CI, not just pre-prod     
✦ SAST (code) + SCA (deps) + image scan     
scanning prevents credential leaks     
✦ Secrets 
✦ OPA/Gatekeeper for K8s policy enforcement 
Q3
2 
What is mTLS and why is it important in microservices? 
Advanced 
ANS mTLS (Mutual TLS) is an extension of standard TLS where both the client and the server 
authenticate each other using certificates. In standard TLS, only the server proves its identity 
(HTTPS). In mTLS, the client also presents a certificate, proving its identity to the server. 
In a microservices architecture without mTLS, services trust any caller on the internal network. 
A compromised service could impersonate any other service and call any API. The internal 
network is not a trust boundary. 
With mTLS, every service has a certificate signed by an internal Certificate Authority (CA). 
When service A calls service B, B verifies A's certificate before accepting the request. This 
enables: service-to-service authentication (zero-trust networking), encryption of all internal 
traffic (prevents eavesdropping on the internal network), and authorization based on service 
identity. 
Implementing mTLS manually (generating certificates, distributing them, rotating them) is 
complex. Service meshes (Istio, Linkerd) automate the entire mTLS lifecycle — they inject 
sidecar proxies that handle certificate issuance, rotation, and enforcement transparently, with 
no application code changes required. 
SPIFFE/SPIRE is the CNCF standard for workload identity in zero-trust environments — it 
assigns cryptographic identities (SVIDs) to workloads and is used by Istio and other service 
meshes under the hood. 
KEY 
✦ Both parties authenticate with certificates     
✦ Zero-trust: no implicit internal trust     
automate mTLS lifecycle     
✦ Service meshes 
✦ SPIFFE/SPIRE for workload identity 
Q3
3 
What is RBAC and how is it implemented in Kubernetes? 
Intermediate 
ANS RBAC (Role-Based Access Control) is a security model that grants permissions to roles rather 
than to individual users or service accounts, and then assigns roles to subjects. It implements 
the principle of least privilege at scale. 
Kubernetes RBAC has four key API objects: Role (grants permissions within a namespace — 
verbs on resources, e.g., get/list/watch pods in namespace 'production'), ClusterRole (same, 
but cluster-wide — covers non-namespaced resources like nodes), RoleBinding (binds a Role 
For personal use only · Recruiter's DevOps Interview Series   Page 15 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
to a subject — User, Group, or ServiceAccount — within a namespace), ClusterRoleBinding 
(binds a ClusterRole to a subject cluster-wide). 
Example: a CI/CD pipeline service account needs to deploy to the 'production' namespace. 
Create a Role in 'production' that allows get/list/watch/create/update/patch/delete on 
deployments. Create a RoleBinding that binds this Role to the pipeline's ServiceAccount. The 
pipeline can now deploy to production — and nothing else. 
Best practices: never use cluster-admin for applications. Create specific roles per team/pipeline 
with minimum required permissions. Audit RBAC with kubectl auth can-i commands and tools 
like rbac-lookup. Use OPA/Gatekeeper to enforce that new service accounts can't be granted 
admin permissions without approval. 
KEY 
✦ Role + RoleBinding (namespaced)     
✦ ClusterRole + ClusterRoleBinding (global)     
ServiceAccount for workloads     
✦ Bind to 
✦ Audit with kubectl auth can-i 
Q3
4 
How do you handle secrets management in a DevOps pipeline? 
Intermediate 
ANS The number one rule: never store secrets (passwords, API keys, certificates, tokens) in source 
code or Dockerfiles — even in private repositories. Use Git scanning tools (GitLeaks, 
TruffleHog) as a pre-commit hook and in CI to catch accidental commits. 
Secrets management solutions: HashiCorp Vault is the most comprehensive — it centralises 
secrets storage, provides dynamic secrets (generates short-lived DB credentials on demand), 
audit logging, and fine-grained access policies. AWS Secrets Manager and Parameter Store 
(for less sensitive config) are managed alternatives on AWS. GCP Secret Manager, Azure Key 
Vault for their respective clouds. 
In Kubernetes: use Kubernetes Secrets (base64-encoded, NOT encrypted by default — enable 
etcd encryption at rest). For production, use External Secrets Operator to sync secrets from 
Vault/AWS Secrets Manager into Kubernetes Secrets automatically. This means the source of 
truth is always the secrets manager, not Kubernetes. 
In CI/CD: inject secrets as environment variables from the secrets manager at pipeline 
execution time. Never print secrets in logs. Use masked variables in GitLab CI / GitHub Actions 
secrets. Rotate secrets regularly and automate rotation with Vault's dynamic secrets. 
The principle: secrets should be short-lived, automatically rotated, and audited. Every access to 
a secret should be logged. 
KEY 
✦ Never in code or Docker — ever     
✦ Vault / AWS Secrets Manager for centralization     
Secrets Operator for Kubernetes     
✦ External 
✦ Short-lived, automatically rotated secrets 
Q3
5 
What is chaos engineering and how does it relate to resilience? 
Advanced 
ANS Chaos engineering is the practice of deliberately introducing failures into production (or 
production-like) systems to test their resilience and uncover weaknesses before they cause real 
outages. It turns the question from 'will our system fail?' to 'how gracefully does our system fail, 
and can we detect and recover quickly?' 
Pioneered by Netflix with the Chaos Monkey tool (randomly kills production EC2 instances), 
chaos engineering has evolved into a discipline. The process: define the steady state 
(measurable normal behaviour — p99 latency < 100ms, error rate < 0.1%), hypothesise that the 
system will maintain steady state under a specific failure, introduce the chaos experiment (kill a 
For personal use only · Recruiter's DevOps Interview Series   Page 16 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
pod, inject network latency, saturate CPU, drop a database connection), observe if steady state 
is maintained, and address any gaps discovered. 
Types of chaos experiments: infrastructure (kill nodes, AZ outages), network (packet loss, 
latency, DNS failures), application (kill pods, memory leaks, slow third-party APIs), data 
(database failover, cache eviction). 
Tools: Chaos Monkey (AWS instances), Chaos Mesh (Kubernetes — pod kill, network chaos, 
IO chaos), LitmusChaos (CNCF, Kubernetes-native), Gremlin (enterprise, managed chaos). 
Chaos engineering should be practised regularly — not just during gamedays. It builds 
confidence in distributed systems and directly improves MTTR because teams learn how their 
systems behave under failure conditions before a real incident. 
KEY 
✦ Deliberate failure injection in prod     
✦ Define steady state before experimenting     
✦ Chaos Mesh / 
LitmusChaos for Kubernetes     ✦ Improves MTTR + team confidence 
Q3
6 
What is a WAF and how does it protect web applications? 
Intermediate 
ANS A WAF (Web Application Firewall) is a security appliance (hardware or software) that monitors, 
filters, and blocks HTTP/HTTPS traffic to and from a web application. It operates at Layer 7 and 
understands the HTTP protocol — unlike a network firewall which only looks at IP addresses 
and ports. 
A WAF protects against the OWASP Top 10 vulnerabilities: SQL injection (malicious SQL in 
form fields), Cross-Site Scripting (XSS — injecting JavaScript into pages), Cross-Site Request 
Forgery (CSRF), broken access control, and more. It inspects request headers, bodies, URLs, 
and query strings for malicious patterns. 
AWS WAF integrates with ALB, CloudFront, and API Gateway. It supports: managed rule 
groups (AWS-curated rules for common attacks, bot detection, known bad IPs), rate limiting 
(block IPs making more than X requests per minute — DDoS protection), custom rules (block 
specific IPs, countries, request patterns), and Bot Control (detect and manage bot traffic). 
WAF limitations: it's not a silver bullet. It can have false positives (blocking legitimate traffic), 
can be bypassed by sophisticated attackers, and doesn't replace secure coding practices. A 
WAF is a defence-in-depth layer, not the primary security control. 
Pair WAF with: DDoS protection (AWS Shield), CDN for rate limiting at edge (CloudFront), 
DAST in CI/CD, and regular penetration testing. 
KEY 
✦ L7 firewall — inspects HTTP content     ✦ Protects against OWASP Top 10     ✦ AWS WAF: managed 
rules + rate limiting     
✦ Defence-in-depth — not a replacement for secure code 
SECTION 5  ADVANCED TOPICS 
Questions 37–45 · GitOps, SRE, resilience patterns & eBPF 
Q3
7 
What is GitOps and how does it differ from traditional CI/CD? 
Advanced 
ANS GitOps is an operational framework that uses Git as the single source of truth for both 
application code and infrastructure configuration. The desired state of the entire system 
(application manifests, Kubernetes configs, Helm values) is stored in Git. An automated agent 
For personal use only · Recruiter's DevOps Interview Series   Page 17 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
continuously compares the actual cluster state to the desired state in Git and reconciles any 
drift. 
Traditional CI/CD is push-based: a CI system (Jenkins, GitHub Actions) detects a code change, 
runs tests, builds an artifact, and pushes it to the cluster via kubectl apply or helm upgrade. The 
CI system needs cluster credentials and direct access to the cluster. 
GitOps is pull-based: an agent running inside the cluster (ArgoCD, Flux) watches a Git 
repository. When it detects a change, it pulls the new configuration and applies it to the cluster 
itself. The CI system never needs cluster credentials — it only pushes to Git. 
Benefits of GitOps: complete audit trail (every infrastructure change has a Git commit with 
author and message), easy rollback (revert the Git commit), consistent drift detection (if 
someone manually kubectl edits something, ArgoCD detects the drift and reconciles back to 
Git), and improved security (cluster credentials never leave the cluster). 
ArgoCD vs Flux: both are CNCF projects. ArgoCD has a rich UI and is easier to get started 
with. Flux is more lightweight and Helm-native. Both support multi-cluster GitOps. 
KEY 
✦ Git = single source of truth     
✦ Pull-based (agent in cluster) vs push-based CI     
auto-reconciliation     
✦ ArgoCD / Flux are the main tools 
✦ Drift detection + 
Q3
8 
Explain the concept of immutable infrastructure. 
Intermediate 
ANS Immutable infrastructure is the practice of never modifying running servers or containers after 
they are deployed. Instead of patching or updating in-place, you build a new image with the 
changes, deploy it alongside the old one, and then destroy the old one. Servers are treated like 
cattle, not pets. 
Traditional mutable infrastructure: SSH into the server, install the patch, restart the service. 
Over time, servers accumulate configuration drift — undocumented changes, different package 
versions, hand-applied fixes. Reproducing a server from scratch becomes impossible. 
With immutable infrastructure: every change goes through a build process (Packer for VM 
images, Dockerfile for containers). The artifact (AMI, Docker image) is tested, promoted through 
environments, and deployed. The old servers are terminated. The infrastructure is always in a 
known, reproducible state. 
Benefits: eliminates configuration drift, makes rollbacks trivial (redeploy the previous image 
version), enables consistent environments (the image that passed staging tests is exactly what 
runs in production — not 'almost the same'), and greatly simplifies debugging (no one-off 
changes to investigate). 
Docker and Kubernetes are natural enablers of immutable infrastructure — containers are 
inherently immutable (you rebuild the image, you don't patch the running container). IaC tools 
like Terraform enforce infrastructure immutability at the cloud resource level. 
KEY 
✦ Never patch running servers — rebuild     ✦ Eliminates configuration drift     
natural fit     
✦ Docker + Terraform = 
✦ Rollback = redeploy previous image 
Q3
9 
What is a sidecar pattern in microservices? 
Intermediate 
ANS The sidecar pattern deploys a helper process alongside the primary application container in the 
same pod (in Kubernetes) or on the same host. The sidecar runs as a separate 
process/container, augmenting the main application with additional capabilities without 
modifying the application itself. 
For personal use only · Recruiter's DevOps Interview Series   Page 18 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
Common sidecar use cases: service mesh proxies (Envoy in Istio — intercepts all network 
traffic for mTLS, observability, and traffic management), log shippers (Fluentd/Filebeat sidecar 
collects logs from the application's stdout and ships to Elasticsearch), config reloaders (Consul 
Template refreshes config files when Consul KV changes and signals the app to reload), proxy 
sidecars (inject an authentication proxy like oauth2-proxy in front of internal services). 
Benefits: separation of concerns (application code stays clean, operational logic lives in the 
sidecar), language agnostic (the sidecar can be written in any language regardless of the main 
app's language), independent deployment and updates of the sidecar (update the Envoy 
version without touching the application). 
Drawbacks: increased resource consumption (every pod has an extra container), higher 
complexity in pod networking, potential latency added by proxy sidecars (though typically < 1ms 
for Envoy), and debugging requires understanding interactions between two containers sharing 
a network namespace. 
KEY 
✦ Helper container in same pod     
✦ Service mesh proxy = most common use case     
agnostic augmentation     
✦ Language
✦ Shared network namespace with main container 
Q4
0 
What is feature flagging and how does it support safe deployments? 
Intermediate 
ANS Feature flags (also called feature toggles or feature switches) are conditional code paths 
controlled by a configuration system rather than a code deployment. A flag can be toggled on or 
off at runtime, without a code deploy — enabling you to separate code deployment from feature 
release. 
How they enable safe deployments: deploy code to 100% of servers with the flag off (dark 
launch). Gradually enable the flag for 1% of users → 10% → 50% → 100%, monitoring error 
rates and business metrics at each step. If problems arise, disable the flag instantly — rollback 
in milliseconds, not minutes. 
Use cases: canary releases (enable for a specific % of users), A/B testing (different 
experiences for different groups), kill switches (instantly disable a feature causing production 
issues), ops flags (toggle logging verbosity, disable non-critical features under load), permission 
flags (enable features for specific user roles or accounts). 
Tools: LaunchDarkly (enterprise, SDKs for every language), Unleash (open-source, self
hosted), Flipt (open-source, cloud-native), AWS AppConfig (AWS-native flags with gradual 
rollout). Feature flags should be short-lived — long-lived flags accumulate technical debt and 
make code hard to reason about. Flag completion: remove the flag and old code path after full 
rollout. 
The risk: too many flags create combinatorial complexity. Instrument flags to track their age and 
create automated reminders to clean up old flags. 
KEY 
✦ Decouple deployment from feature release     
✦ Instant rollback = flag off     
metric monitoring     
✦ Short-lived: remove after full rollout 
✦ Gradual rollout with 
Q4
1 
What is eBPF and how is it changing observability and security in 
Linux? 
Advanced 
ANS eBPF (extended Berkeley Packet Filter) is a revolutionary Linux kernel technology that allows 
running sandboxed programs inside the Linux kernel without modifying kernel source code or 
For personal use only · Recruiter's DevOps Interview Series   Page 19 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
loading kernel modules. It is now the foundation of a new generation of observability, 
networking, and security tools. 
Before eBPF: to observe kernel events (syscalls, network packets, file access), you needed 
kernel modules (risky, complex, version-specific) or user-space sampling tools (high overhead, 
incomplete data). eBPF programs are verified by the kernel's verifier (ensuring safety — no 
infinite loops, no kernel crashes) and then JIT-compiled to native machine code for near-zero 
overhead. 
In observability: Pixie and Parca use eBPF for continuous profiling — capturing CPU flame 
graphs for every process with ~1% overhead. Cilium uses eBPF for deep visibility into network 
traffic between Kubernetes pods without sidecar proxies — eliminating the overhead of service 
meshes like Istio. 
In security: Falco uses eBPF to monitor every syscall in real time and alert on suspicious 
behaviour (container spawning a shell, reading /etc/shadow, unexpected network connection). 
Tetragon provides runtime security enforcement — it can kill a process making a disallowed 
syscall before it completes. 
eBPF is enabling a new architecture: observe once at the kernel level for all containers on the 
node, rather than injecting sidecars into every pod. This reduces overhead from O(pods) to 
O(nodes). 
KEY 
✦ Kernel-level programs, safely sandboxed     ✦ Zero-overhead observability (vs sidecar)     ✦ Cilium = 
eBPF-based Kubernetes networking     
✦ Falco / Tetragon = runtime security 
Q4
2 
What is SRE (Site Reliability Engineering) and how does it differ from 
DevOps? 
Intermediate 
ANS SRE is a discipline created at Google that applies software engineering principles to operations 
problems. It is Google's concrete implementation of DevOps. Where DevOps is a culture and 
philosophy, SRE is a specific set of practices, roles, and metrics. 
Core SRE concepts: SLI (Service Level Indicator) — a quantitative measure of service 
behaviour (e.g., request latency, availability). SLA (Service Level Agreement) — the contractual 
commitment to customers (e.g., 99.9% uptime = 8.7 hours downtime/year). SLO (Service Level 
Objective) — an internal target, stricter than the SLA (e.g., 99.95% internally, buffer before 
breaching 99.9% SLA). 
The error budget is the key concept: if your SLO is 99.9% availability, your error budget is 0.1% 
— about 43.8 minutes of downtime per month. As long as you're within budget, you can take 
risks (new features, experiments). When the error budget is exhausted, all new deployments 
freeze until it recovers. This creates a data-driven negotiation between development (wants to 
ship features) and operations (wants stability). 
Toil reduction: SREs are engineers who automate themselves out of manual operational work. 
If a task is repetitive, manual, and could be automated, it's toil. SREs are expected to spend no 
more than 50% of their time on toil. The rest is engineering work that reduces future toil. 
SRE vs DevOps: DevOps is a culture. SRE is a job role and practice set. SRE teams are 
typically embedded with product teams to improve reliability using engineering approaches. 
KEY 
✦ SLI / SLO / SLA / Error budget     
✦ Error budget = balance speed vs stability     
through automation     
✦ SRE = software eng applied to ops 
✦ Toil reduction 
Q4
3 
Explain the 12-Factor App methodology and its relevance to DevOps. 
Intermediate 
For personal use only · Recruiter's DevOps Interview Series   Page 20 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
ANS The 12-Factor App is a methodology for building modern, scalable, maintainable software-as-a
service applications. Created by Heroku engineers, it is highly relevant to DevOps because it 
defines the application contract that makes CI/CD, containerisation, and cloud deployment work 
smoothly. 
The most DevOps-critical factors: Factor 3 — Config: store config in environment variables, 
never in code. This enables the same image to run across dev/staging/prod with different 
config. Factor 6 — Processes: execute the app as stateless, share-nothing processes. State 
lives in external services (database, cache). This enables horizontal scaling and immutable 
deployments. Factor 7 — Port binding: the app is self-contained and exports services by 
binding to a port — no dependency on a webserver injected by the runtime. Factor 9 — 
Disposability: fast startup (seconds) and graceful shutdown (handles SIGTERM). Critical for 
Kubernetes pod scheduling and rolling deployments. Factor 11 — Logs: treat logs as event 
streams — write to stdout, not files. The execution environment (Docker, K8s) captures and 
routes them. 
Other key factors: Codebase (one repo, multiple deploys), Dependencies (explicitly declare and 
isolate), Backing services (treat databases/queues as attached resources), Build/release/run 
(strict separation of stages), Dev/prod parity (keep environments as similar as possible), Admin 
processes (run admin tasks as one-off processes). 
A 12-factor app is natively cloud-native — it runs cleanly in containers, scales horizontally, 
deploys with zero-downtime, and integrates naturally with CI/CD pipelines. 
KEY 
✦ Config in env vars (not code)     
✦ Stateless processes = horizontal scaling     
files)     
✦ Disposability = fast startup + graceful shutdown 
✦ Logs to stdout (not 
Q4
4 
What is a circuit breaker pattern and how does it improve resilience? 
Advanced 
ANS The circuit breaker pattern is a resilience pattern for distributed systems that prevents a failing 
downstream service from causing cascading failures throughout the system. It is inspired by 
electrical circuit breakers — when too much current flows, the breaker trips to prevent damage. 
How it works: the circuit breaker wraps calls to a downstream service and monitors for failures. 
It has three states: Closed (normal operation — all requests pass through, failures are 
counted), Open (too many failures — the breaker 'trips', all requests immediately return an error 
or fallback without calling the downstream service, allowing it to recover), Half-Open (after a 
timeout, a small number of test requests are allowed through — if they succeed, the breaker 
closes; if they fail, it opens again). 
Without a circuit breaker: Service A calls Service B (which is down). Each call hangs for 30 
seconds (timeout). Service A's thread pool fills with waiting threads. Service A runs out of 
threads and becomes unresponsive. Service C calls Service A — now both are down. 
Cascading failure brings down the entire system. 
With a circuit breaker: after 5 failures, the breaker opens. Service A immediately returns a 
fallback response (cached data, error message, default value) without waiting. Service A 
remains healthy. The failure is contained. 
Implementation: Resilience4j (Java), Hystrix (Netflix, deprecated → Resilience4j), Polly (.NET), 
Istio/Envoy (service mesh handles circuit breaking transparently). Pair with timeout 
configuration, bulkhead pattern (isolate thread pools per dependency), and retry with 
exponential backoff and jitter. 
KEY 
✦ Closed → Open → Half-Open states     
✦ Prevents cascading failures     
open     
✦ Resilience4j (Java), Envoy (service mesh) 
✦ Immediate fallback when 
For personal use only · Recruiter's DevOps Interview Series   Page 21 
DevOps Interview Mastery Guide   |   45 Questions with Detailed Answers 
Q4
5 
How would you design a zero-downtime deployment strategy for a 
critical production service? 
Advanced 
ANS Zero-downtime deployment requires combining the right deployment strategy, database 
migration approach, observability, and rollback capability. There is no single tool — it is an 
architectural and process discipline. 
Deployment strategy: use rolling deployments (Kubernetes default) for simple cases — new 
pods come up before old ones go down. Use canary for high-risk changes — 5% traffic to new 
version, monitor for 30 minutes, then progress. Use blue-green for instant cutover capability on 
stateless services. 
Database migrations are the hardest part. Expand-Contract (also called backward-compatible 
migrations): Expand phase — add new columns/tables while keeping old ones (both old and 
new code work). Deploy new code. Contract phase — remove old columns/tables in a separate 
deployment after verifying no traffic hits them. Never drop a column in the same deployment 
that removes the code using it. 
Feature flags: deploy new code behind a flag. The deployment itself is zero-risk. Enable the flag 
gradually. This completely decouples the deployment from the feature release, allowing instant 
rollback without a redeployment. 
Pre-deployment checklist: health checks configured and fast (liveness + readiness probes in 
Kubernetes), PodDisruptionBudgets set (e.g., minAvailable=1 ensures K8s never takes down 
the last replica during node drain), connection draining enabled on load balancer (in-flight 
requests complete before traffic stops), runbook and rollback procedure documented and 
practised. 
Observability: monitor error rate, p99 latency, and business metrics (conversions, orders) 
during and after deployment. Have a pre-agreed rollback threshold (e.g., if error rate > 1%, 
auto-rollback via ArgoCD or run helm rollback immediately). Deployment isn't done until 15 
minutes of stable metrics post-release. 
KEY 
✦ Rolling / canary / blue-green based on risk     
✦ Expand-Contract for DB migrations     
decouple deploy from release     
✦ Feature flags 
✦ PodDisruptionBudgets + connection draining 
For personal use only · Recruiter's DevOps Interview Series   Page 22 
