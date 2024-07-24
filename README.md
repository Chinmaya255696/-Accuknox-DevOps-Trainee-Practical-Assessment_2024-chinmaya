# Wisecow Application Deployment on Kubernetes

## Objective

To containerize and deploy the Wisecow application, hosted in the GitHub repository, on a Kubernetes environment with secure TLS communication. In this deployment, we are using a local Kubernetes cluster, and the application will run on localhost.

## Getting Started

To run this project on your local machine, follow these steps:

### Installation

Install `fortune-mod` and `cowsay` on Debian-based systems like Ubuntu using the following commands:

```bash
sudo apt update
sudo apt install fortune-mod cowsay -y
