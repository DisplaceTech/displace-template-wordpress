# Displace WordPress Template

A production-ready WordPress deployment template for Kubernetes, designed for use with the [Displace CLI](https://displace.tech).

## Overview

This template provides everything you need to deploy WordPress to Kubernetes with enterprise-grade features:

- 🐘 **WordPress** - Latest Bitnami WordPress image with persistent storage
- 🗄️ **MariaDB** - Database backend with persistent storage
- ☸️ **Kubernetes Manifests** - Production-ready deployment without Helm complexity
- 🔒 **Automatic HTTPS** - SSL certificates via cert-manager and Let's Encrypt
- 🛠️ **Cross-platform Tooling** - Makefile that works on Linux, macOS, and Windows
- 📊 **Built-in Monitoring** - Health checks, resource limits, and observability
- 🔐 **Security First** - Secure credential management with git-safe templates

## Template Structure

```
templates/
├── .gitignore.tmpl                    # Git ignore file template
├── .credentials.tmpl                  # Credential file template (git-ignored)
├── Makefile.tmpl                      # Cross-platform commands
├── README.md.tmpl                     # Project documentation template
├── manifests/                         # Kubernetes manifests
│   ├── 01-namespace.yaml.tmpl         # Namespace definition
│   ├── 02-database-secret.yaml.tmpl   # Secret template (placeholders)
│   ├── 03-mariadb.yaml.tmpl          # MariaDB deployment & service
│   ├── 04-wordpress.yaml.tmpl        # WordPress deployment & service
│   └── 05-ingress.yaml.tmpl          # Ingress configuration
└── secrets/                          # Secret overrides (git-ignored)
    └── database-secret-override.yaml.tmpl  # Real credentials
```

## Quick Start

### 1. Initialize Your Project

```bash
# Initialize a new WordPress project
displace project init wordpress

# You'll be prompted for:
# - Project name (e.g., "my-blog")
# - Kubernetes namespace (defaults to project name)
```

### 2. Deploy to Kubernetes

```bash
cd my-blog

# Deploy everything with one command
make deploy

# Check status
make status

# View logs
make logs
```

### 3. Access WordPress

```bash
# Forward local port to WordPress service
make port-forward

# Then visit http://localhost:8080
# Admin panel: http://localhost:8080/wp-admin
```

## Usage

This template is automatically downloaded and used by the Displace CLI when initializing WordPress projects:

```bash
displace project init wordpress
```

The CLI will:
1. Download the latest template release
2. Generate secure passwords
3. Process templates with project-specific values
4. Create all necessary files with proper security settings

## Template Variables

### Required Variables
| Variable | Description | Sensitive |
|----------|-------------|-----------|
| `ProjectName` | Project identifier | No |
| `Namespace` | Kubernetes namespace | No |
| `DBRootPassword` | MariaDB root password | Yes |
| `DBPassword` | WordPress database password | Yes |
| `WordPressPassword` | WordPress admin password | Yes |

### Optional Variables
| Variable | Default | Description |
|----------|---------|-------------|
| `ProjectDisplayName` | `{{.ProjectName}}` | Human-readable name |
| `WordPressReplicas` | `1` | Number of WordPress replicas |
| `WordPressStorageSize` | `10Gi` | WordPress PVC size |
| `MariaDBStorageSize` | `8Gi` | MariaDB PVC size |
| `IngressClass` | `nginx` | Ingress controller class |
| `CertIssuer` | `letsencrypt-prod` | Cert-manager issuer |

## Available Commands

The generated Makefile includes comprehensive commands:

### Deployment
| Command | Description |
|---------|-------------|
| `make deploy` | Deploy WordPress to Kubernetes |
| `make destroy` | Remove deployment from cluster |
| `make status` | Check deployment status |

### Monitoring
| Command | Description |
|---------|-------------|
| `make logs` | View WordPress application logs |
| `make logs-db` | View MariaDB database logs |
| `make events` | View recent Kubernetes events |

### Access
| Command | Description |
|---------|-------------|
| `make shell` | Access WordPress pod shell |
| `make shell-db` | Access MariaDB pod shell |
| `make mysql` | Connect to MySQL CLI |
| `make port-forward` | Forward local port to WordPress |
| `make open` | Open WordPress in browser |

### Database Management
| Command | Description |
|---------|-------------|
| `make backup-db` | Backup database to local file |
| `make restore-db` | Restore database from backup |
| `make backup-content` | Backup wp-content directory |

### Scaling & Utilities
| Command | Description |
|---------|-------------|
| `make scale REPLICAS=N` | Scale WordPress replicas |
| `make validate` | Validate Kubernetes manifests |
| `make info` | Display project information |

## Security Features

### Git Safety
- Automatic `.gitignore` generation protects sensitive files
- Separate credential files (`.credentials`) automatically excluded from version control
- Kubernetes secrets use placeholder values in main templates
- Real credentials stored in git-ignored override files

### Credential Management
- Secure password generation with YAML-safe character sets
- Environment variable format for easy shell integration
- Clear separation between public manifests and private credentials

## Docker Images

This template uses official Bitnami images:

- **WordPress**: `docker.io/bitnami/wordpress:6.8.2-debian-12-r4`
- **MariaDB**: `docker.io/bitnami/mariadb:11.5.2-debian-12-r4`

## Troubleshooting

### Common Issues

#### 1. Pods Not Starting
```bash
make status
make events
```

#### 2. Database Connection Issues
```bash
# Check MariaDB is running
make logs-db

# Test connection
make mysql
```

#### 3. HTTPS Certificate Issues
```bash
kubectl get certificates -n [namespace]
kubectl logs -n cert-manager deployment/cert-manager
```

## Attribution

This template is based on and inspired by the excellent work done by Bitnami:

- **Original Helm Chart**: [bitnami/wordpress](https://artifacthub.io/packages/helm/bitnami/wordpress)
- **License**: Apache License 2.0
- **Images**: [Bitnami WordPress](https://github.com/bitnami/containers/tree/main/bitnami/wordpress) and [Bitnami MariaDB](https://github.com/bitnami/containers/tree/main/bitnami/mariadb)

This template provides a simplified, security-focused WordPress deployment optimized for the Displace CLI workflow.

## License

Copyright 2025 Displace Technologies

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## About Displace Technologies

[Displace Technologies](https://displace.tech) - Making Kubernetes deployment simple and reliable.
