FROM ubuntu:22.04

ARG RUNNER_VERSION="2.328.0"
ARG DEBIAN_FRONTEND=noninteractive

# Update and upgrade the system
RUN apt update -y && apt upgrade -y

# Add a user named docker
RUN useradd -m docker

# Add sudo support
RUN apt-get update && apt-get install -y sudo
RUN echo 'docker ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# Install necessary packages
RUN apt install -y --no-install-recommends \
    curl build-essential libssl-dev libffi-dev python3 python3-venv python3-dev python3-pip jq
RUN apt-get -yqq install ssh
# Install Git 2.18 or higher
RUN apt install -y git && git --version
# Install PowerShell
RUN apt update && apt install -y wget apt-transport-https software-properties-common \
    && wget -q https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && apt update && apt install -y powershell
# Install Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash
# Set up the actions runner
RUN cd /home/docker && mkdir actions-runner && cd actions-runner \
    && curl -o actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz -L https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz \
    && tar xzf actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz

# Change ownership to docker user and install dependencies
RUN chown -R docker /home/docker && /home/docker/actions-runner/bin/installdependencies.sh

# Grant permissions to docker user for /usr/share/dotnet
RUN mkdir -p /usr/share/dotnet && chown docker:docker /usr/share/dotnet

# Copy the start script and make it executable
COPY start.sh /start.sh
RUN chmod +x /start.sh
# Ensure start.sh has LF line endings
RUN sed -i 's/\r$//' /start.sh

# Switch to docker user
USER docker

# Define the entrypoint
ENTRYPOINT ["/start.sh"]