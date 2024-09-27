Here is a sample structure for the README file based on your project configuration:

---

# Build and Push Action

This project contains a custom GitHub Action that builds and pushes Docker images using the Harness Drone CI plugin.

## Overview

This action is designed to help automate Docker image builds and push operations, triggered within a Drone CI pipeline. By utilizing the `plugins/github-actions:0.0.3`, the action can be configured to run privileged tasks, including the building and pushing of Docker images to DockerHub.

## Prerequisites

- **DockerHub Account**: Ensure you have a valid DockerHub account and store your credentials securely in the pipeline secrets.
- **Drone CI**: Make sure that you have Drone CI installed and configured in your project.
- **GitHub Token**: You will need a GitHub token stored in the Drone secrets for authentication during the action execution.

## Usage

Here’s how you can use the action in your Drone CI pipeline:

```yaml
- step:
    type: Plugin
    name: Private Action
    identifier: private_action
    spec:
      connectorRef: account.DockerHubKroon
      image: plugins/github-actions:0.0.3
      privileged: true
      settings:
        uses: gregkroon/buildandpushaction@v1
        with:
          dockerfile: ./Dockerfile
          context: .
          image_name: munkys123/actions-golang
          username: munkys123
          password: <+secrets.getValue("account.dockerhubkroon")>
          tags: <+pipeline.sequenceId>
        env:
          GITHUB_TOKEN: <+secrets.getValue("gittoken")>
      runAsUser: "0"
      resources:
        limits:
          memory: 8G
          cpu: 2000m
```

### Step Breakdown

- **`type: Plugin`**: Indicates the use of a Drone plugin.
- **`image`**: Specifies the plugin image to use, in this case, the `plugins/github-actions` with version `0.0.3`.
- **`privileged: true`**: Ensures the step runs with the necessary permissions to use Docker-in-Docker (DinD).
- **`settings`**: These are passed to the GitHub Action:
  - `dockerfile`: Path to the Dockerfile.
  - `context`: Directory context for the Docker build.
  - `image_name`: The name of the Docker image to be pushed.
  - `username` and `password`: DockerHub credentials pulled from Drone secrets.
  - `tags`: Dynamically generated tags for the Docker image.
- **`env`**: Environment variables, including the `GITHUB_TOKEN` required for accessing GitHub.
- **`resources`**: Specifies the resource limits for the build step (8GB memory, 2000m CPU).
  
## Secrets

You will need to configure the following secrets in Drone CI:

- `account.dockerhubkroon`: DockerHub credentials.
- `gittoken`: GitHub token for authentication.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

This should provide a complete overview of how the plugin step functions and how to set it up within your pipeline. You can adjust the content to better suit your project details if necessary.
