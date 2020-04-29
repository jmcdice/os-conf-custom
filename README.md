# os-conf-custom
Customize os-conf bosh release for fun stuff.

### Upload the bosh release
```console
  $ bosh upload-release --sha1 6946056ad69ae378cb89c9ef76daf66370a7dc6a \
     https://bosh.io/d/github.com/cloudfoundry/os-conf-release?v=22.0.0
```

### Create os-conf manifest (vim project-manifest.yml)
```bash
---
releases:
  - name: os-conf
    version: 22.0.0

addons:
  - name: os-configuration
    # include:
      # deployments:
        # - cfcr
    jobs:
    - name: pre-start-script
      release: os-conf
      properties:
        script: |-
          #!/bin/bash
          echo "Hi everybody." > /root/os-conf-script.log
```

### Update runtime config
Any new deployments will get this by default
```console
  $ bosh update-config --name=project --type=runtime project-manifest.yml
```

Existing bosh deployments need to be updated with the new config (think.. apply-changes).
```console
  $ bosh -d <deployment> manifest > deployment.yml
  $ bosh -d <deployment> deploy deployment.yml
```
