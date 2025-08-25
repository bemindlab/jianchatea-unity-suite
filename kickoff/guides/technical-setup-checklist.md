# Technical Setup Checklist - 2026 AI Development Stack
## Jian Cha Tea Unity Suite - Phase 1 Implementation

### Overview

This comprehensive technical setup checklist ensures all team members have a consistent, AI-native development environment optimized for 2026's mature AI development ecosystem. The setup prioritizes AI tool integration, cloud-native development, and enterprise security standards.

**Estimated Setup Time**: 4-6 hours  
**Prerequisites**: Administrative access to development machine  
**Support**: Available via Slack #technical-setup channel

---

## Pre-Setup Requirements Verification

### Hardware Requirements Validation

#### Development Machine Specifications
```bash
# Run this script to verify hardware compatibility
#!/bin/bash
echo "=== Hardware Compatibility Check ==="

# Check OS version
if [[ "$OSTYPE" == "darwin"* ]]; then
    echo "✓ macOS detected"
    sw_vers
    if [[ $(sysctl -n machdep.cpu.brand_string) == *"Apple"* ]]; then
        echo "✓ Apple Silicon detected - Optimal for AI tools"
    else
        echo "⚠ Intel Mac - AI tools will work but slower"
    fi
elif [[ "$OSTYPE" == "linux"* ]]; then
    echo "✓ Linux detected"
    lsb_release -a
else
    echo "❌ Unsupported OS - Please use macOS or Linux"
    exit 1
fi

# Check RAM
RAM=$(free -g 2>/dev/null | awk '/^Mem:/ {print $2}' || sysctl -n hw.memsize | awk '{print int($0/1024/1024/1024)}')
if [ "$RAM" -ge 16 ]; then
    echo "✓ RAM: ${RAM}GB (minimum 16GB met)"
else
    echo "❌ RAM: ${RAM}GB (minimum 16GB required for AI tools)"
    exit 1
fi

# Check available storage
STORAGE=$(df -h / | awk 'NR==2 {print $4}' | sed 's/G.*//')
if [ "$STORAGE" -ge 50 ]; then
    echo "✓ Available Storage: ${STORAGE}GB (minimum 50GB met)"
else
    echo "❌ Available Storage: ${STORAGE}GB (minimum 50GB required)"
    exit 1
fi

# Check internet speed
echo "Testing internet speed for AI tool performance..."
curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python3 - --simple
```

**Checklist Items**:
- [ ] **Operating System**: macOS 14+ or Ubuntu 22.04+ LTS
- [ ] **CPU**: Apple Silicon M2/M3 (preferred) or Intel i7/AMD Ryzen 7+
- [ ] **RAM**: Minimum 16GB (32GB recommended for AI workloads)
- [ ] **Storage**: 100GB+ available SSD space
- [ ] **Internet**: 100+ Mbps for seamless AI tool operation
- [ ] **External Monitors**: Dual 4K monitors for enhanced productivity
- [ ] **Peripherals**: High-quality webcam and headset for remote collaboration

### Network and Security Prerequisites

#### Corporate Network Access
- [ ] **VPN Access**: Corporate VPN client installed and configured
- [ ] **Firewall Whitelist**: AI tool domains added to firewall exceptions
- [ ] **Certificate Installation**: Corporate root certificates installed
- [ ] **Proxy Configuration**: Corporate proxy settings configured (if applicable)

#### Security Requirements
- [ ] **Antivirus**: Corporate antivirus installed and updated
- [ ] **Encryption**: Full disk encryption enabled (FileVault/LUKS)
- [ ] **Password Manager**: Corporate password manager installed
- [ ] **Two-Factor Authentication**: Authenticator app configured

---

## Phase 1: Core Development Environment

### 1.1 Package Managers and Foundation Tools

#### macOS Setup (Using Homebrew)
```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Update Homebrew and install essential tools
brew update
brew upgrade

# Core development tools
brew install git
brew install curl
brew install wget
brew install jq
brew install tree
brew install htop
brew install unzip

# Version managers
brew install nvm        # Node.js version manager
brew install pyenv      # Python version manager  
brew install rbenv      # Ruby version manager
brew install gvm        # Go version manager

# Database tools
brew install postgresql@15
brew install redis
brew install mysql-client

# Cloud and container tools
brew install docker
brew install docker-compose
brew install kubectl
brew install helm
brew install terraform
brew install aws-cli
brew install azure-cli
brew install google-cloud-sdk

echo "✓ Core development tools installed"
```

#### Ubuntu Setup (Using APT)
```bash
# Update package lists
sudo apt update && sudo apt upgrade -y

# Core development tools
sudo apt install -y \
    git \
    curl \
    wget \
    jq \
    tree \
    htop \
    unzip \
    build-essential \
    software-properties-common \
    apt-transport-https \
    ca-certificates \
    gnupg \
    lsb-release

# Install Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Add user to docker group
sudo usermod -aG docker $USER

# Install Kubernetes tools
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install Terraform
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

echo "✓ Core development tools installed"
```

**Verification**:
```bash
# Verify installations
git --version
docker --version
kubectl version --client
terraform --version
aws --version

echo "✓ All core tools verified"
```

### 1.2 Programming Language Environments

#### Node.js Setup (Latest LTS + Current)
```bash
# Install and configure Node.js versions
nvm install --lts
nvm install node
nvm use --lts
nvm alias default lts/*

# Verify installation
node --version  # Should show LTS version (v20.x)
npm --version

# Install global packages for development
npm install -g \
    typescript \
    ts-node \
    @types/node \
    eslint \
    prettier \
    jest \
    @vue/cli \
    @angular/cli \
    create-react-app \
    next \
    serverless \
    pm2

echo "✓ Node.js environment configured"
```

#### Python Setup (3.11 + 3.12)
```bash
# Install Python versions
pyenv install 3.11.7
pyenv install 3.12.1
pyenv global 3.11.7

# Verify installation
python --version  # Should show 3.11.7
pip --version

# Create virtual environment for AI development
python -m venv ~/venvs/jianchatea-ai
source ~/venvs/jianchatea-ai/bin/activate

# Install essential Python packages
pip install --upgrade pip
pip install \
    requests \
    numpy \
    pandas \
    scikit-learn \
    tensorflow \
    torch \
    transformers \
    langchain \
    openai \
    anthropic \
    fastapi \
    uvicorn \
    pytest \
    black \
    flake8 \
    mypy

echo "✓ Python environment configured"
```

#### Java Setup (OpenJDK 17 + 21)
```bash
# Install OpenJDK versions
brew install openjdk@17
brew install openjdk@21

# Set JAVA_HOME for Java 17 (primary)
echo 'export JAVA_HOME=/opt/homebrew/opt/openjdk@17' >> ~/.zshrc
echo 'export PATH="$JAVA_HOME/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Verify installation
java -version  # Should show OpenJDK 17
javac -version

# Install Maven and Gradle
brew install maven
brew install gradle

mvn --version
gradle --version

echo "✓ Java environment configured"
```

#### Go Setup (Latest Stable)
```bash
# Install Go
gvm install go1.21.6
gvm use go1.21.6 --default

# Verify installation
go version

# Set up Go workspace
mkdir -p ~/go/{bin,src,pkg}
echo 'export GOPATH=$HOME/go' >> ~/.zshrc
echo 'export PATH="$GOPATH/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Install useful Go tools
go install golang.org/x/tools/gopls@latest
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
go install github.com/gorilla/mux@latest

echo "✓ Go environment configured"
```

---

## Phase 2: AI Development Tools Integration

### 2.1 Primary AI Coding Assistants

#### GitHub Copilot X (Business License)
```bash
# Install GitHub CLI
brew install gh

# Authenticate with GitHub
gh auth login

# Install Copilot CLI extension
gh extension install github/gh-copilot

# Verify installation
gh copilot --help

# Configure Copilot settings
gh copilot config set editor=cursor
gh copilot config set suggestions=true
gh copilot config set autocompletions=true
gh copilot config set chat=true

echo "✓ GitHub Copilot X configured"
```

**VS Code Configuration**:
```json
{
  "github.copilot.enable": {
    "*": true,
    "yaml": true,
    "plaintext": false,
    "markdown": true
  },
  "github.copilot.advanced": {
    "length": 500,
    "temperature": 0.1,
    "top_p": 1,
    "enableAutoCompletions": true,
    "enableChatCompletion": true,
    "enableVoiceInteraction": true
  },
  "github.copilot.customization": {
    "businessDomain": "franchise-management",
    "codingStandards": "./docs/coding-standards.md",
    "architectureContext": "./docs/architecture/",
    "teamPreferences": {
      "framework": "react",
      "backend": "nodejs",
      "database": "postgresql",
      "cloud": "aws"
    }
  }
}
```

#### Cursor IDE (Professional License)
```bash
# Download and install Cursor IDE
if [[ "$OSTYPE" == "darwin"* ]]; then
    curl -L "https://download.todesktop.com/210303leazlircz/Cursor-0.19.3-universal.dmg" -o cursor.dmg
    hdiutil attach cursor.dmg
    cp -R "/Volumes/Cursor/Cursor.app" /Applications/
    hdiutil detach /Volumes/Cursor
    rm cursor.dmg
elif [[ "$OSTYPE" == "linux"* ]]; then
    curl -L "https://download.todesktop.com/210303leazlircz/Cursor-0.19.3.AppImage" -o cursor.AppImage
    chmod +x cursor.AppImage
    sudo mv cursor.AppImage /usr/local/bin/cursor
fi

echo "✓ Cursor IDE installed"
```

**Cursor Configuration** (`~/.cursor/config.json`):
```json
{
  "ai": {
    "provider": "claude-3.5-sonnet",
    "apiKey": "${ANTHROPIC_API_KEY}",
    "context": {
      "includeOpenFiles": true,
      "includeGitHistory": true,
      "includeDocumentation": true,
      "maxTokens": 200000,
      "includeTests": true
    },
    "features": {
      "autoComplete": true,
      "codeGeneration": true,
      "refactoring": true,
      "bugFix": true,
      "testGeneration": true,
      "documentation": true,
      "codeExplanation": true
    },
    "customPrompts": {
      "franchiseComponent": "Create a React TypeScript component for franchise management following our design system",
      "apiEndpoint": "Generate a secure REST API endpoint with validation and error handling",
      "testSuite": "Create comprehensive tests including unit, integration, and edge cases",
      "documentation": "Generate technical documentation with examples and usage instructions"
    }
  },
  "workflow": {
    "gitIntegration": true,
    "liveShare": true,
    "aiPairProgramming": true,
    "contextPreservation": true
  },
  "editor": {
    "theme": "dark",
    "fontSize": 14,
    "fontFamily": "JetBrains Mono",
    "tabSize": 2,
    "formatOnSave": true
  }
}
```

#### Claude AI API Integration
```bash
# Install Claude CLI tool
npm install -g @anthropic-ai/claude-cli

# Configure API key (use environment variable)
echo 'export ANTHROPIC_API_KEY="your-api-key-here"' >> ~/.zshrc
source ~/.zshrc

# Test Claude API
claude --version
claude chat "Hello, I'm setting up my development environment for the Jian Cha Tea project."

echo "✓ Claude AI API configured"
```

### 2.2 AI-Enhanced Development Tools

#### AI Code Review and Quality Tools
```bash
# Install Snyk for security scanning with AI
npm install -g snyk
snyk auth

# Install SonarQube CLI with AI enhancements
brew install sonar-scanner

# Install AI-powered linting tools
npm install -g \
    @typescript-eslint/parser \
    @typescript-eslint/eslint-plugin \
    eslint-plugin-ai-assisted \
    prettier-plugin-ai-format

# Install DeepCode (now part of Snyk)
code --install-extension snyk-security.snyk-vulnerability-scanner

echo "✓ AI code review tools configured"
```

#### AI Testing and Documentation Tools
```bash
# Install AI-powered testing tools
npm install -g \
    @testim/testim-cli \
    jest-ai-assistant \
    playwright-ai-helper

# Install documentation generation tools
npm install -g \
    @mintlify/scraping \
    typedoc \
    jsdoc \
    doctoc

# Install AI writing assistants
pip install \
    grammarly-api \
    openai-documentation-helper

echo "✓ AI testing and documentation tools configured"
```

---

## Phase 3: Cloud-Native Development Environment

### 3.1 Container and Orchestration Tools

#### Docker Desktop Configuration
```bash
# Start Docker Desktop
open -a Docker

# Configure Docker for development
cat > ~/.docker/daemon.json << EOF
{
  "experimental": false,
  "debug": false,
  "insecure-registries": [],
  "registry-mirrors": [],
  "storage-driver": "overlay2",
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "features": {
    "kubernetes": true,
    "vpnkit": true
  },
  "resources": {
    "cpus": 4,
    "memory": 8589934592
  }
}
EOF

# Restart Docker to apply configuration
killall Docker && open -a Docker

# Verify Docker installation
docker --version
docker-compose --version
docker system info

echo "✓ Docker configured for development"
```

#### Kubernetes Development Environment
```bash
# Enable Kubernetes in Docker Desktop
# This is done through Docker Desktop UI: Settings -> Kubernetes -> Enable

# Install additional Kubernetes tools
brew install k9s          # Kubernetes cluster management
brew install kubectx      # Context switching
brew install kustomize    # Configuration management
brew install skaffold     # Development workflow

# Install Helm
brew install helm

# Add essential Helm repositories
helm repo add stable https://charts.helm.sh/stable
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add jetstack https://charts.jetstack.io
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# Verify Kubernetes setup
kubectl cluster-info
kubectl get nodes

# Install local development namespace
kubectl create namespace jianchatea-dev
kubectl config set-context --current --namespace=jianchatea-dev

echo "✓ Kubernetes development environment ready"
```

### 3.2 Infrastructure as Code Setup

#### Terraform Configuration
```bash
# Verify Terraform installation
terraform --version

# Set up Terraform workspace
mkdir -p ~/terraform/jianchatea
cd ~/terraform/jianchatea

# Create Terraform configuration for local development
cat > main.tf << 'EOF'
terraform {
  required_version = ">= 1.6"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "~> 2.0"
    }
  }
}

provider "aws" {
  region = "ap-southeast-1"
}

provider "kubernetes" {
  config_path = "~/.kube/config"
}
EOF

# Initialize Terraform
terraform init

echo "✓ Terraform workspace initialized"
```

#### AWS CLI Configuration
```bash
# Configure AWS CLI
aws configure

# Set up development profile
aws configure set profile.jianchatea-dev.region ap-southeast-1
aws configure set profile.jianchatea-dev.output json

# Verify AWS access
aws sts get-caller-identity --profile jianchatea-dev

# Install additional AWS tools
brew install awscli
brew install eksctl
brew install aws-iam-authenticator

echo "✓ AWS CLI configured"
```

---

## Phase 4: IDE and Editor Configuration

### 4.1 Primary IDE Setup (Cursor)

#### Cursor Extensions and Configuration
```bash
# Launch Cursor and install essential extensions
cursor --install-extension ms-python.python
cursor --install-extension ms-vscode.vscode-typescript-next
cursor --install-extension bradlc.vscode-tailwindcss
cursor --install-extension ms-vscode.vscode-json
cursor --install-extension redhat.vscode-yaml
cursor --install-extension ms-kubernetes-tools.vscode-kubernetes-tools
cursor --install-extension ms-vscode.vscode-docker
cursor --install-extension hashicorp.terraform
cursor --install-extension github.copilot
cursor --install-extension github.copilot-chat
cursor --install-extension anthropic.claude-dev
cursor --install-extension tabnine.tabnine-vscode

echo "✓ Cursor extensions installed"
```

#### Cursor Workspace Settings
```json
{
  "editor.fontSize": 14,
  "editor.fontFamily": "JetBrains Mono, 'Courier New', monospace",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "editor.rulers": [80, 120],
  "editor.wordWrap": "wordWrapColumn",
  "editor.wordWrapColumn": 120,
  
  "typescript.preferences.importModuleSpecifier": "relative",
  "typescript.updateImportsOnFileMove.enabled": "always",
  
  "python.defaultInterpreterPath": "~/venvs/jianchatea-ai/bin/python",
  "python.formatting.provider": "black",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": true,
  
  "docker.defaultRegistryPath": "registry.gitlab.com/jianchatea",
  
  "kubernetes.outputFormat": "yaml",
  "kubernetes.defaultNamespace": "jianchatea-dev",
  
  "ai.cursor.model": "claude-3.5-sonnet",
  "ai.cursor.temperature": 0.1,
  "ai.cursor.maxTokens": 200000,
  "ai.cursor.contextWindow": 50000,
  
  "files.exclude": {
    "**/node_modules": true,
    "**/.git": true,
    "**/.DS_Store": true,
    "**/dist": true,
    "**/build": true,
    "**/__pycache__": true,
    "**/*.pyc": true
  }
}
```

### 4.2 VS Code (Secondary IDE) Setup

#### Essential VS Code Extensions
```bash
# Install VS Code extensions for backup IDE
code --install-extension ms-python.python
code --install-extension ms-vscode.vscode-typescript-next
code --install-extension ms-vscode.vscode-json
code --install-extension redhat.vscode-yaml
code --install-extension ms-kubernetes-tools.vscode-kubernetes-tools
code --install-extension ms-vscode.vscode-docker
code --install-extension hashicorp.terraform
code --install-extension github.copilot
code --install-extension github.copilot-chat
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-eslint

echo "✓ VS Code extensions installed"
```

---

## Phase 5: Database and Message Queue Setup

### 5.1 Local Development Databases

#### PostgreSQL Local Setup
```bash
# Start PostgreSQL service
brew services start postgresql@15

# Create development databases
createdb jianchatea_dev
createdb jianchatea_test

# Create development user
psql -d postgres -c "CREATE USER jianchatea_dev WITH PASSWORD 'dev_password';"
psql -d postgres -c "GRANT ALL PRIVILEGES ON DATABASE jianchatea_dev TO jianchatea_dev;"
psql -d postgres -c "GRANT ALL PRIVILEGES ON DATABASE jianchatea_test TO jianchatea_dev;"

# Verify database connection
psql -h localhost -U jianchatea_dev -d jianchatea_dev -c "SELECT version();"

echo "✓ PostgreSQL configured"
```

#### Redis Local Setup
```bash
# Start Redis service
brew services start redis

# Test Redis connection
redis-cli ping

# Configure Redis for development
cat > /opt/homebrew/etc/redis.conf << EOF
# Development configuration
port 6379
bind 127.0.0.1
save 900 1
save 300 10
save 60 10000
rdbcompression yes
dbfilename dump.rdb
dir /opt/homebrew/var/db/redis/
maxmemory 256mb
maxmemory-policy allkeys-lru
EOF

# Restart Redis with new configuration
brew services restart redis

echo "✓ Redis configured"
```

### 5.2 Message Queue Setup

#### Apache Kafka Local Development
```bash
# Using Docker Compose for Kafka development environment
mkdir -p ~/docker/kafka
cd ~/docker/kafka

cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:7.4.0
    hostname: zookeeper
    container_name: zookeeper
    ports:
      - "2181:2181"
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000

  kafka:
    image: confluentinc/cp-kafka:7.4.0
    hostname: kafka
    container_name: kafka
    depends_on:
      - zookeeper
    ports:
      - "29092:29092"
      - "9092:9092"
      - "9101:9101"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: 'zookeeper:2181'
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
      KAFKA_METRIC_REPORTERS: io.confluent.metrics.reporter.ConfluentMetricsReporter
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS: 0
      KAFKA_CONFLUENT_METRICS_REPORTER_BOOTSTRAP_SERVERS: kafka:29092
      KAFKA_CONFLUENT_METRICS_REPORTER_TOPIC_REPLICAS: 1
      KAFKA_CONFLUENT_METRICS_ENABLE: 'true'
      KAFKA_CONFLUENT_SUPPORT_CUSTOMER_ID: anonymous

  kafka-ui:
    image: provectuslabs/kafka-ui:latest
    container_name: kafka-ui
    depends_on:
      - kafka
    ports:
      - "8080:8080"
    environment:
      KAFKA_CLUSTERS_0_NAME: local
      KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS: kafka:29092
EOF

# Start Kafka development environment
docker-compose up -d

# Verify Kafka is running
docker-compose ps

# Install Kafka CLI tools
brew install kafka

echo "✓ Kafka development environment ready"
echo "Kafka UI available at: http://localhost:8080"
```

---

## Phase 6: Development Workflow Setup

### 6.1 Git Configuration and Workflow

#### Git Global Configuration
```bash
# Configure Git with your information
git config --global user.name "Your Name"
git config --global user.email "your.email@jianchatea.com"

# Configure Git for better development workflow
git config --global init.defaultBranch main
git config --global pull.rebase true
git config --global rebase.autoStash true
git config --global push.default simple
git config --global core.autocrlf input
git config --global core.editor cursor
git config --global diff.tool vscode
git config --global merge.tool vscode

# Configure Git aliases for productivity
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.visual '!gitk'
git config --global alias.graph 'log --oneline --graph --all'

# Configure GPG signing (if required)
# git config --global user.signingkey YOUR_GPG_KEY
# git config --global commit.gpgsign true

echo "✓ Git configured"
```

#### SSH Key Setup for Git
```bash
# Generate SSH key for Git operations
ssh-keygen -t ed25519 -C "your.email@jianchatea.com" -f ~/.ssh/id_ed25519_jianchatea

# Add SSH key to agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_jianchatea

# Display public key for adding to GitLab/GitHub
cat ~/.ssh/id_ed25519_jianchatea.pub

echo "✓ SSH key generated"
echo "Please add the above public key to your GitLab/GitHub account"
```

### 6.2 Project Repository Setup

#### Clone Core Repositories
```bash
# Create development workspace
mkdir -p ~/workspace/jianchatea
cd ~/workspace/jianchatea

# Clone core repositories
git clone git@gitlab.com:jianchatea/platform-services.git
git clone git@gitlab.com:jianchatea/frontend-applications.git
git clone git@gitlab.com:jianchatea/mobile-applications.git
git clone git@gitlab.com:jianchatea/infrastructure.git
git clone git@gitlab.com:jianchatea/documentation.git

# Verify repository access
for repo in */; do
    echo "Checking $repo"
    cd "$repo"
    git status
    cd ..
done

echo "✓ Repositories cloned and accessible"
```

#### Development Branch Setup
```bash
# Set up development branches for each repository
repositories=("platform-services" "frontend-applications" "mobile-applications" "infrastructure")

for repo in "${repositories[@]}"; do
    cd "$repo"
    git checkout main
    git pull origin main
    git checkout -b develop
    git push -u origin develop
    cd ..
done

echo "✓ Development branches created"
```

---

## Phase 7: AI Development Workflow Validation

### 7.1 AI Tool Integration Testing

#### GitHub Copilot Validation
```typescript
// Test GitHub Copilot functionality
// Type this comment and wait for Copilot suggestions:
// Create a TypeScript function that validates franchise application data

// Expected: Copilot should suggest a function implementation
function validateFranchiseApplication(application: any): boolean {
    // Copilot should suggest validation logic
}

// Test Copilot Chat
// Open Copilot Chat and ask: "Explain how to implement user authentication in a Node.js Express app"
```

#### Cursor AI Testing
```python
# Test Cursor AI functionality in a Python file
# Type this comment and use Cursor's AI completion:
# Create a Python class for handling franchise data processing

# Expected: Cursor should suggest a comprehensive class implementation
class FranchiseDataProcessor:
    # Cursor should suggest methods and implementation
    pass

# Test Cursor AI chat
# Use Cmd+L (Mac) or Ctrl+L (Windows/Linux) to open AI chat
# Ask: "Help me create a data validation function for franchise applications"
```

#### Claude API Testing
```bash
# Test Claude API functionality
claude chat "I'm working on a franchise management system. Can you help me design a database schema for storing franchise information, including locations, owners, financial data, and operational metrics?"

# Expected: Detailed database schema suggestions
```

### 7.2 Development Workflow Validation

#### Full Stack Development Test
```bash
# Create a test project to validate the entire development stack
mkdir ~/test-project
cd ~/test-project

# Initialize a new Node.js project
npm init -y

# Install dependencies
npm install express typescript ts-node @types/express @types/node

# Create a simple Express server with AI assistance
cat > src/index.ts << 'EOF'
// Ask AI: "Create an Express.js server with TypeScript that handles franchise data"
import express from 'express';

const app = express();
const port = 3000;

// AI should suggest middleware and routes
app.use(express.json());

// AI should suggest franchise-related endpoints
app.get('/api/franchises', (req, res) => {
    // AI implementation suggestion
});

app.listen(port, () => {
    console.log(`Server running at http://localhost:${port}`);
});
EOF

# Test TypeScript compilation
npx tsc --init
npx tsc src/index.ts

# Test Docker containerization
cat > Dockerfile << 'EOF'
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
EOF

# Build and test Docker container
docker build -t test-app .
docker run -d -p 3000:3000 test-app

# Verify application is running
curl http://localhost:3000/api/franchises

echo "✓ Full development stack validated"
```

---

## Phase 8: Team Collaboration Setup

### 8.1 Communication Tools Configuration

#### Slack Integration
```bash
# Install Slack desktop app
brew install --cask slack

# Configure Slack with these channels:
# - #general: General team communication
# - #development: Development-specific discussions
# - #ai-assistance: AI tool help and sharing
# - #deployment: Deployment notifications
# - #alerts: System alerts and monitoring
```

#### Video Conferencing Setup
```bash
# Install video conferencing tools
brew install --cask zoom
brew install --cask google-chrome  # For Google Meet

# Test camera and microphone
# Zoom: zoom.us/test
# Google Meet: meet.google.com/test
```

### 8.2 Documentation and Knowledge Sharing

#### Notion Setup
```bash
# Install Notion desktop app
brew install --cask notion

# Key Notion pages to bookmark:
# - Team Handbook
# - AI Development Guidelines
# - Project Documentation
# - Meeting Notes
# - Knowledge Base
```

---

## Final Verification and Validation

### Comprehensive System Check
```bash
#!/bin/bash
echo "=== Jian Cha Tea Development Environment Validation ==="

# Check development tools
tools=("git" "docker" "kubectl" "terraform" "aws" "node" "python" "java" "go")
for tool in "${tools[@]}"; do
    if command -v "$tool" &> /dev/null; then
        echo "✓ $tool: $(command -v "$tool")"
    else
        echo "❌ $tool: Not found"
    fi
done

# Check AI tools
echo "=== AI Tools Check ==="
if command -v gh &> /dev/null; then
    echo "✓ GitHub CLI: $(gh --version | head -1)"
else
    echo "❌ GitHub CLI: Not found"
fi

if [ -f "/Applications/Cursor.app/Contents/MacOS/Cursor" ]; then
    echo "✓ Cursor IDE: Installed"
else
    echo "❌ Cursor IDE: Not found"
fi

if command -v claude &> /dev/null; then
    echo "✓ Claude CLI: $(claude --version)"
else
    echo "❌ Claude CLI: Not found"
fi

# Check services
echo "=== Services Check ==="
if brew services list | grep postgresql@15 | grep -q started; then
    echo "✓ PostgreSQL: Running"
else
    echo "❌ PostgreSQL: Not running"
fi

if brew services list | grep redis | grep -q started; then
    echo "✓ Redis: Running"
else
    echo "❌ Redis: Not running"
fi

if docker ps | grep -q kafka; then
    echo "✓ Kafka: Running"
else
    echo "❌ Kafka: Not running"
fi

# Check Docker
if docker info &> /dev/null; then
    echo "✓ Docker: Running"
else
    echo "❌ Docker: Not running"
fi

# Check Kubernetes
if kubectl cluster-info &> /dev/null; then
    echo "✓ Kubernetes: Connected"
else
    echo "❌ Kubernetes: Not connected"
fi

echo "=== Setup Validation Complete ==="
```

### Performance Benchmark
```bash
# Test AI tool response times
echo "Testing AI tool performance..."

# GitHub Copilot response time test
time gh copilot suggest "create a simple hello world function"

# Cursor AI response time test (manual verification needed)
echo "Please test Cursor AI response time manually"

# Claude API response time test
time claude chat "Hello, please respond with a simple greeting"

echo "✓ AI tool performance testing complete"
```

---

## Troubleshooting Guide

### Common Issues and Solutions

#### 1. GitHub Copilot Not Working
```bash
# Check Copilot authentication
gh auth status

# Re-authenticate if needed
gh auth logout
gh auth login

# Check Copilot subscription
gh copilot status

# Restart VS Code/Cursor after authentication
```

#### 2. Docker Issues
```bash
# Reset Docker Desktop
docker system prune -a --volumes

# Restart Docker Desktop
killall Docker && open -a Docker

# Check Docker resource allocation
docker system df
docker system info
```

#### 3. Kubernetes Connection Issues
```bash
# Reset Kubernetes configuration
kubectl config current-context
kubectl config get-contexts

# Switch to correct context
kubectl config use-context docker-desktop

# Verify cluster access
kubectl cluster-info
kubectl get nodes
```

#### 4. AI Tool Performance Issues
```bash
# Check internet connection speed
speedtest-cli

# Verify API keys are set correctly
echo $ANTHROPIC_API_KEY | wc -c  # Should be > 50 characters

# Clear AI tool cache
rm -rf ~/.cursor/cache/
rm -rf ~/.vscode/extensions/github.copilot-*/
```

#### 5. Database Connection Issues
```bash
# Check PostgreSQL status
brew services list | grep postgresql

# Restart PostgreSQL
brew services restart postgresql@15

# Check Redis status
redis-cli ping

# Restart Redis if needed
brew services restart redis
```

---

## Security Checklist

### Final Security Validation
- [ ] **Full Disk Encryption**: FileVault (macOS) or LUKS (Linux) enabled
- [ ] **Firewall**: System firewall enabled with appropriate rules
- [ ] **VPN**: Corporate VPN configured and functional
- [ ] **Antivirus**: Corporate antivirus installed and updated
- [ ] **Password Manager**: Corporate password manager configured
- [ ] **2FA**: Two-factor authentication enabled for all accounts
- [ ] **SSH Keys**: Secure SSH keys generated and configured
- [ ] **API Keys**: All API keys stored securely (environment variables)
- [ ] **Certificates**: Corporate certificates installed
- [ ] **Network**: Secure network connections validated

### Development Security Best Practices
- [ ] **Code Scanning**: Snyk and SonarQube configured
- [ ] **Secrets Management**: No secrets in code repositories
- [ ] **Container Security**: Docker image scanning enabled
- [ ] **Dependency Management**: Regular dependency updates scheduled
- [ ] **Access Controls**: Principle of least privilege applied
- [ ] **Audit Logging**: Development activity logging enabled
- [ ] **Backup Strategy**: Code and configuration backup procedures
- [ ] **Incident Response**: Security incident response procedures reviewed

---

## Completion Certification

### Setup Completion Checklist
- [ ] **Hardware**: All hardware requirements met and validated
- [ ] **Software**: All development tools installed and configured
- [ ] **AI Tools**: All AI development tools configured and tested
- [ ] **Cloud Access**: AWS, Azure, and GCP access configured
- [ ] **Repositories**: All project repositories cloned and accessible
- [ ] **Databases**: Local development databases operational
- [ ] **Message Queues**: Kafka development environment running
- [ ] **Container Platform**: Docker and Kubernetes operational
- [ ] **IDE Configuration**: Primary and secondary IDEs configured
- [ ] **Team Tools**: Communication and collaboration tools setup
- [ ] **Security**: All security requirements implemented
- [ ] **Validation**: Comprehensive system validation completed

### Sign-off
**Developer Name**: ________________________  
**Setup Completion Date**: ________________  
**Mentor/Lead Approval**: __________________  
**Date**: ________________

### Next Steps
After completing this technical setup:
1. **Schedule onboarding meeting** with technical lead
2. **Complete AI development training** (Week 1 of onboarding)
3. **Join team standup meetings** and sprint planning
4. **Start contributing** to Sprint 1 development tasks
5. **Begin continuous learning** program for AI development mastery

**Support Contacts**:
- **Technical Issues**: #technical-support on Slack
- **AI Tools Help**: #ai-assistance on Slack  
- **Security Questions**: security@jianchatea.com
- **General Questions**: your-mentor@jianchatea.com

Welcome to the AI-native development revolution! 🚀