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
- Horizontal Scaling: Multiple instances may operate in a cluster-type configuration
- Easily operated within a container

### Mastodon Streamer
- NodeJS-based service
- Provides notifications to mobile devices and connected web-clients
- Connects via HTTP long pull or WebSockets
- Horizontal Scaling: Multiple instances may operate in a cluster-type configuration (requires session pinning)
- Easily operated within a container

### Queue Processor
- [Sidekiq](https://github.com/mperham/sidekiq): Ruby-based service
- Queues write-type operations, including
  - Writes to PostgreSQL
  - Transaction emails
  - Other write operations
- Stateless, pulls from Redis queue
- Horizontal Scaling: Multiple instances may operate in a cluster-type configuration
- Easily operated within a container

### PostgreSQL Database
- The "permanent record" of a Mastodon service's state
- No special configuration needed other than pgTune
- Primarily vertical scaling; horizontal scaling through use of read replicas in a hot standby confiburaiton

### Redis Instance
- Maintains the queue of Sidekiq jobs
- Also feeds and volatile cache
- Horizontally scalable through a Redis cluster configuration
- Memory-only

### Object Storage (e.g., S3)
- Cached copies of posts from other servers
- Posts from own server
- Stores all media
- Tendency to get a bit large, as any servers' interaction with other servers will result in a data copy/transfer

### ElasticSearch (optional)
- For text-based searches of all cached post data
- Expensive to operate
- Basic text search used in lieu of ElasticSearch


### Production-ish operations should probably have the following

### Nginx Reverse Proxy
- Sits on top of the HTTPS endpoint
- Relays requests to:
  - Static compiled React pages
  - REST API
  - Streamer/WebSockets
- May be horizonta

### HAProxy Forward Proxy
- (Final) HTTPs endpoint
- Load balancer to Nginx
- WAF implementation

## Explainers

- https://softwaremill.com/the-architecture-of-mastodon/
