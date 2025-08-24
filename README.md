# Displace WordPress Template

A production-ready WordPress deployment template for Kubernetes, designed for use with the [Displace CLI](https://displace.tech).

## Overview

This template provides a complete WordPress deployment including:

- **WordPress** - Latest Bitnami WordPress image with persistent storage
- **MariaDB** - Database backend with persistent storage  
- **Ingress** - External access configuration
- **Secrets Management** - Secure credential handling with git-safe templates
- **Security Features** - Automatic .gitignore generation and credential separation

## Template Structure

```
templates/
├── .gitignore.tmpl                    # Git ignore file template
├── .credentials.tmpl                  # Credential file template (git-ignored)
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

The following variables are available in templates:

- `{{.ProjectName}}` - The project name
- `{{.Namespace}}` - Kubernetes namespace
- `{{.DBRootPassword}}` - MariaDB root password
- `{{.DBPassword}}` - WordPress database password
- `{{.WordPressPassword}}` - WordPress admin password
- `{{.DBRootPasswordB64}}` - Base64 encoded root password
- `{{.DBPasswordB64}}` - Base64 encoded database password

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

## Attribution

This template is based on and inspired by the excellent work done by Bitnami:

- **Original Helm Chart**: [bitnami/wordpress](https://artifacthub.io/packages/helm/bitnami/wordpress)
- **License**: Apache License 2.0
- **Images**: [Bitnami WordPress](https://github.com/bitnami/containers/tree/main/bitnami/wordpress) and [Bitnami MariaDB](https://github.com/bitnami/containers/tree/main/bitnami/mariadb)

This pared-down template provides a simplified, security-focused WordPress deployment optimized for the Displace CLI workflow while maintaining compatibility with the robust Bitnami container ecosystem.

## License

Copyright © 2025 Displace Technologies

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
