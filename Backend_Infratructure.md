# Resources and Information on Backend Infrastrucrure

## General Infrastructure

The core Mastodon service operates via a mesh of micro-services:

### Mastodon Client
- React-based Web App
- Static pages designed to interface with the Mastodon API
- Pages are transpiled before runtime

### Mastodon API
- [Ruby on Rails](https://rubyonrails.org)-based service
- REST API Interface
- HTTPS endpoint runs [Puma](https://puma.io)
- Stateless
- GETs messages via the ActivityPub spec to the Fediverse
- Messages vis the ActivityPub spec are POSTed to the API from the Fediverse
- Multiple instances may operate in a cluster-type configuration
- Easily operated within a container

### Mastodon Streamer
- NodeJS-based service
- Provides notifications to mobile devices and connected web-clients
- Connects via HTTP long pull or WebSockets
- Multiple instances may operate in a cluster-type configuration (requires session pinning)
- Easily operated within a container

### Queue Processor
- Ruby-based service
- Queues write-type operations, including
  - Writes to PostgreSQL
  - Transaction emails
  - Other write operations
- Stateless, pulls from Redis queue
- Multiple instances may operate in a cluster-type configuration
- Easily operated within a container



### Production-ish operations should probably also have the following


## Explainers

- https://softwaremill.com/the-architecture-of-mastodon/
