[![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-Workflows-2088FF?logo=github-actions&logoColor=white)](https://github.com/ImagingDataCommons/idc-viewers-sandbox-gha-testing/actions)

# IDC Viewers Sandbox

**GitHub Actions for automated deployment and testing of Imaging Data Commons (IDC) viewer applications**

This repository manages GitHub Actions workflows for deploying preview instances of various IDC medical imaging viewer applications to Firebase Hosting.

## Table of Contents

- [Associated Repositories](#associated-repositories)
- [Workflows](#workflows)
  - [OHIF Viewer Workflows](#ohif-viewer-workflows)
  - [Slim Viewer Workflows](#slim-viewer-workflows)
- [Deployment URLs](#deployment-urls)
- [Configuration](#configuration)
  - [Required Secrets](#required-secrets)
  - [Required Variables](#required-variables)
  - [Optional Variables](#optional-variables)
- [Package Managers](#package-managers)
- [License](#license)

## Associated Repositories

| Repository | Description |
| :--------- | :---------- |
| [OHIF Viewers](https://github.com/OHIF/Viewers) | Upstream OHIF medical imaging viewer |
| [ViewersV3](https://github.com/ImagingDataCommons/ViewersV3) | IDC fork of OHIF Viewers |
| [ohif-gcp-mode](https://github.com/ImagingDataCommons/ohif-gcp-mode) | OHIF mode for Google Cloud Platform integration |
| [ohif-gcp-extension](https://github.com/ImagingDataCommons/ohif-gcp-extension) | OHIF extension for Google Cloud Platform features |
| [Slim](https://github.com/ImagingDataCommons/slim) | Interoperable slide microscopy viewer |
| [dicom-microscopy-viewer](https://github.com/ImagingDataCommons/dicom-microscopy-viewer) | JavaScript library for DICOM microscopy viewing |

## Workflows

### OHIF Viewer Workflows

| Workflow | Description | Trigger |
| :------- | :---------- | :------ |
| `ohif/deploy-v3` | Deploy [ViewersV3](https://github.com/ImagingDataCommons/ViewersV3) | Manual |
| `ohif/deploy-v3-with-add-ons` | Deploy ViewersV3 with [ohif-gcp-mode](https://github.com/ImagingDataCommons/ohif-gcp-mode) and [ohif-gcp-extension](https://github.com/ImagingDataCommons/ohif-gcp-extension) | Manual |
| `ohif/deploy-v3-upstream` | Deploy upstream [OHIF Viewers](https://github.com/OHIF/Viewers) | Manual |
| `ohif/deploy-v3-upstream-with-add-ons` | Deploy upstream OHIF Viewers with GCP add-ons | Manual, Daily (11:00 UTC) |

**Workflow inputs:**

- `viewers_branch` / `ohif_version`: Branch, tag, or SHA to deploy
- `cs3d_version`: Optional Cornerstone3D version override (e.g., `3.0.0` or `latest`)

**Example URLs:**

```
# Standard viewer
https://viewers-sandbox-gha-testing.web.app/viewer?StudyInstanceUIDs=<UID>

# With secondary GCP DICOM store
https://viewers-sandbox-gha-testing.web.app/viewer?StudyInstanceUIDs=<UID>&gcp=projects/<project>/locations/<location>/datasets/<dataset>/dicomStores/<store>

# Direct GCP store access
https://viewers-sandbox-gha-testing.web.app/projects/<project>/locations/<location>/datasets/<dataset>/dicomStores/<store>/study/<StudyInstanceUID>
```

### Slim Viewer Workflows

| Workflow | Description | Trigger |
| :------- | :---------- | :------ |
| `slim/deploy` | Deploy [Slim](https://github.com/ImagingDataCommons/slim) | Manual |
| `slim/deploy-with-dmv` | Deploy Slim with linked [dicom-microscopy-viewer](https://github.com/ImagingDataCommons/dicom-microscopy-viewer) | Manual, Daily (11:00 UTC) |
| `slim/deploy-with-dmv-proxy` | Deploy Slim with DMV using proxy configuration | Manual |

**Workflow inputs:**

- `slim_branch`: Slim branch or tag to deploy
- `dmv_branch`: dicom-microscopy-viewer branch or tag to deploy

**Example URL:**

```
https://andrey-slim-test.web.app/studies/<StudyInstanceUID>?gcp=https://healthcare.googleapis.com/v1/<store>/dicomWeb
```

## Deployment URLs

| Application | URL |
| :---------- | :-- |
| OHIF Viewer | https://viewers-sandbox-gha-testing.web.app/ |
| Slim Viewer | https://andrey-slim-test.web.app/ |

> **Note:** OAuth consent screen is managed under `idc-external-031`.

## Configuration

### Required Secrets

| Secret | Description |
| :----- | :---------- |
| `FIREBASE_SERVICE_ACCOUNT` | Firebase service account for OHIF deployments |
| `FIREBASE_SERVICE_ACCOUNT_SLIM` | Firebase service account for Slim deployments |

### Required Variables

| Variable | Description |
| :------- | :---------- |
| `FIREBASE_PROJECT_ID` | Firebase project ID for OHIF deployments |
| `FIREBASE_PROJECT_ID_SLIM` | Firebase project ID for Slim deployments |
| `OHIF_CONFIG_JS_URL` | URL to OHIF configuration file (with OIDC) |
| `OHIF_FIREBASE_JSON_URL` | URL to Firebase configuration for OHIF |
| `SLIM_CONFIG_JS_URL` | URL to Slim configuration file (with OIDC) |
| `SLIM_FIREBASE_JSON_URL` | URL to Firebase configuration for Slim |

### Optional Variables

| Variable | Description | Default |
| :------- | :---------- | :------ |
| `VIEWERS_BRANCH` | Default branch for ViewersV3 | `master` |
| `OHIF_VERSION_TAG` | Default version for upstream OHIF | default branch |
| `CS3D_VERSION` | Default Cornerstone3D version override | from repo |
| `SLIM_BRANCH` | Default branch for Slim | `master` |
| `DMV_BRANCH` | Default branch for dicom-microscopy-viewer | `master` |
| `SLIM_PROXY_CONFIG_JS_URL` | Slim configuration for proxy deployments | — |

## Package Managers

**OHIF workflows** use **Yarn** for dependency management.

**Slim workflows** use **pnpm** (`packageManager: pnpm@10.34.1`). The `slim/deploy-with-dmv*` workflows link the local dicom-microscopy-viewer build:

```sh
cd dicom-microscopy-viewer && pnpm install --frozen-lockfile && pnpm run build
cd ../slim && pnpm link ../dicom-microscopy-viewer && pnpm install --no-frozen-lockfile
```

> **Note:** With pnpm 10, `pnpm link` records the link locally. Running `pnpm install` afterward ensures `node_modules` resolves to the sibling DMV build rather than the registry copy.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
