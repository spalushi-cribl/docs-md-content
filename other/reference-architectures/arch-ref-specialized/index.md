# Specialized Reference Architectures


Specialized Reference Architectures are targeted, high-value blueprints designed to help teams quickly and confidently implement solutions for specific, complex deployment scenarios.

These architectures complement the full-suite architecture and core planning guidance—they don't replace them—by drilling into best practices for common, demanding use cases.

## Key Specialized Architectures
- [Full-suite reference architecture](reference-arch-full-suite): Outlines the comprehensive topology and connections for the full suite of Cribl products. 
- [Syslog to Cribl Stream](reference-arch-syslog): Designs reliable, scalable Syslog ingestion. Provides best practices for transport choice (TCP/UDP), load balancing, and production sizing.
- [Unified ingest & replay data](deploy-reference-data): Minimizes egress costs by centralizing routing. Supports data replay from low-cost object storage or Cribl Lake.
- [Distributed Agents](deploy-reference-many-agents): Scales cleanly to a large number of agents. Addresses socket scaling, uneven EPS, and safe growth practices.
- [Multi-Destination enterprise](deploy-reference-destinations): Optimizes routing to many downstream platforms (SIEM, data lakes). Uses streaming versus non-streaming destination models and meta-routing.
- [Hybrid cloud data](deploy-hybrid-data): Balances centralized, cloud-managed control with local processing. Keeps the data plane (Workers) near sources for compliance and cost control.

