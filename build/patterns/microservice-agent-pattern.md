---
title: "Microservice Agent Pattern"
description: "A pattern for building distributed AI agents that operate across microservices"
date: "2025-04-15"
type: "patterns"
skillLevel: "Intermediate"
frameworks: ["Strands", "LangGraph"]
services: ["Amazon Bedrock", "AWS Lambda", "Amazon EKS"]
url: "https://aws.amazon.com/architecture/ai-ml-patterns/"
image: "/images/aws-logo.svg"
---

# Microservice Agent Pattern

This pattern demonstrates how to design AI agents that operate across distributed microservices, allowing for separation of concerns, scalability, and fault tolerance.

## Key Components

- Agent Orchestrator
- Service-specific intelligent agents
- Shared memory layer
- Communication protocols between agents

## Implementation Guidelines

1. Define clear boundaries and responsibilities for each agent
2. Establish communication protocols between agents
3. Design a shared memory architecture
4. Implement fault tolerance and recovery mechanisms
