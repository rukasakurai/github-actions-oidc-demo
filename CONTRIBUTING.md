# Contributing

Thank you for your interest in contributing to this project. This document is the canonical source of contribution guidelines and applies to **all contributors**—whether working manually or through automated tooling.

## Collaboration & Decision-Making Style

- **Start lean**: Prefer minimal, focused implementations over comprehensive solutions
- **Minimize changes**: Make the smallest possible modifications to achieve the goal
- **Defer specifics**: Avoid hardcoding environment-specific values (tenant IDs, subscription IDs, secrets)
- **Public-safe by default**: Never suggest or add sensitive information (secrets, Azure IDs, API keys)
- **Demo-first thinking**: Keep additions focused on explaining or demonstrating OIDC authentication
- **Respect established decisions**: Do not change the MIT license or add specific deployment configurations or technology-specific scaffolding unless explicitly requested
- **Validate assumptions**: When working on derived repositories, check if baseline conventions are still relevant before applying them

## Constraints & Assumptions

- **MIT license**: Contributions are covered by the repository's [MIT license](LICENSE)
- **No automatic CI/CD**: Continuous integration and deployment patterns vary by project
- **No committed credentials or environment-specific IDs**: Configure Azure identifiers through repository variables/secrets
- **No application stack assumptions**: No language-specific tooling, frameworks, or build systems
