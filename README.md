# vscode

VSCode template for typescript development workspace

1. Install [Visual Studio Code Remote - Containers extension](https://code.visualstudio.com/docs/remote/containers)
2. Clone this repo, copy the `.devcontainer` directory to your workspace
3. Edit `.devcontainer/devcontainer.json` as per your settings. Pay special attention to 'mounts` and `containerEnv`.
4. Edit `.devcontainer/Dockerfile` for any changes you wish to make
5. Export GITHUB_TOKEN to your shell (or add it to your `.[ba|z]shrc` file)
4. Load your workspace in the Container using the extension -> this will first build the container using the Dockerfile and then load your workspace directory in `/workspaces` in your container.
