# Jenkins MCP Server

A Model Context Protocol (MCP) server for integrating Jenkins automation server with GenAI applications, enabling intelligent CI/CD pipeline management.

## Features

- **Comprehensive Jenkins API Coverage**:
  - Job Management: Create, configure, build, delete jobs
  - Pipeline Operations: Execute, monitor, and manage pipelines
  - Build Management: Trigger builds, get status, retrieve artifacts
  - Queue Management: Monitor build queue, cancel queued items
  - Node/Agent Management: List nodes, get node status, manage agents
  - Plugin Management: List, install, update plugins
  - Credentials Management: Securely manage Jenkins credentials
  - Folder/Organization: Support for folders and organization folders
  - Blue Ocean API: Modern pipeline visualization and management
  - Webhook Integration: GitHub, GitLab, Bitbucket webhooks

- **Authentication Methods**:
  - API Token authentication
  - Username/Password (not recommended)
  - OAuth 2.0 support
  - LDAP/AD integration

- **Enterprise Features**:
  - Multi-master Jenkins support
  - Role-based access control (RBAC)
  - Audit logging
  - Performance monitoring
  - Batch operations for efficiency

## Installation

```bash
pip install jenkins-mcp-server
```

Or install from source:

```bash
git clone https://github.com/asklokesh/jenkins-mcp-server.git
cd jenkins-mcp-server
pip install -e .
```

## Configuration

Create a `.env` file or set environment variables:

```env
# Jenkins Connection
JENKINS_URL=https://jenkins.example.com
JENKINS_USERNAME=your_username
JENKINS_API_TOKEN=your_api_token

# Optional Settings
JENKINS_VERIFY_SSL=true
JENKINS_TIMEOUT=30
JENKINS_MAX_RETRIES=3

# Multi-Master Support (optional)
JENKINS_PROD_URL=https://jenkins-prod.example.com
JENKINS_PROD_USERNAME=prod_user
JENKINS_PROD_API_TOKEN=prod_token

JENKINS_DEV_URL=https://jenkins-dev.example.com
JENKINS_DEV_USERNAME=dev_user
JENKINS_DEV_API_TOKEN=dev_token
```

## Quick Start

### Basic Usage

```python
from jenkins_mcp import JenkinsMCPServer

# Initialize the server
server = JenkinsMCPServer()

# Start the server
server.start()
```

### Claude Desktop Configuration

Add to your Claude Desktop config:

```json
{
  "mcpServers": {
    "jenkins": {
      "command": "python",
      "args": ["-m", "jenkins_mcp.server"],
      "env": {
        "JENKINS_URL": "https://jenkins.example.com",
        "JENKINS_USERNAME": "your_username",
        "JENKINS_API_TOKEN": "your_api_token"
      }
    }
  }
}
```

## Available Tools

### Job Management

#### List Jobs
```python
{
  "tool": "jenkins_list_jobs",
  "arguments": {
    "folder": "optional/folder/path",
    "view": "optional_view_name"
  }
}
```

#### Create Job
```python
{
  "tool": "jenkins_create_job",
  "arguments": {
    "job_name": "my-new-job",
    "config_xml": "<project>...</project>",
    "folder": "optional/folder/path"
  }
}
```

#### Get Job Config
```python
{
  "tool": "jenkins_get_job_config",
  "arguments": {
    "job_name": "my-job",
    "folder": "optional/folder/path"
  }
}
```

### Build Operations

#### Trigger Build
```python
{
  "tool": "jenkins_build_job",
  "arguments": {
    "job_name": "my-job",
    "parameters": {
      "BRANCH": "main",
      "ENVIRONMENT": "staging"
    },
    "folder": "optional/folder/path"
  }
}
```

#### Get Build Status
```python
{
  "tool": "jenkins_get_build_status",
  "arguments": {
    "job_name": "my-job",
    "build_number": 123
  }
}
```

#### Get Build Logs
```python
{
  "tool": "jenkins_get_build_logs",
  "arguments": {
    "job_name": "my-job",
    "build_number": 123,
    "start": 0,
    "max_lines": 1000
  }
}
```

### Pipeline Operations

#### Execute Pipeline
```python
{
  "tool": "jenkins_execute_pipeline",
  "arguments": {
    "job_name": "my-pipeline",
    "pipeline_script": "pipeline { agent any; stages { ... } }",
    "parameters": {}
  }
}
```

#### Get Pipeline Status
```python
{
  "tool": "jenkins_get_pipeline_status",
  "arguments": {
    "job_name": "my-pipeline",
    "build_number": 123
  }
}
```

### Queue Management

#### List Queue Items
```python
{
  "tool": "jenkins_list_queue",
  "arguments": {}
}
```

#### Cancel Queue Item
```python
{
  "tool": "jenkins_cancel_queue_item",
  "arguments": {
    "queue_id": 456
  }
}
```

### Node Management

#### List Nodes
```python
{
  "tool": "jenkins_list_nodes",
  "arguments": {}
}
```

#### Get Node Info
```python
{
  "tool": "jenkins_get_node_info",
  "arguments": {
    "node_name": "agent-01"
  }
}
```

### Plugin Management

#### List Plugins
```python
{
  "tool": "jenkins_list_plugins",
  "arguments": {
    "active_only": true
  }
}
```

#### Install Plugin
```python
{
  "tool": "jenkins_install_plugin",
  "arguments": {
    "plugin_name": "git",
    "version": "latest"
  }
}
```

### Credentials Management

#### Create Credentials
```python
{
  "tool": "jenkins_create_credentials",
  "arguments": {
    "credential_id": "github-token",
    "credential_type": "secret_text",
    "secret": "ghp_xxxxx",
    "description": "GitHub API Token",
    "domain": "global"
  }
}
```

## Advanced Configuration

### Multi-Master Support

```python
from jenkins_mcp import JenkinsMCPServer, JenkinsConfig

# Configure multiple Jenkins masters
masters = {
    "production": JenkinsConfig(
        url="https://jenkins-prod.example.com",
        username="prod_user",
        api_token="prod_token"
    ),
    "development": JenkinsConfig(
        url="https://jenkins-dev.example.com",
        username="dev_user",
        api_token="dev_token"
    ),
    "staging": JenkinsConfig(
        url="https://jenkins-staging.example.com",
        username="staging_user",
        api_token="staging_token"
    )
}

server = JenkinsMCPServer(masters=masters, default_master="production")
```

### Pipeline as Code

```python
# Define reusable pipeline templates
pipeline_templates = {
    "standard_build": """
        pipeline {
            agent any
            stages {
                stage('Build') {
                    steps {
                        sh 'make build'
                    }
                }
                stage('Test') {
                    steps {
                        sh 'make test'
                    }
                }
                stage('Deploy') {
                    when {
                        branch 'main'
                    }
                    steps {
                        sh 'make deploy'
                    }
                }
            }
        }
    """
}

server = JenkinsMCPServer(pipeline_templates=pipeline_templates)
```

### Webhook Configuration

```python
from jenkins_mcp import JenkinsMCPServer, WebhookConfig

webhook_config = WebhookConfig(
    enabled=True,
    port=8080,
    secret="webhook_secret",
    ssl_cert="/path/to/cert.pem",
    ssl_key="/path/to/key.pem"
)

server = JenkinsMCPServer(webhook_config=webhook_config)
```

## Integration Examples

See the `examples/` directory for complete integration examples:

- `basic_operations.py` - Common Jenkins operations
- `pipeline_management.py` - Pipeline creation and management
- `multi_master.py` - Managing multiple Jenkins instances
- `gitops_integration.py` - GitOps workflow with Jenkins
- `genai_automation.py` - AI-powered pipeline generation
- `monitoring_dashboard.py` - Jenkins monitoring and alerting

## Security Best Practices

1. **Use API tokens** instead of passwords
2. **Enable HTTPS** for all connections
3. **Implement RBAC** with minimal permissions
4. **Rotate API tokens** regularly
5. **Use Jenkins credentials** for sensitive data
6. **Enable audit logging** for compliance
7. **Restrict network access** to Jenkins

## Error Handling

The server provides detailed error information:

```python
try:
    result = server.execute_tool("jenkins_build_job", {
        "job_name": "my-job"
    })
except JenkinsError as e:
    print(f"Jenkins error: {e.error_code} - {e.message}")
    if e.status_code == 404:
        print("Job not found")
    elif e.status_code == 403:
        print("Permission denied")
```

## Performance Optimization

1. **Use batch operations** for multiple jobs
2. **Enable caching** for job configurations
3. **Implement pagination** for large result sets
4. **Use lightweight API calls** when possible
5. **Monitor API rate limits**

## Troubleshooting

### Common Issues

1. **Authentication failures**
   - Verify API token is valid
   - Check user permissions
   - Ensure CSRF protection is handled

2. **Connection timeouts**
   - Increase timeout values
   - Check network connectivity
   - Verify Jenkins URL

3. **Permission errors**
   - Verify user has required permissions
   - Check folder-level permissions
   - Review security realm settings

## Contributing

Contributions are welcome! Please read our contributing guidelines and submit pull requests.

## License

MIT License - see LICENSE file for details