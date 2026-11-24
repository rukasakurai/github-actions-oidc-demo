# GitHub Actions OIDC Demo

A minimal demonstration of GitHub Actions authenticating to Azure without storing long-lived credentials.

## How it works

1. GitHub Actions issues a short-lived OpenID Connect (OIDC) token to the job. The workflow needs `id-token: write` to request it.
2. `azure/login@v2` presents that token to Microsoft Entra ID, which checks the configured federated trust for the repository and branch.
3. Azure issues a short-lived access token for the identity. The workflow then uses the authenticated Azure CLI account.

No client secret, password, or long-lived access token is stored. Azure permissions are assigned separately through RBAC; `id-token: write` does not itself grant Azure access.

## Run the demo

Follow [the Azure OIDC setup guide](docs/azure-oidc-setup.md) to create an identity, configure federated trust for your repository's `main` branch, and grant Reader access at the narrowest appropriate scope.

Configure these values under **Settings → Secrets and variables → Actions**:

| Name | Stored as |
|------|-----------|
| `AZURE_CLIENT_ID` | Repository variable |
| `AZURE_TENANT_ID` | Repository secret |
| `AZURE_SUBSCRIPTION_ID` | Repository secret |

These are **identifiers, not credentials**. This repository keeps the tenant and subscription IDs in secrets to reduce accidental disclosure of environment metadata; OIDC security does not depend on hiding them.

1. Open **Actions → Azure OIDC Connectivity Check → Run workflow**.
2. Select **main**, matching the branch trusted by Azure, and run the workflow.
3. Inspect the **Azure Login via OIDC** and **Verify Azure connectivity** steps.

A successful run shows the selected Azure account and `Azure OIDC authentication successful`. The existing `az account show` command verifies authenticated account context; it does not itself query an Azure resource API. This workflow creates, deploys, and deletes no Azure resources.

The demo is implemented in [`.github/workflows/azure-oidc-check.yml`](.github/workflows/azure-oidc-check.yml). For failures, see [troubleshooting](docs/azure-oidc-setup.md#troubleshooting).

## License

[MIT](LICENSE).

## Optional guidance

The following bootstrap guidance is **not required for the OIDC demo**.

### Post-Creation Checklist

The retained [`repo-setup` Agent Skill](.github/skills/repo-setup/SKILL.md) can help with optional follow-up setup:

- [x] **Choose and add a LICENSE file** - This repository uses [MIT](LICENSE)
- [ ] **Configure Azure OIDC** - Set up federated credentials and configure the following repository variable and secrets:
  - `AZURE_CLIENT_ID` (repository variable)
  - `AZURE_TENANT_ID` (repository secret)
  - `AZURE_SUBSCRIPTION_ID` (repository secret)

  See [docs/azure-oidc-setup.md](docs/azure-oidc-setup.md) for detailed setup instructions. Then run the "Azure OIDC Connectivity Check" workflow manually to verify the configuration.
- [ ] **Discover and install Agent Skills** (optional) - Find reusable [Agent Skills](https://agentskills.io) relevant to your stack and install them with [`gh skill install`](https://cli.github.com/manual/gh_skill_install) (GitHub CLI v2.90.0+). See the [`repo-setup` skill](.github/skills/repo-setup/SKILL.md#discover-and-install-agent-skills) for where to look and how to vet them.
- [x] **Update README.md** - Document this repository's OIDC demo
- [x] **Review AGENTS.md** - Describe this repository's purpose and conventions
