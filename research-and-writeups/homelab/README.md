# Cybersecurity Home Lab

**Author:** Dennis Kalinin  
**Project Type:** Self-Hosted Infrastructure / Home Lab  
**Platform:** UGREEN NASync DXP8800 Plus  
**Storage:** 8 × 10 TB HDDs — 80 TB raw capacity  
**Environment:** ZimaOS, TrueNAS, Docker  
**Core Services:** Pi-hole, Plex/Jellyfin, Immich, Tailscale  
**Status:** Active / Continuing Development  

---

## Project Overview

This project documents the design and operation of my personal cybersecurity and infrastructure home lab.

The environment is built around a **UGREEN NASync DXP8800 Plus** populated with eight 10 TB hard drives, providing approximately **80 TB of raw storage capacity**.

Rather than using the system only as network-attached storage, I developed it into a multi-purpose self-hosted environment for storage management, networking, containerized applications, remote access, family services, and cybersecurity experimentation.

I used a public YouTube build guide as one of the references during the original setup, then adapted the final environment around my own hardware, services, storage requirements, and security goals.

---

## Architecture

```text
UGREEN NASync DXP8800 Plus
│
├── 8 × 10 TB HDDs
│   └── 80 TB Raw Storage
│
├── ZimaOS
│   └── Linux-Based Host Environment
│
├── TrueNAS
│   └── Storage Configuration & Management
│
└── Docker
    │
    ├── Pi-hole
    │   └── Network-Wide DNS Filtering
    │
    ├── Plex / Jellyfin
    │   └── Self-Hosted Media
    │
    ├── Immich
    │   └── Family Photo & Video Storage
    │
    └── Tailscale
        └── Secure Remote Access
```

The environment is designed around three main layers:

1. **Physical infrastructure and storage**
2. **Host and storage management**
3. **Containerized application services**

---

## Technology Stack

| Technology | Role |
|---|---|
| **UGREEN NASync DXP8800 Plus** | Physical NAS platform |
| **8 × 10 TB HDDs** | 80 TB raw storage capacity |
| **ZimaOS** | Linux-based operating environment |
| **TrueNAS** | Storage configuration and management |
| **Docker** | Application separation and deployment |
| **Pi-hole** | Network-wide DNS filtering |
| **Plex / Jellyfin** | Self-hosted media services |
| **Immich** | Private family photo and video storage |
| **Tailscale** | Secure remote connectivity |

> **Storage Note:** 80 TB represents raw installed capacity. Actual usable capacity depends on the storage and redundancy configuration.

---

## Containerized Services

### Pi-hole

Pi-hole provides centralized DNS-based filtering for devices on the home network.

Instead of installing an ad-blocking solution separately on every supported device, DNS requests can be filtered centrally.

**Skills applied:**

- DNS
- Network services
- Docker networking
- Centralized filtering
- Troubleshooting name resolution

---

### Plex / Jellyfin

Plex or Jellyfin provides centralized access to media stored on the NAS.

Running the media service through Docker also gave me experience connecting applications to persistent storage while maintaining separation between the service and host environment.

**Skills applied:**

- Docker
- Persistent storage
- File permissions
- Network streaming
- Application configuration

---

### Immich

Immich provides private, self-hosted photo and video storage for my family.

A primary use case is allowing my parents to move photographs and videos from their phones to the NAS when mobile-device storage becomes limited.

This also introduced real data-protection considerations because family photographs are important and potentially irreplaceable.

**Security considerations include:**

- User authentication
- Storage permissions
- Application updates
- Backup planning
- Recovery testing
- Limiting unnecessary access to photo storage

---

### Tailscale

Tailscale provides secure remote access to approved home-lab resources while away from the local network.

This allows me to access the environment without unnecessarily exposing administrative interfaces or multiple internal services directly to the public internet.

**Skills applied:**

- Secure remote access
- Private networking
- Device authentication
- Remote administration
- Network troubleshooting

---

## Security Design

Security influenced several architectural decisions throughout the project.

### Application Separation

Additional services are deployed using Docker rather than installing every application directly into the host operating environment.

This makes services easier to manage independently and reduces configuration overlap between applications.

### Reduced Public Exposure

Tailscale is used as the primary remote-access method so internal administrative services do not need to be broadly exposed through public-facing port forwarding.

### Least-Privilege Approach

Applications should receive access only to the storage and resources necessary for their function.

For example:

- Media services require access to media storage.
- Immich requires access to photo and application storage.
- Pi-hole does not require access to personal file storage.

### Centralized DNS Filtering

Pi-hole provides a centralized control point for DNS filtering across supported devices.

### Controlled Administration

Administrative interfaces are intended to remain accessible only through trusted local networks or authenticated remote-access paths.

---

## Why Docker?

Docker was an important part of the design because I wanted applications to remain separated rather than placing every dependency directly onto one operating system.

Benefits include:

- Independent application deployment
- Easier upgrades
- Easier troubleshooting
- Separated dependencies
- Cleaner removal or replacement of services
- Organized persistent storage
- Reduced modification of the underlying host

Containers are not equivalent to full virtual machines from a security-isolation perspective, but they provide useful application boundaries and simplify administration.

---

## Skills Demonstrated

This project has provided hands-on experience across several technical areas.

### Infrastructure

- Network-attached storage
- Large-capacity storage administration
- Linux administration
- Storage planning
- Self-hosted infrastructure

### Networking

- DNS
- Secure remote access
- Client/server communication
- Network troubleshooting
- Private network services

### Containers

- Docker deployment
- Container networking
- Persistent storage
- Application separation
- Service lifecycle management

### Cybersecurity

- Attack-surface reduction
- Access control
- Service isolation
- Secure remote connectivity
- Least-privilege thinking
- Security hardening
- Data-protection planning

---

## Lessons Learned

One of the biggest lessons from building the environment has been understanding how closely infrastructure components depend on each other.

Storage, networking, permissions, Docker, applications, authentication, and remote access cannot be treated as completely independent technologies.

A storage-permission problem can prevent an application from functioning. A DNS configuration change can affect the entire network. A remote-access decision can directly change the system's attack surface.

Self-hosting also creates responsibility.

Using my own infrastructure gives me more control over data and services, but it also makes me responsible for:

- Updates
- Availability
- Storage health
- User access
- Security
- Backups
- Troubleshooting
- Recovery

---

## Future Improvements

The environment continues to evolve.

Planned areas for improvement include:

- More formal network segmentation
- Centralized security logging
- SIEM integration
- Backup and restore testing
- Container permission reviews
- Vulnerability management
- Additional IDS/IPS experimentation
- Infrastructure monitoring
- Documented disaster-recovery procedures
- Additional architecture and network diagrams

---

## Build Reference

The following video was used as one of the references while planning and building the original environment:

[UGREEN NAS / Home Lab Build Reference](https://www.youtube.com/watch?v=0TctEEh7Xuw)

The final implementation was adapted to my own hardware, services, users, storage requirements, and security objectives.

---

## Project Takeaway

What began as a high-capacity storage project developed into a practical self-hosted infrastructure environment supporting real users and services.

The project has given me experience working across **Linux, storage, networking, Docker, DNS, remote access, application management, and cybersecurity** in a system that I continue to maintain and improve.

---

[← Return to Research & Writeups](../)
