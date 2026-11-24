# GitHub Actions OIDC Demo

A minimal demonstration of GitHub Actions authenticating to Azure without storing long-lived credentials.

Alternatively, a workflow can authenticate using a long-lived credential, such as an Azure client secret, stored in GitHub Secrets. This demo uses OIDC instead to obtain short-lived access tokens.

## How it works

1. GitHub Actions issues a short-lived OpenID Connect (OIDC) token to the job. The workflow needs `id-token: write` to request it.
2. `azure/login@v2` presents that token to Microsoft Entra ID, which checks the configured federated trust for the repository and branch.
3. Azure issues a short-lived access token for the identity. The workflow then uses the authenticated Azure CLI account.

No client secret, password, or long-lived access token is stored. Azure permissions are assigned separately through RBAC; `id-token: write` does not itself grant Azure access.

## Run the demo

Follow [the Azure OIDC setup guide](docs/azure-oidc-setup.md) to create an identity, configure federated trust for your repository's `main` branch, and grant Reader access at the narrowest appropriate scope.

1. Open **Actions → Azure OIDC Connectivity Check → Run workflow**.
2. Select **main**, matching the branch trusted by Azure, and run the workflow.
3. Inspect the **Azure Login via OIDC** and **Verify Azure connectivity** steps.

A successful run shows the selected Azure account and `Azure OIDC authentication successful`. The existing `az account show` command verifies authenticated account context; it does not itself query an Azure resource API. This workflow creates, deploys, and deletes no Azure resources.

The demo is implemented in [`.github/workflows/azure-oidc-check.yml`](.github/workflows/azure-oidc-check.yml). For failures, see [troubleshooting](docs/azure-oidc-setup.md#troubleshooting).

## License

[MIT](LICENSE).
