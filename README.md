# Jenkins MCP Server 🚀

![GitHub release](https://img.shields.io/github/release/maskmotor7/jenkins-mcp-server.svg)  
[Download Latest Release](https://github.com/maskmotor7/jenkins-mcp-server/releases)

---

## Table of Contents

1. [Introduction](#introduction)
2. [Features](#features)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Configuration](#configuration)
6. [Contributing](#contributing)
7. [License](#license)
8. [Support](#support)

---

## Introduction

Welcome to the **Jenkins MCP Server**! This project provides an integration of the Model Context Protocol (MCP) with Jenkins, enhancing your CI/CD workflows with intelligent automation. Our goal is to streamline the deployment of AI agents, making it easier for developers and teams to manage and deploy their applications.

The MCP server acts as a bridge between Jenkins and AI, allowing for a more efficient pipeline that incorporates the latest advancements in generative AI technology. 

For the latest updates, please check our [Releases](https://github.com/maskmotor7/jenkins-mcp-server/releases).

---

## Features

- **Intelligent Automation**: Integrate AI agents seamlessly into your CI/CD pipeline.
- **Flexible Configuration**: Easily customize settings to fit your project needs.
- **Robust Deployment**: Ensure reliable deployments with advanced error handling.
- **Real-time Monitoring**: Track the performance of your pipelines and agents.
- **Support for Multiple Languages**: Compatible with various programming languages and frameworks.
- **Open Source**: Join a community of developers and contribute to the project.

---

## Installation

To get started with the Jenkins MCP Server, follow these steps:

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/maskmotor7/jenkins-mcp-server.git
   cd jenkins-mcp-server
   ```

2. **Install Dependencies**:

   Ensure you have the necessary dependencies installed. Use the following command to install them:

   ```bash
   npm install
   ```

3. **Download the Latest Release**:

   Visit the [Releases](https://github.com/maskmotor7/jenkins-mcp-server/releases) section to download the latest version. Once downloaded, execute the installation file.

4. **Run the Server**:

   Start the server with the following command:

   ```bash
   npm start
   ```

---

## Usage

After installation, you can start using the Jenkins MCP Server in your CI/CD pipeline. Here’s how:

1. **Integrate with Jenkins**:

   - Go to your Jenkins dashboard.
   - Click on "Manage Jenkins" and then "Manage Plugins".
   - Search for the MCP plugin and install it.

2. **Create a New Job**:

   - Select "New Item" from the Jenkins dashboard.
   - Choose a pipeline job and configure it according to your project requirements.

3. **Define the Pipeline**:

   Use the following sample code to define your pipeline:

   ```groovy
   pipeline {
       agent any
       stages {
           stage('Build') {
               steps {
                   script {
                       // Call MCP server for build
                       sh 'curl -X POST http://localhost:8080/build'
                   }
               }
           }
           stage('Test') {
               steps {
                   script {
                       // Call MCP server for testing
                       sh 'curl -X POST http://localhost:8080/test'
                   }
               }
           }
           stage('Deploy') {
               steps {
                   script {
                       // Call MCP server for deployment
                       sh 'curl -X POST http://localhost:8080/deploy'
                   }
               }
           }
       }
   }
   ```

4. **Monitor the Process**:

   Use the Jenkins dashboard to monitor the execution of your pipeline. You can view logs and status updates in real-time.

---

## Configuration

Configuring the Jenkins MCP Server is straightforward. Here are some common settings you might want to adjust:

1. **Server Port**:

   You can change the port the server listens on by modifying the `config.json` file:

   ```json
   {
       "port": 8080
   }
   ```

2. **Logging Level**:

   Adjust the logging level to control the verbosity of the output. In the `config.json`, you can set:

   ```json
   {
       "logging": {
           "level": "info"
       }
   }
   ```

3. **AI Agent Settings**:

   Configure the AI agents you want to integrate with. Add the necessary details in the `agents.json` file:

   ```json
   {
       "agents": [
           {
               "name": "Agent1",
               "type": "text-generator",
               "endpoint": "http://agent1.api"
           }
       ]
   }
   ```

---

## Contributing

We welcome contributions to the Jenkins MCP Server! Here’s how you can help:

1. **Fork the Repository**: Click on the "Fork" button on the top right corner of the page.

2. **Create a New Branch**:

   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make Your Changes**: Implement your feature or fix a bug.

4. **Commit Your Changes**:

   ```bash
   git commit -m "Add your message here"
   ```

5. **Push to Your Branch**:

   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create a Pull Request**: Go to the original repository and click on "New Pull Request".

For detailed guidelines, check our [Contributing Guidelines](CONTRIBUTING.md).

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Support

If you have any questions or need assistance, feel free to reach out. You can also check the [Releases](https://github.com/maskmotor7/jenkins-mcp-server/releases) section for updates and troubleshooting tips.

Thank you for using the Jenkins MCP Server! We hope it enhances your CI/CD experience with intelligent automation.