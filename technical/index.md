---
layout: docs.njk
title: Technical Overview
description: Deep dive into Hay's architecture and development philosophy
section: technical
---

## Technical Documentation

Welcome to Hay's technical documentation. This section is designed for developers, system administrators, and technical users who want to understand how Hay works under the hood.

### What You'll Find Here

This documentation covers:

- **Architecture**: How Hay is structured and why
- **Plugin Development**: Build custom extensions for Hay
- **API Reference**: Comprehensive API documentation
- **Contributing**: How to contribute to Hay's development

### Prerequisites

To make the most of this documentation, you should be familiar with:

- JavaScript/TypeScript
- RESTful APIs
- Async programming concepts
- Basic DevOps principles

### Hay's Technology Stack

Hay is built with modern, production-tested technologies:

```
Frontend:
- React 18+ with TypeScript
- TailwindCSS for styling
- Zustand for state management

Backend:
- Node.js with Express
- PostgreSQL for data persistence
- Redis for caching and real-time features
- Bull for job queues

Infrastructure:
- Docker for containerization
- Kubernetes for orchestration
- GitHub Actions for CI/CD
```

### Core Concepts

#### Event-Driven Architecture

Hay uses an event-driven architecture to enable:
- Real-time updates across clients
- Loose coupling between components
- Easy extension through plugins

#### Plugin System

Everything in Hay is a plugin, including core features. This allows for:
- Maximum flexibility
- Easy customization
- Clean separation of concerns

#### Type Safety

Hay is written in TypeScript throughout, providing:
- Better developer experience
- Fewer runtime errors
- Self-documenting code

### Getting Started

Choose your path:

- **[Architecture](/docs/technical/architecture/)** - Understand Hay's design
- **[Philosophy](/docs/technical/philosophy/)** - Learn our design principles
- **[Plugin Development](/docs/technical/plugins/getting-started/)** - Build your first plugin
- **[API Reference](/docs/technical/api/authentication/)** - Integrate with Hay's API

### Community

Join our developer community:

- **GitHub**: [github.com/hay-chat/hay](https://github.com)
- **Discord**: Developer channel for technical discussions
- **Contributing**: See our [contribution guide](/docs/technical/contributing/setup/)

### Support

Need help with something technical?

- Open an issue on [GitHub](https://github.com)
- Join our Discord server
- Email us at [{{ site.email }}](mailto:{{ site.email }})
