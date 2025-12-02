# Overview of Deployment Architecture


To successfully deploy Cribl, you need to understand the architectural decisions that determine its cost, performance, and compliance profile. Specifically, we cover relevant topics including:

- [Choosing an Architecture](arch-deployment-framework): A comparison of the three available models, detailing the trade-offs in operational overhead, data sovereignty, and cost structure.
- [Worker Group and Fleet Placement](worker-group-placement): The guiding principle for high performance: placing Worker Groups/Fleets close to the data source to minimize latency and egress costs.
- [High Availability (HA) Leader](arch-deploy-ha): Requirements for building a resilient system, including using N+1 redundancy for the Data Plane and ensuring Leader HA with shared state for the Control Plane.
- [Cribl.Cloud Architecture Overview](arch-deploy-ha): Understanding Cribl.Cloud deployment model, its domains, and high-level components. 
- [On-Prem Architecture Planning](arch-onprem-planning): Specific requirements for containerized deployments (Kubernetes), proper Worker sizing, and manual responsibilities for security and upgrades.
- [Hybrid Deployment Architecture Planning](arch-hybrid-planning): How to use a Cribl.Cloud Leader to centrally manage customer-managed Workers to meet data residency and compliance needs while leveraging SaaS control.
- [Securing On-Prem and Hybrid Deployments](arch-onprem-security): Mandates for all environments, including using TLS encryption, implementing Role-Based Access Control (RBAC), and securely managing all configurations and secrets.