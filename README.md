# IDC Viewers Sandbox - GitHub Actions Testing

This repository stores and manages GitHub Actions for various Imaging Data Commons (IDC) repositories.

### Associated IDC Repositories:

- [Viewers](https://github.com/OHIF/Viewers) (upstream)
- [ViewersV3](https://github.com/ImagingDataCommons/ViewersV3) (fork of upstream)
- [OHIF IDC Mode](https://github.com/ImagingDataCommons/ohif-idc-mode)
- [OHIF GCP Mode](https://github.com/ImagingDataCommons/ohif-gcp-mode)
- [Slim Viewer](https://github.com/ImagingDataCommons/slim)
- [DICOM Microscopy Viewer](https://github.com/ImagingDataCommons/dicom-microscopy-viewer)

## GitHub Actions Overview

This repository contains the following GitHub Actions for automated deployment and testing across IDC repositories:

- **ohif/deploy-v3**  
  Deploys a preview of the [ViewersV3](https://github.com/ImagingDataCommons/ViewersV3) repository’s `master` branch.
- **ohif/deploy-v3-with-add-ons**  
  Deploys a preview of the [ViewersV3](https://github.com/ImagingDataCommons/ViewersV3) `master` branch, pre-configured with the latest changes from:

  - [OHIF GCP Mode](https://github.com/ImagingDataCommons/ohif-gcp-mode)
  - [OHIF GCP Extension](https://github.com/ImagingDataCommons/ohif-gcp-extension)

- **ohif/deploy-v3-upstream**  
  Deploys a preview of the [Viewers](https://github.com/OHIF/Viewers) repository’s `master` branch.

- **ohif/deploy-v3-upstream-with-add-ons**  
  Deploys a preview of the [Viewers](https://github.com/OHIF/Viewers) `master` branch, pre-configured with the latest changes from:

  - [OHIF GCP Mode](https://github.com/ImagingDataCommons/ohif-gcp-mode)
  - [OHIF GCP Extension](https://github.com/ImagingDataCommons/ohif-gcp-extension)
 
  Format of the URL for configuring secondary GHC DICOM store: `https://viewers-sandbox-gha-testing.web.app/viewer?StudyInstanceUIDs=<UID>&gcp=projects/<project>/locations/<location>/datasets/<dataset>/dicomStores/<store>`

  Switching primary DICOM store: `http://localhost:3000/projects/project-x/locations/us/datasets/some-dataset/dicomStores/test-samples/study/1.3.6.1.4.1.123.5.2.1.123.123.123`


- **slim/deploy**  
  Deploys the latest version of [Slim Viewer](https://github.com/ImagingDataCommons/slim).

- **slim/deploy-with-dmv**  
  Deploys the latest version of [Slim Viewer](https://github.com/ImagingDataCommons/slim) along with the latest [DICOM Microscopy Viewer](https://github.com/ImagingDataCommons/dicom-microscopy-viewer).

### Deployment URL

Actions deploy:

* OHIF v3 to [https://viewers-sandbox-gha-testing.web.app/](https://viewers-sandbox-gha-testing.web.app/)
* Slim to [https://andrey-slim-test.web.app/](https://andrey-slim-test.web.app/)

Consent screen is managed under `idc-external-031`.

## Configuration

The following environment variables are required for configuring the GitHub Actions:

- **FIREBASE_PROJECT_ID**  
  Firebase project identifier.
- **OHIF_CONFIG_JS_URL**  
  URL to a Gist containing the OHIF configuration (with OIDC).

- **OHIF_FIREBASE_JSON_URL**  
  URL to the Firebase configuration for OHIF.

- **SLIM_CONFIG_JS_URL**  
  URL to a Gist containing the Slim Viewer configuration (with OIDC).

- **SLIM_FIREBASE_JSON_URL**  
  URL to the Firebase configuration for Slim Viewer.

---
