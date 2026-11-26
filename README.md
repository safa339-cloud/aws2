# AWS Application Load Balancer – Path-Based Routing

**Name:** Safa
**Region:** Asia Pacific (Mumbai) – ap-south-1
**Date:** 19 October 2026

## Project Overview

This project uses an **Application Load Balancer (ALB)** to send traffic to three different EC2 servers, based on the URL path.

| URL Path | Target Group | EC2 Server | Page Shown |
|----------|--------------|------------|------------|
| `/gmail*` | TG-Gmail | server-gmail | Gmail App - Safa |
| `/drive*` | TG-Drive | server-driver | Drive App - Safa |
| `/home*` | TG-Home | server-home | Home App - Safa |
| Anything else (default) | TG-Home | server-home | Home App - Safa |

## Architecture

```
User (Browser)
      |
      v
ALB-Safa (Internet-facing, HTTP:80)
      |
      +-- /gmail*  --> TG-Gmail --> server-gmail
      +-- /drive*  --> TG-Drive --> server-driver
      +-- /home*   --> TG-Home  --> server-home
      +-- default  --> TG-Home  --> server-home
```

## Resources Created

### 1. EC2 Instances
- `server-gmail` – t3.micro, ap-south-1a
- `server-home` – t3.micro, ap-south-1b
- `server-driver` – t3.micro, ap-south-1b
- All 3 instances: Running, 3/3 status checks passed

### 2. Target Groups
- `TG-Gmail`, `TG-Drive`, `TG-Home`
- Protocol: HTTP, Port: 80, Target type: Instance
- All attached to ALB-Safa
- TG-Home health: 1 Healthy, 0 Unhealthy

### 3. Load Balancer
- Name: `ALB-Safa`
- Type: Application, Internet-facing, IPv4
- State: Active
- Availability Zones: ap-south-1a and ap-south-1b (2 AZs)

### 4. Listener Rules (HTTP:80)
| Priority | Condition | Action |
|----------|-----------|--------|
| 1 | Path = `/gmail*` | Forward to TG-Gmail |
| 2 | Path = `/drive*` | Forward to TG-Drive |
| 3 | Path = `/home*` | Forward to TG-Home |
| Last (default) | No other rule matches | Forward to TG-Home |

## How to Test

Open these links in a browser (replace with your ALB DNS name):

- `http://<ALB-DNS-name>/gmail/index.html` → **Gmail App - Safa**
- `http://<ALB-DNS-name>/drive/index.html` → **Drive App - Safa**
- `http://<ALB-DNS-name>/home/index.html` → **Home App - Safa**

## Steps Followed

1. Launched 3 EC2 instances and checked they are running.
2. Created 3 target groups and registered one instance in each.
3. Created the Application Load Balancer in 2 Availability Zones.
4. Added listener rules for path-based routing (`/gmail`, `/drive`, `/home`).
5. Tested all 3 paths through the ALB DNS name.

## Result

All three paths opened the correct app through the single ALB DNS name. Path-based routing works.

## Screenshots

See `ALB_Assignment_Safa.pdf` for screenshots of every step.
