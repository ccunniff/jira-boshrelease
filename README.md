# Jira BOSH Release

## Running Locally

### Install BOSH Lite

Install BOSH Lite using the instructions at https://github.com/cloudfoundry/bosh-lite.  You do not need to install Cloud Foundry.

### Deploy Jira Release

* Upload the Warden stemcell

 A stemcell is a VM template with an embedded BOSH Agent. BOSH Lite uses the Warden CPI, so we need to use the Warden Stemcell which will be the root file system for all Linux Containers created by the Warden CPI.

 Download latest Warden stemcell:

 ```
 wget http://bosh-jenkins-artifacts.s3.amazonaws.com/bosh-stemcell/warden/latest-bosh-stemcell-warden.tgz
 ```

 Upload the stemcell:

 ```
 bosh upload stemcell latest-bosh-stemcell-warden.tgz
 ```

* Populate the blobs directory

The blobs aren't published yet to a blobstore so we need to populate our local blobs directory. It should look like the following:

```
$ tree blobs/
blobs
├── atlassian-jira-6.4.12
│   └── atlassian-jira-6.4.12.tar.gz
└── jdk-8u71-linux-x64
    └── jdk-8u71-linux-x64.gz

2 directories, 2 files
```

* Upload the jira release

Create a dev release:

```
bosh create release --force 
```

Upload the release:

```
bosh upload release
```

* Generate a deployment manifest

```
bosh-lite/make_manifest
```

* Deploy

Target the generated deployment manifest

```
bosh deployment bosh-lite/manifests/jira-manifest.yml
```

Deploy

```
bosh deploy
```

Cross your fingers.

Jira server is available at http://10.244.2.2:8080

For a Bamboo example see https://github.com/davidehringer/bamboo-boshrelease