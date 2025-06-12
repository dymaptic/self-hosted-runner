# Self-Hosted Runner Dockerization

Based on the original work by [youssefbrr](https://github.com/youssefbrr/self-hosted-runner)

Welcome to the GitHub Self-Hosted Runner Dockerization repository. This project provides a Dockerized solution for setting up a self-hosted GitHub Actions runner.

## Features

- **Docker Compose Setup**: Easily deploy self-hosted runners using Docker Compose.
- **Customizable**: Use the provided Docker image or build your own using the Dockerfile.
- **Scalable**: Deploy multiple runner replicas with resource constraints.
- **Cross-Platform**: Support for Linux, macOS, and Windows environments.

## Repository Contents

- `LICENSE`: The license file for this project.
- `README.md`: The documentation file you are currently reading.
- `docker-compose.yml`: The Docker Compose file to deploy the self-hosted runner on Linux.
- `Runner/`: A directory containing the Dockerfile and start scripts for building the runner image.

## Getting Started

### Prerequisites

- Docker
- Docker Compose
- Docker Desktop (for macOS and Windows users)
- Get a GitHub runner registration token from your GitHub repository settings. Go to your repository, click on "Settings" > "Actions" > "Runners" > "New self-hosted runner", scroll down to the `Configure` script, and copy the value after `--token`.

### Using Docker Compose

1. Clone the repository:

   ```sh
   git clone https://github.com/youssefbrr/self-hosted-runner.git
   cd self-hosted-runner
   ```

2. Edit the `docker-compose.yml` file to specify your repository, registration token, and runner name.

3. Build the Docker image. This step is especially necessary if you have made changes to the Dockerfile since a previous build:

   ```sh
   docker-compose build --no-cache
   ```

4. Deploy the self-hosted runner:
   ```sh
   docker-compose up
   ```

5. Tear down the deployment if you don't want to leave it running:

   ```sh
   docker-compose down
   ```

## Configuration

### Environment Variables

Add your environment variables in a file called `.env` in the root directory of the repository. The `.env` file should contain the following variables:

- `REPO`: The GitHub repository to register the runner to (format: `<owner>/<repo>`).
- `REG_TOKEN`: The registration token for the self-hosted runner from the GitHub repository settings.
- `NAME`: The name of the self-hosted runner.

### Example `.env` File

```env
REPO=dymaptic/GeoBlazor
REG_TOKEN=abcd1234efgh5678ijk
NAME=self-hosted-runner
```
