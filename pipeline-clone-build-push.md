# Pipeline manifests

```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: clone-build-push
spec:
  description: |
    This pipeline clones a git repo, builds a Docker image with Kaniko and
    pushes it to a registry    
  params:
  - name: repo-url
    type: string
  - name: image-reference
    type: string
  workspaces:
  - name: shared-data
  - name: docker-credentials
  tasks:
  - name: fetch-source
    taskRef:
      name: git-clone
    workspaces:
    - name: output
      workspace: shared-data
    params:
    - name: url
      value: $(params.repo-url)
  - name: build-push
    runAfter: ["fetch-source"]
    taskRef:
      name: kaniko
    workspaces:
    - name: source
      workspace: shared-data
    - name: dockerconfig
      workspace: docker-credentials
    params:
    - name: IMAGE
      value: $(params.image-reference)
```

# PipelineRun manifests

```yaml
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  generateName: clone-build-push-run-
spec:
  pipelineRef:
    name: clone-build-push
  podTemplate:
    securityContext:
      fsGroup: 65532
  workspaces:
  - name: shared-data
    volumeClaimTemplate:
      spec:
        accessModes:
        - ReadWriteOnce
        resources:
          requests:
            storage: 1Gi
  - name: docker-credentials
    secret:
      secretName: docker-credentials
  params:
  - name: repo-url
    value: https://github.com/google/docsy-example.git
  - name: image-reference
    value: container.registry.com/sublocation/my_app:version
```

# container registry authentication
kubernetes secret used for registry authentication

get the base64 encoded file 
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: docker-credentials
data:
  config.json: efuJAmF1...
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: docker-credentials
data:
  config.json: efuJAmF1...
```
