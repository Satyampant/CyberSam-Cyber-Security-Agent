# Cyber-Security Agent

A Python code security analyzer powered by OpenAI Agents and Semgrep MCP server. This tool provides comprehensive security analysis for Python code, identifying vulnerabilities and recommending fixes.

## Features

- **AI-Powered Analysis**: Uses OpenAI GPT-4.1-mini with Semgrep MCP server for intelligent security scanning
- **Comprehensive Reporting**: Generates detailed security reports with CVSS scores and severity levels
- **Interactive Web Interface**: Modern Next.js frontend for easy code upload and analysis
- **Structured Output**: Returns security issues with descriptions, vulnerable code snippets, and recommended fixes
- **Cloud-Ready**: Deployable to Azure Container Apps or Google Cloud Run

## Architecture

- **Backend**: FastAPI server with OpenAI Agents integration
- **Frontend**: Next.js 15 with React 19 and Tailwind CSS 4
- **Security Scanner**: Semgrep MCP server for static analysis
- **Deployment**: Docker containerized with Terraform IaC

## Prerequisites

- Python 3.12+
- Node.js 20+
- Docker (for containerized deployment)
- OpenAI API key
- Semgrep App Token (optional)

## Local Development

### Backend Setup

```bash
cd backend
pip install uv
uv sync
```

Create a `.env` file:

```env
OPENAI_API_KEY=your_openai_api_key
SEMGREP_APP_TOKEN=your_semgrep_token
```

Run the backend:

```bash
uv run uvicorn server:app --reload --port 8000
```

### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The application will be available at `http://localhost:3000`

## Docker Deployment

Build and run with Docker:

```bash
docker build -t cyber-analyzer .
docker run -p 8000:8000 \
  -e OPENAI_API_KEY=your_key \
  -e SEMGREP_APP_TOKEN=your_token \
  cyber-analyzer
```

## Cloud Deployment

### Azure Container Apps

```bash
cd terraform/azure
terraform init
terraform apply \
  -var="openai_api_key=your_key" \
  -var="semgrep_app_token=your_token" \
  -var="resource_group_name=your_rg"
```

### Google Cloud Run

```bash
cd terraform/gcp
terraform init
terraform apply \
  -var="project_id=your_project" \
  -var="openai_api_key=your_key" \
  -var="semgrep_app_token=your_token"
```

## API Usage

### Analyze Code Endpoint

```bash
POST /api/analyze
Content-Type: application/json

{
  "code": "your_python_code_here"
}
```

### Response Format

```json
{
  "summary": "Analysis summary with issue counts",
  "issues": [
    {
      "title": "SQL Injection Vulnerability",
      "description": "Detailed description of the issue",
      "code": "Vulnerable code snippet",
      "fix": "Recommended fix",
      "cvss_score": 9.1,
      "severity": "critical"
    }
  ]
}
```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `OPENAI_API_KEY` | OpenAI API key for GPT-4 access | Yes |
| `SEMGREP_APP_TOKEN` | Semgrep app token for enhanced scanning | No |
| `ENVIRONMENT` | Set to `production` for cloud deployment | No |
| `PYTHONUNBUFFERED` | Set to `1` for real-time logging | No |

## Project Structure

```
.
├── backend/
│   ├── server.py           # FastAPI server
│   ├── context.py          # Security analysis prompts
│   ├── mcp_servers.py      # Semgrep MCP configuration
│   └── pyproject.toml      # Python dependencies
├── frontend/
│   ├── src/
│   │   ├── app/            # Next.js app router
│   │   ├── components/     # React components
│   │   └── types/          # TypeScript types
│   └── package.json        # Node dependencies
├── terraform/
│   ├── azure/              # Azure deployment
│   └── gcp/                # GCP deployment
└── Dockerfile              # Container configuration
```

## Security Analysis Features

The analyzer identifies:

- **Critical Issues**: SQL injection, command injection, insecure deserialization
- **High Severity**: Hardcoded secrets, weak cryptography, authentication bypasses
- **Medium Severity**: Input validation issues, insecure configurations
- **Low Severity**: Code quality issues, minor security concerns

Each issue includes:
- Clear title and description
- CVSS score (0.0-10.0)
- Severity classification
- Vulnerable code snippet
- Recommended fix with code examples

## Technology Stack

- **Backend**: FastAPI, OpenAI Agents (0.2.4+), MCP (1.12.2)
- **Frontend**: Next.js 15, React 19, Tailwind CSS 4
- **Security**: Semgrep MCP server
- **Infrastructure**: Docker, Terraform, Azure/GCP

## License

MIT

## Contributing

Contributions are welcome! Please ensure code passes security analysis before submitting PRs.
