# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Jian Cha Tea Unity Suite** - AI-native franchise management platform supporting 500+ locations globally. Implementation begins Q1 2026 to leverage mature AI development tools for 55% productivity gains.

## Repository Structure

```
jianchatea-platfrom/
├── apps/
│   └── roadmap-visual/    # Next.js roadmap visualization (port 4500)
├── docs/                  # Architecture and technical documentation
├── requirements/          # Business requirements by module
└── workflows/            # Role-based workflow documentation
```

## Development Commands

### Roadmap Visualization App (`apps/roadmap-visual/`)

```bash
# Development (port 4500 with Turbopack)
npm run dev
yarn dev

# Build with Turbopack
npm run build
yarn build

# Production server
npm run start
yarn start

# Linting
npm run lint
yarn lint
```

## Technology Stack (2026 Implementation)

### Core Languages
- **Java 21 (Spring Boot 4)** - Business logic services
- **Python 3.13 (FastAPI 2.0)** - ML/AI services
- **Node.js 22 (NestJS 11)** - Real-time services, BFF
- **TypeScript 6.0** - Frontend development
- **Rust** - High-performance services (payments, auth)

### Frontend
- **Next.js 15+** with React 19 Server Components
- **React Native 0.75+** - Mobile apps
- **Tailwind CSS** - Styling
- **Radix UI** - Component primitives

### Infrastructure
- **AWS** - Primary cloud with edge computing
- **Kubernetes 1.30+** - Container orchestration
- **PostgreSQL 17** with pgvector - Primary database
- **Redis 8** - Caching and real-time
- **Apache Kafka 4.0** - Event streaming

### AI/ML Tools
- **GitHub Copilot X** - AI pair programming
- **Cursor IDE** - AI-native development
- **Vector databases** - Pinecone/Weaviate for embeddings
- **LLM APIs** - OpenAI/Claude integration

## Implementation Roadmap

### Phase 1: Q1-Q2 2026 - Foundation
- Cloud infrastructure setup
- AI-enhanced IAM system
- Franchise portal
- Intelligent POS

### Phase 2: Q3-Q4 2026 - Customer Experience
- AI mobile app
- Digital wallet
- CRM/CDP platform
- Loyalty program

### Phase 3: Q1-Q2 2027 - Operations
- HR management
- Employee assistant
- Advanced POS features
- Predictive inventory

### Phase 4: Q3-Q4 2027 - Global Scale
- AutoML analytics
- Real-time predictions
- Global expansion
- Quantum-resistant security

### Phase 5: 2028+ - Future Tech
- Metaverse integration
- Robotics automation
- 6G optimization
- Sustainable AI

## Key Performance Targets

- **API Response**: <100ms (95th percentile)
- **Availability**: 99.99% uptime
- **Concurrent Users**: 500,000+ global
- **AI Inference**: <50ms
- **Transaction Volume**: 5M+ daily

## Architecture Patterns

- **Microservices** with domain-driven design
- **Event-driven** via Kafka
- **Database per service** isolation
- **CQRS** for read/write separation
- **Circuit breaker** for resilience
- **Edge computing** for low latency

## Security Requirements

- **Zero-trust** architecture
- **OAuth 2.0/OIDC** authentication
- **PCI DSS Level 1** compliance
- **GDPR/PDPA** data privacy
- **Post-quantum** cryptography ready
- **End-to-end** encryption (TLS 1.3)

## Development Guidelines

1. **AI-assisted coding** - Use GitHub Copilot X for all tasks
2. **API standards** - OpenAPI 3.1 specifications
3. **Testing** - >90% coverage with AI-generated tests
4. **Code review** - AI-assisted + human approval
5. **Monitoring** - Full observability with ML anomaly detection
6. **Accessibility** - WCAG 3.0 Level AA compliance

## Current Status

- **Planning Phase** - Architecture and requirements documented
- **Development Start** - Q1 2026
- **First Deployment** - Q2 2026 (10 pilot stores)
- **Full Rollout** - Q4 2027 (500+ stores)

## Important Notes

- This is a **greenfield project** - no legacy code constraints
- **Multi-language support** - 100+ languages via AI translation
- **Expected ROI** - 380% over 3 years
- **Cost savings** - 40% lower infrastructure costs with serverless
- **Development speed** - 55% faster with AI tools