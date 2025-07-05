---
title: "Hello World Agent Template"
description: "A simple template for creating your first agentic AI application with basic conversational capabilities"
date: "2025-01-05"
type: "templates"
skillLevel: "Beginner"
frameworks: ["Strands", "LangChain"]
services: ["Amazon Bedrock", "AWS Lambda"]
url: "https://github.com/aws-samples/hello-world-agent-template"
image: "/images/aws-logo.svg"
---

# Hello World Agent Template

This template provides a foundational structure for building your first agentic AI application. It demonstrates basic conversational capabilities and can be extended for more complex use cases.

## Architecture Overview

The Hello World Agent template consists of:

- **Agent Core**: Basic reasoning engine using Amazon Bedrock
- **Conversation Handler**: Manages user interactions and context
- **Response Generator**: Formats and delivers responses
- **Deployment Stack**: AWS infrastructure as code

## Key Components

### 1. Agent Configuration

```python
from strands import Agent, BedrockProvider

# Initialize the agent
agent = Agent(
    name="HelloWorldAgent",
    model_provider=BedrockProvider(
        model_id="anthropic.claude-3-sonnet-20240229-v1:0",
        region="us-east-1"
    ),
    system_prompt="You are a helpful AI assistant. Respond in a friendly and concise manner."
)
```

### 2. Basic Conversation Flow

The template handles simple conversational patterns:

- Greeting detection and response
- Question answering capabilities
- Context awareness within conversation
- Graceful error handling

### 3. Infrastructure Setup

Using AWS CDK for deployment:

```typescript
import * as cdk from 'aws-cdk-lib';
import * as lambda from 'aws-cdk-lib/aws-lambda';
import * as bedrock from 'aws-cdk-lib/aws-bedrock';

export class HelloWorldAgentStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props);
    
    // Lambda function for the agent
    const agentFunction = new lambda.Function(this, 'HelloWorldAgent', {
      runtime: lambda.Runtime.PYTHON_3_11,
      handler: 'main.handler',
      code: lambda.Code.fromAsset('src'),
      environment: {
        BEDROCK_MODEL_ID: 'anthropic.claude-3-sonnet-20240229-v1:0'
      }
    });
  }
}
```

## Getting Started

### Prerequisites

- AWS CLI configured with appropriate permissions
- Node.js 18+ (for CDK deployment)
- Python 3.11+ (for agent code)

### Installation Steps

1. **Clone the template repository**
   ```bash
   git clone https://github.com/aws-samples/hello-world-agent-template
   cd hello-world-agent-template
   ```

2. **Install dependencies**
   ```bash
   npm install
   pip install -r requirements.txt
   ```

3. **Configure your environment**
   ```bash
   cp .env.example .env
   # Edit .env with your AWS settings
   ```

4. **Deploy the infrastructure**
   ```bash
   npm run deploy
   ```

5. **Test your agent**
   ```bash
   python test_agent.py
   ```

## Customization Options

### Extending the System Prompt

Modify the agent's behavior by updating the system prompt:

```python
system_prompt = """
You are a helpful AI assistant specialized in [YOUR DOMAIN].
Your responses should be:
- Clear and concise
- Technically accurate
- Friendly and approachable
"""
```

### Adding Tools and Functions

Enhance your agent with additional capabilities:

```python
from strands.tools import WebSearchTool, CalculatorTool

agent.add_tools([
    WebSearchTool(),
    CalculatorTool()
])
```

### Custom Response Formatting

Implement custom response handlers:

```python
def format_response(response: str, context: dict) -> dict:
    return {
        "message": response,
        "timestamp": datetime.now().isoformat(),
        "context_used": len(context.get("messages", []))
    }
```

## Testing

The template includes comprehensive tests:

```bash
# Run unit tests
python -m pytest tests/unit/

# Run integration tests
python -m pytest tests/integration/

# Run end-to-end tests
python -m pytest tests/e2e/
```

## Monitoring and Observability

Built-in monitoring includes:

- CloudWatch logs for all agent interactions
- Metrics for response times and success rates
- Cost tracking for Bedrock usage

## Security Considerations

- All API calls are authenticated using IAM roles
- Environment variables for sensitive configuration
- Input validation and sanitization
- Rate limiting for API endpoints

## Next Steps

Once you have your Hello World agent running:

1. **Experiment with different prompts** to understand behavior changes
2. **Add your first tool** to extend functionality
3. **Implement conversation memory** for multi-turn interactions
4. **Explore advanced templates** for specific use cases

## Troubleshooting

### Common Issues

**Agent not responding:**
- Check CloudWatch logs for errors
- Verify Bedrock model permissions
- Ensure environment variables are set correctly

**High costs:**
- Review your conversation patterns
- Implement caching for repeated queries
- Monitor token usage in CloudWatch

**Slow responses:**
- Check Lambda timeout settings
- Consider using provisioned concurrency
- Optimize your system prompt length

## Related Resources

- [Strands Agents Documentation](https://docs.strands.ai)
- [Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/)
- [Advanced Agent Templates](https://aws.amazon.com/ai/agents/templates/)

---

*This template serves as your starting point for agentic AI development. Modify and extend it based on your specific requirements.*