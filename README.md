### YAML snippet


- step:
    type: Plugin
    name: private action
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
