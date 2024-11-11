<div align="center">
  <img src="./assets/Syrin.png" alt="SYRIN Logo" width="300"/>
</div>

# SYRIN Installation Guide

SYRIN is an innovative solution designed to enhance IT infrastructure monitoring with critical alert humanization, improving awareness and engagement in incident response. This guide provides a step-by-step process to install and configure SYRIN components in a Kubernetes environment.

## Prerequisites

- **Kubernetes Cluster**: Ensure you have a functional Kubernetes cluster.
- **kubectl**: Command-line tool for controlling Kubernetes clusters.
- **Namespace Configuration**: A designated namespace for SYRIN components.

## Installation Steps

Follow these steps to deploy SYRIN and its components:

### Step 1: Set Up Namespace

Create the namespace for organizing SYRIN components:

```bash
kubectl apply -f ./k8s/namespace.yaml
```

### Step 2: Install Base Components

SYRIN depends on certain base services for handling messaging, storage, and language processing. Install these components in the specified order:

#### 1. RabbitMQ

RabbitMQ is the message broker used by SYRIN to queue and route messages among services.

```bash
kubectl apply -f ./k8s/01-base/01-rabbitmq/
```

#### 2. MinIO

MinIO provides storage for SYRIN audio files. This is where generated audio files will be stored, retrieved, and managed.

```bash
kubectl apply -f ./k8s/01-base/02-minio/
```

#### 3. Ollama

Ollama is an AI language model used by SYRIN for text humanization and language processing.

```bash
kubectl apply -f ./k8s/01-base/03-ollama/
```

### Step 3: Deploy SYRIN Components

With the base components set up, proceed with installing each SYRIN component:

#### 1. syrin-webhook

This component handles incoming webhook notifications and processes them into the SYRIN system.

```bash
kubectl apply -f ./k8s/02-syrin-webhook/
```

#### 2. syrin-bridge

The bridge component is responsible for message routing within SYRIN, connecting various services and ensuring efficient message flow.

```bash
kubectl apply -f ./k8s/03-syrin-bridge/
```

#### 3. syrin-humanization

SYRIN Humanization applies AI-powered processing to transform system messages into human-readable and engaging alerts.

```bash
kubectl apply -f ./k8s/04-syrin-humanization/
```

#### 4. syrin-make-audio-tts

This component generates audio from text messages, enabling auditory notifications for alerts.

```bash
kubectl apply -f ./k8s/05-syrin-make-audio-tts/
```

#### 5. syrin-notify-message

The message notification component manages outgoing notifications, sending messages to configured endpoints.

```bash
kubectl apply -f ./k8s/06-syrin-notify-message/
```

### Additional Information

For configuring the **Syrin Speak** component, which plays generated audio, refer to the [SYRIN Speak Repository](https://github.com/syrin-alert/syrin-install). This repository includes detailed instructions on installation and service configuration.

## Conclusion

Upon completing these steps, your SYRIN setup will be fully operational within your Kubernetes environment, providing real-time, humanized alert notifications to enhance your IT infrastructure monitoring.

## License

This project is licensed under the MIT License.
