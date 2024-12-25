# Use the official Jenkins LTS image as the base image
FROM jenkins/jenkins:lts

# Switch to root to install dependencies
USER root

# Install Docker dependencies
RUN apt-get update && \
    apt-get install -y \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Add Docker’s official GPG key
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/trusted.gpg.d/docker.asc

# Set up the Docker stable repository
RUN echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list

# Install Docker Engine
RUN apt-get update && \
    apt-get install -y docker-ce docker-ce-cli containerd.io && \
    rm -rf /var/lib/apt/lists/*

# Add the Jenkins user to the Docker group
RUN usermod -aG docker jenkins

# Ensure the Jenkins user can access Docker
RUN chown -R jenkins:jenkins /var/run/docker.sock

# Switch back to the Jenkins user
USER jenkins

# Expose necessary ports
EXPOSE 8080

# Start Jenkins
ENTRYPOINT ["/usr/bin/java", "-jar", "/usr/share/jenkins/jenkins.war"]
