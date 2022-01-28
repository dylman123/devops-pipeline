# devops-pipeline
An example devops pipeline for prototyping.
Author: Dylan Klein

## What does this pipeline do?
The demo pipeline builds a simple "hit counter" web app using Python's Flask framework with Redis as a database. It then deploys the dockerised architecture to Google Kubernetes Engine (GKE).

### 1. Code
Developers can develop the Flask app on their localhost in a containerised environment using Docker.

### 2. Build
The following docker image is built whenever the `main` branch receives a `git push` via GitHub Actions (Continuous Integration):
- `dylan-website` (Flask frontend)

### 3. Deploy
Once the docker build is successful, GitHub Actions deploys the following 2 docker images as workloads to GKE (Continuous Deployment):
1. `gke-test` (Flask frontend)
2. `redis-master` (Redis backend)

GitHub Actions also deploys the following 2 services, such that the workloads mentioned above can be accessed:
1. `gke-test-service` (External Loadbalancer - i.e. internet facing)
2. `my-redis-svc` (ClusterIP)

## Architecture Diagram
The following is a visual representation of the CICD process.
![Architecture Diagram](devops-pipeline.drawio.png)

## Installation
1. Create a new GCP project and authorise GitHub Actions via IAM. 
Follow these instructions for help: https://github.com/google-github-actions/setup-gcloud/tree/master/example-workflows/gke-kustomize
2. Git clone this repository:
`$ git clone https://github.com/dylman123/devops-pipeline.git`
3. Add your GCP project ID as a secret in your project repository, using `GKE_PROJECT` as the key.
4. Modify `cicd-pipeline.yml` with your specific env variables.
5. Modify `cicd-pipeline.yml` with your specific auth details for:
- `workload_identity_provider` and
- `service_account`

## Run
To run, all you have to do is `git push` your code or manually click 'Re-run all jobs' in GitHub Actions.

## Test
To ensure your deployment worked successfully:
1. Find the IP address of the `gke-test-service` service which you deployed in GCP.
2. Navigate to the IP address in your browser on port 80.
