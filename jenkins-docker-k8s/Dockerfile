FROM jenkins/jenkins:lts
 
# Switch to root to install Docker
USER root
 
# Install Docker CLI
RUN apt-get update && \
    apt-get install -y docker.io && \
    rm -rf /var/lib/apt/lists/*
 
# Create docker group if it doesn't exist and add jenkins user to it
RUN groupadd -for docker && usermod -aG docker jenkins
 
# Switch back to jenkins user
USER jenkins

 
