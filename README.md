# Tekton on minikube
## Tekton CLI
**Step 1:** Download the Tekton CLI
```bash
wget https://github.com/tektoncd/cli/releases/download/v0.31.0/tkn_0.31.0_Linux_x86_64.tar.gz
```
Replace v0.31.0 with the latest version if necessary.

**Step 2:** Extract the Binary
Extract the Downloaded Archive:
Use tar to extract the downloaded archive:

```bash
tar -xvzf tkn_0.31.0_Linux_x86_64.tar.gz
```

This will extract the tkn binary.

**Step 3:** Move the Binary to a Directory in Your PATH
Move the Binary:
Move the tkn binary to a directory that is in your PATH, such as /usr/local/bin:

```bash
sudo mv tkn /usr/local/bin/
```

Verify the Installation:
Check that the tkn command is available:

```bash
tkn version
```
This should display the version of the Tekton CLI you installed.

**Step 4:** Verify the CLI is Working
Check Tekton Resources:
You can now use the tkn CLI to interact with your Tekton resources. For example, to list all pipelines:

```bash
tkn pipeline list
```
Or to list all tasks:

```bash
tkn task list
```

## Minikube setup

**Step 1:** Start Minikube
If Minikube is not already running, start it:

```bash
minikube start
```

**Step 2:** Install Tekton Pipelines
You can install Tekton Pipelines using kubectl or Helm.

**Option 1:** Install Tekton Pipelines using kubectl
Apply the Tekton Pipelines release YAML:

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

Verify the installation:

Wait for the Tekton Pipelines components to be deployed. You can check the status of the pods in the tekton-pipelines namespace:

```bash
kubectl get pods --namespace tekton-pipelines --watch
```

Once all the pods are in the Running state, Tekton Pipelines is successfully installed.

**Option 2:** Install Tekton Pipelines using Helm
Add the Tekton Helm repository:

```bash
helm repo add tekton https://charts.tekton.dev
helm repo update
```

Install Tekton Pipelines:

```bash
helm install tekton-pipelines tekton/tekton-pipeline --namespace tekton-pipelines --create-namespace
```

Verify the installation:

Check the status of the pods in the tekton-pipelines namespace:

```bash
kubectl get pods --namespace tekton-pipelines --watch
```

**Step 3:** Install Tekton Triggers
If you want to install Tekton Triggers, follow these steps:

Apply the Tekton Triggers release YAML:

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
```

Verify the installation:

Check the status of the pods in the tekton-pipelines namespace:

```bash
kubectl get pods --namespace tekton-pipelines --watch
```

**Step 4:** Install Tekton Dashboard
To install the Tekton Dashboard:

Apply the Tekton Dashboard release YAML:

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/dashboard/latest/release.yaml
```

Verify the installation:

Check the status of the pods in the tekton-pipelines namespace:

```bash
kubectl get pods --namespace tekton-pipelines --watch
```

**Step 5:** Access the Tekton Dashboard
To access the Tekton Dashboard, you can use kubectl port-forward:

```bash
kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097
```

Now, you can access the Tekton Dashboard by navigating to `http://localhost:9097` in your web browser.

## Create a Sample Tekton Task and Pipeline

```bash
kubectl apply -f tasks.yaml
kubectl apply -f pipeline.yaml
tkn pipeline start cd-pipeline \
    --showlog \
    -p repo-url="https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git" \
    -p branch="main"

```

```bash
PipelineRun started: cd-pipeline-run-9xxks
Waiting for logs to be available...
[clone : checkout] Cloning into 'wtecc-CICD_PracticeCode'...

[lint : echo-message] Calling Flake8 linter...

[tests : echo-message] Running unit tests with PyUnit...

[build : echo-message] Building image for https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git ...

[deploy : echo-message] Deploying main branch of https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git ...

```

## Useful Commands

The following commands help you understand and effectively use the Tekton CLI:

 * `tkn help:` Displays a list of the commands with helpful information.
 * [`tkn bundle:`](docs/cmd/tkn_bundle.md) Manage Tekton [bundles](https://github.com/tektoncd/pipeline/blob/main/docs/tekton-bundle-contracts.md)
 * [`tkn clustertask:`](docs/cmd/tkn_clustertask.md) Parent command of the ClusterTask command group.
 * [`tkn clustertriggerbinding:`](docs/cmd/tkn_clustertriggerbinding.md) Parent command of the ClusterTriggerBinding command group.
 * [`tkn completion:`](docs/cmd/tkn_completion.md) Outputs a BASH, ZSH, Fish or PowerShell completion script for `tkn` to allow command completion with Tab.
 * [`tkn eventlistener:`](docs/cmd/tkn_eventlistener.md) Parent command of the Eventlistener command group.
 * [`tkn hub:`](docs/cmd/tkn_hub.md) Search and install Tekton Resources from [Hub](https://hub.tekton.dev)
 * [`tkn pipeline:`](docs/cmd/tkn_pipeline.md) Parent command of the Pipeline command group.
 * [`tkn pipelinerun:`](docs/cmd/tkn_pipelinerun.md) Parent command of the Pipelinerun command group.
 * [`tkn task:`](docs/cmd/tkn_task.md) Parent command of the Task command group.
 * [`tkn taskrun:`](docs/cmd/tkn_taskrun.md) Parent command of the Taskrun command group.
 * [`tkn triggerbinding:`](docs/cmd/tkn_triggerbinding.md) Parent command of the Triggerbinding command group.
 * [`tkn triggertemplate:`](docs/cmd/tkn_triggertemplate.md) Parent command of the Triggertemplate command group.
 * [`tkn version:`](docs/cmd/tkn_version.md) Outputs the cli version.

For every `tkn` command, you can use `-h` or `--help` flags to display specific help for that command.
