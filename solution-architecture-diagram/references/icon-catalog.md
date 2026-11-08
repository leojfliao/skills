# Hand-Drawn Icon Catalog

The icon bodies live in `assets/icon-library.svg`, 47 in total, all hand-drawn geometric shapes, unified on a 24×24 grid, stroke width 1.5, `fill="none"`, with strokes inheriting `currentColor`. Open the file directly in a browser to see the full named preview grid.

## Usage Rules

1. Pick icons from the meaning table below; do not choose casually by shape similarity.
2. Copy the entire corresponding `<symbol>` block into the output diagram's `<defs>`, copying only those used.
3. Reference within the same file: `<use href="#icon-database" x="..." y="..." width="24" height="24"/>`.
4. Do not write a cross-file `href` (local opening will be blocked by the browser), do not individually change a single icon's color, stroke width, or fill; only the focal component may switch its entire group of strokes to red.
5. Icon size is uniformly 24×24, aligned to the text baseline, with no scaling up or down.
6. When no icon matches, use the closest general icon + a text label to explain; do not draw a new icon with an inconsistent style on the fly.

## Usage Example

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 720" role="img" aria-label="Sample layered architecture">
  <defs>
    <symbol id="icon-database" viewBox="0 0 24 24">
      <ellipse cx="12" cy="5.5" rx="8" ry="3"/>
      <path d="M4 5.5v13c0 1.7 3.6 3 8 3s8-1.3 8-3v-13M4 12c0 1.7 3.6 3 8 3s8-1.3 8-3"/>
    </symbol>
  </defs>
  <g fill="none" stroke="#333333" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
    <use href="#icon-database" x="100" y="500" width="24" height="24"/>
  </g>
</svg>
```

The stroke attributes on the parent `<g>` are inherited by `<use>`; do not set `stroke` separately on the paths inside the symbol.

## Catalog

### Cloud & Regions

| id | Meaning | When to use |
|---|---|---|
| `icon-cloud` | Cloud / cloud provider (abstract) | A cloud environment not tied to a specific vendor |
| `icon-region` | Region | A geographic or logical large area |
| `icon-vpc` | Virtual private cloud | Isolated network boundary inside the cloud |
| `icon-onprem` | On-premises machine room | Self-built data center |
| `icon-edge` | Edge node | CDN edge, nearest access point |
| `icon-availability` | Availability zone / multi-instance distribution | Multi-availability-zone deployment in one region |

### Compute

| id | Meaning | When to use |
|---|---|---|
| `icon-compute` | Compute instance / virtual machine | General-purpose compute power |
| `icon-container` | Container | Containerized runtime unit |
| `icon-serverless` | Function compute | Event-triggered short task |
| `icon-kubernetes` | Orchestration platform | Container orchestration and scheduling |
| `icon-batch` | Batch job | Scheduled or offline batch processing |

### Ingress

| id | Meaning | When to use |
|---|---|---|
| `icon-gateway` | Gateway | Unified entry point, routing |
| `icon-loadbalancer` | Load balancer | Traffic distribution |
| `icon-api` | API interface | External contract layer |
| `icon-firewall` | Firewall | Network access control |
| `icon-dns` | Domain name resolution | Service discovery and resolution |

### Services

| id | Meaning | When to use |
|---|---|---|
| `icon-service` | General service | A service whose implementation form is not emphasized |
| `icon-microservice` | Microservice cluster | Multiple small services calling each other |
| `icon-agent` | Agent | Side-channel collection, delegated execution |
| `icon-scheduler` | Scheduler | Scheduled triggering and orchestration |

### Data

| id | Meaning | When to use |
|---|---|---|
| `icon-database` | General database | A store with no type distinction |
| `icon-sql` | Relational database | Structured query scenarios |
| `icon-nosql` | Non-relational database | Document / KV / graph |
| `icon-warehouse` | Data warehouse | Analytical storage |
| `icon-lake` | Data lake | Raw multimodal accumulation |
| `icon-objectstore` | Object storage | Files, archiving, backup |

### Middleware

| id | Meaning | When to use |
|---|---|---|
| `icon-cache` | Cache | Hotspot acceleration |
| `icon-queue` | Message queue | Async decoupling, peak shaving |
| `icon-topic` | Pub/sub topic | One-to-many broadcast |
| `icon-event` | Event | Event body in event-driven design |
| `icon-stream` | Stream processing | Continuous data processing pipeline |

### Security

| id | Meaning | When to use |
|---|---|---|
| `icon-iam` | Identity and access | Authentication, authorization, roles |
| `icon-key` | Key | Credential and key management |
| `icon-shield` | Security protection | General security boundary |
| `icon-lock` | Encryption / lock | Encryption in transit or at rest |

### Observability

| id | Meaning | When to use |
|---|---|---|
| `icon-monitor` | Monitoring | Metrics dashboard |
| `icon-log` | Log | Log collection and search |
| `icon-trace` | Distributed tracing | Distributed call chain |
| `icon-alert` | Alert | Threshold triggering and notification |

### Clients & External

| id | Meaning | When to use |
|---|---|---|
| `icon-user` | User | A person as a participant |
| `icon-web` | Web client | Browser client |
| `icon-mobile` | Mobile client | App client |
| `icon-partner` | Partner system | Integration with external institutions |
| `icon-external` | External dependency | Any external party not part of this system |

### General

| id | Meaning | When to use |
|---|---|---|
| `icon-network` | Network interconnect | Abstract network topology |
| `icon-boundary` | Boundary box | Trust domain, partition |
| `icon-note` | Note / to be confirmed | Placeholder box for an undecided component |

## When No Icon Matches

When the catalog has no matching meaning, handle it in this order:

1. Use `icon-service` (software component) or `icon-external` (external party) + an accurate text label.
2. For components that need to be marked "not yet confirmed", use `icon-note`, place it in a "to be confirmed" dashed box, and list it in the legend.
3. When it still cannot be explained clearly, prefer drawing a plain text-only box over drawing a new icon whose stroke width is inconsistent with the rest.

## Prohibited

- Do not bring in a cloud vendor's official icon pack (bundled distribution raises licensing issues); use an abstract icon + a text label instead.
- Do not convert icons into a raster image, do not embed `<image>`.
- Do not fill a single icon with color, gradients, or shadows.
- Do not modify `viewBox`, stroke width, or corner radius just to "make it look nice".

## Acceptance

- [ ] Every `id` used is in the catalog
- [ ] Every `<use>` `href` finds a same-named `id` in this file's `<defs>`
- [ ] The legend lists the meanings of all icons used this time
- [ ] No cross-file reference, no external images, no raster image
- [ ] All icon sizes are consistent, stroke inheritance is uniform
