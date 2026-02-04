 InsightStream-Distributed-Cloud-Analytics-SaaS
InsightStream is a multi-tenant analytics SaaS platform that allows organizations to upload structured datasets (CSV/JSON), process them asynchronously, and visualize aggregated insights through a scalable cloud-based architecture.

The project was built to simulate an enterprise-grade analytics system similar to large-scale BI platforms. It focuses on distributed system design, microservices architecture, asynchronous data processing, and cloud-native deployment.

Problem Statement

Traditional analytics systems often struggle with scaling data ingestion and processing workloads. InsightStream addresses this by separating ingestion, processing, and analytics services into independent components that can scale horizontally.

The system is designed to:
Handle concurrent dataset uploads
Process large files asynchronously
Isolate tenant data securely

Scale compute services independently

Remain resilient to partial service failures

System Architecture

The application follows a microservices-based architecture with loosely coupled services.

Core Services

API Gateway
Acts as the entry point for all client requests and routes them to appropriate services.

Authentication Service
Handles user registration and login using JWT-based authentication. Implements role-based access control and ensures tenant-level isolation.

Data Ingestion Service
Accepts dataset uploads and stores them in cloud object storage . Once uploaded, it publishes a processing task to the message queue.

Processing Service
Consumes messages from AWS SQS and performs data aggregation asynchronously. The service is stateless and horizontally scalable.

Analytics Service
Serves aggregated insights via REST APIs. Optimized for read-heavy workloads.

Frontend Dashboard
Displays metrics such as revenue trends, growth rates, and grouped analytics.

Distributed System Design Decisions

The system is intentionally designed using distributed architecture principles:

Services are independently deployable

Communication between services is asynchronous (via SQS)

Processing workloads are decoupled from user-facing APIs

Services are stateless to allow horizontal scaling

Failure in processing service does not affect authentication or ingestion

Cloud Infrastructure

Compute
Docker containers orchestrated using ECS 

Database
Amazon RDS for storing structured aggregated data.

Object Storage
Amazon S3 for storing uploaded datasets.

Message Queue
Amazon SQS for asynchronous job handling.

Monitoring
CloudWatch logs for observability.

CD
GitHub Actions pipeline for automated build and deployment.

Data Flow

User uploads dataset.

File is stored in S3.

A message is pushed to SQS.

Processing service consumes the message.

Data is aggregated and stored in PostgreSQL.

Analytics service serves processed metrics to dashboard.

Scalability Approach

Stateless containers allow horizontal scaling.

Processing services can scale independently based on queue depth.

Message queue buffers traffic spikes.

Read-optimized queries reduce load on the database.

Security Design

JWT-based authentication

Role-based access control

Tenant-specific data filtering

Environment variable-based secret management

IAM policies for controlled cloud access

Tech Stack

Backend: FastAPI
Database: PostgreSQL
Containerization: Docker
Cloud Platform: AWS 
CI/CD: GitHub Actions
Frontend: React

Future Improvements

Real-time streaming using Kafka

Redis-based caching layer

Auto-scaling policies based on metrics

Multi-region deployment

Distributed tracing
