---
title: "Simple Chatbot Use Case"
description: "A ready-to-deploy chatbot implementation for customer support with web interface and AWS backend"
date: "2025-01-05"
type: "use-cases"
skillLevel: "Beginner"
frameworks: ["React", "Strands", "AWS CDK"]
services: ["Amazon Bedrock", "AWS Lambda", "Amazon API Gateway", "Amazon S3"]
url: "https://github.com/aws-samples/simple-chatbot-use-case"
image: "/images/aws-logo.svg"
---

# Simple Chatbot Use Case

This use case provides a complete, production-ready chatbot implementation that you can deploy and customize for your business needs. Perfect for customer support, FAQ handling, or general conversational AI applications.

## Project Overview

The Simple Chatbot includes:

- **Web Interface**: React-based chat UI with modern design
- **Backend API**: Serverless Lambda functions with API Gateway
- **AI Engine**: Amazon Bedrock integration for natural language processing
- **Infrastructure**: Complete AWS CDK deployment stack
- **Monitoring**: CloudWatch dashboards and alerts

## Live Demo

🔗 **[Try the live demo](https://demo.simple-chatbot.aws-samples.com)**

## Architecture Diagram

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Web Client    │────│   API Gateway    │────│  Lambda Function │
│   (React App)   │    │                  │    │   (Chatbot)     │
└─────────────────┘    └──────────────────┘    └─────────────────┘
                                                         │
                                                         ▼
                                                ┌─────────────────┐
                                                │ Amazon Bedrock  │
                                                │  (Claude 3)     │
                                                └─────────────────┘
```

## Features

### Core Functionality
- Real-time chat interface
- Context-aware conversations
- Message history persistence
- Typing indicators and status
- Mobile-responsive design

### AI Capabilities
- Natural language understanding
- Intent recognition
- Multi-turn conversations
- Fallback handling
- Custom knowledge integration

### Admin Features
- Conversation analytics
- Usage metrics dashboard
- Configuration management
- A/B testing support

## Quick Start

### 1. Prerequisites

Ensure you have the following installed:
- Node.js 18+
- AWS CLI configured
- Git

### 2. Clone and Setup

```bash
# Clone the repository
git clone https://github.com/aws-samples/simple-chatbot-use-case
cd simple-chatbot-use-case

# Install dependencies
npm install
cd frontend && npm install && cd ..
```

### 3. Configure Environment

```bash
# Copy environment template
cp .env.example .env

# Edit configuration
nano .env
```

Required environment variables:
```bash
AWS_REGION=us-east-1
BEDROCK_MODEL_ID=anthropic.claude-3-sonnet-20240229-v1:0
CHATBOT_NAME=Simple Chatbot
COMPANY_NAME=Your Company
```

### 4. Deploy Infrastructure

```bash
# Deploy backend infrastructure
npm run deploy:backend

# Build and deploy frontend
npm run deploy:frontend
```

### 5. Test Your Chatbot

After deployment, you'll receive:
- **Frontend URL**: Your chatbot web interface
- **API Endpoint**: Backend API for integration
- **CloudWatch Dashboard**: Monitoring and metrics

## Implementation Details

### Backend Lambda Function

```python
import json
import boto3
from strands import Agent, BedrockProvider

# Initialize the chatbot agent
chatbot = Agent(
    name="SimpleChatbot",
    model_provider=BedrockProvider(
        model_id="anthropic.claude-3-sonnet-20240229-v1:0"
    ),
    system_prompt="""
    You are a helpful customer support chatbot.
    - Be friendly and professional
    - Provide clear, concise answers
    - If you don't know something, offer to connect with human support
    - Keep responses under 200 words
    """
)

def lambda_handler(event, context):
    try:
        # Parse incoming message
        body = json.loads(event['body'])
        user_message = body['message']
        session_id = body.get('session_id', 'default')
        
        # Generate response
        response = chatbot.process_message(
            message=user_message,
            session_id=session_id
        )
        
        # Return formatted response
        return {
            'statusCode': 200,
            'headers': {
                'Access-Control-Allow-Origin': '*',
                'Content-Type': 'application/json'
            },
            'body': json.dumps({
                'response': response.content,
                'session_id': session_id,
                'timestamp': response.timestamp
            })
        }
        
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

### Frontend React Component

```jsx
import React, { useState, useEffect } from 'react';
import ChatInterface from './components/ChatInterface';
import { ChatbotAPI } from './services/api';

function App() {
  const [messages, setMessages] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  
  const sendMessage = async (message) => {
    setIsLoading(true);
    
    // Add user message
    setMessages(prev => [...prev, {
      type: 'user',
      content: message,
      timestamp: new Date()
    }]);
    
    try {
      // Call backend API
      const response = await ChatbotAPI.sendMessage(message);
      
      // Add bot response
      setMessages(prev => [...prev, {
        type: 'bot',
        content: response.response,
        timestamp: new Date(response.timestamp)
      }]);
    } catch (error) {
      console.error('Failed to send message:', error);
    } finally {
      setIsLoading(false);
    }
  };
  
  return (
    <div className="App">
      <ChatInterface 
        messages={messages}
        onSendMessage={sendMessage}
        isLoading={isLoading}
      />
    </div>
  );
}

export default App;
```

## Customization Guide

### 1. Modify Chatbot Personality

Edit the system prompt in `src/lambda/chatbot.py`:

```python
system_prompt = """
You are a [YOUR BRAND] support assistant.
- Speak in a [TONE: friendly/professional/casual] manner
- Focus on [YOUR DOMAIN: e-commerce/SaaS/healthcare]
- Always mention [YOUR COMPANY VALUES]
"""
```

### 2. Add Custom Knowledge

Integrate your knowledge base:

```python
from strands.tools import KnowledgeBaseTool

knowledge_tool = KnowledgeBaseTool(
    documents_path="knowledge/",
    embedding_model="amazon.titan-embed-text-v1"
)

chatbot.add_tools([knowledge_tool])
```

### 3. Customize UI Theme

Update `frontend/src/styles/theme.js`:

```javascript
export const theme = {
  primary: '#your-brand-color',
  secondary: '#your-accent-color',
  background: '#your-bg-color',
  // ... other theme variables
};
```

## Deployment Options

### Option 1: Serverless (Recommended)
- Automatic scaling
- Pay-per-use pricing
- Minimal maintenance

### Option 2: Container-based
- More control over environment
- Consistent scaling
- Suitable for high-volume usage

### Option 3: Development Mode
```bash
# Run locally for development
npm run dev:backend &
npm run dev:frontend
```

## Monitoring and Analytics

### Built-in Metrics
- Message volume
- Response times
- User satisfaction
- Error rates
- Cost per conversation

### CloudWatch Dashboard
Access your dashboard at: `https://console.aws.amazon.com/cloudwatch/home#dashboards:name=SimpleChatbot`

Key metrics tracked:
- Lambda invocations
- API Gateway requests
- Bedrock token usage
- Error counts

## Security Features

- **API Authentication**: Optional JWT integration
- **Rate Limiting**: Prevents abuse
- **Input Validation**: Sanitizes user input
- **CORS Configuration**: Secure cross-origin requests
- **IAM Permissions**: Least privilege access

## Cost Optimization

### Expected Costs (Monthly)
- **Light usage** (1K messages): ~$5-10
- **Medium usage** (10K messages): ~$25-50
- **Heavy usage** (100K messages): ~$200-400

### Optimization Tips
1. Implement response caching for common queries
2. Use shorter system prompts
3. Set conversation limits
4. Monitor token usage regularly

## Troubleshooting

### Common Issues

**Chatbot not responding:**
```bash
# Check Lambda logs
aws logs tail /aws/lambda/simple-chatbot --follow
```

**CORS errors:**
- Verify API Gateway CORS configuration
- Check frontend environment variables

**High costs:**
- Review CloudWatch metrics
- Implement conversation limits
- Optimize system prompts

## Integration Examples

### Slack Integration
```python
from slack_bolt import App

app = App(token="your-slack-token")

@app.message()
def handle_message(message, say):
    response = chatbot.process_message(message['text'])
    say(response.content)
```

### WhatsApp Integration
```python
from twilio.rest import Client

def handle_whatsapp_message(request):
    message = request.form['Body']
    response = chatbot.process_message(message)
    
    # Send via Twilio
    client.messages.create(
        body=response.content,
        from_='whatsapp:+14155238886',
        to=request.form['From']
    )
```

## Next Steps

1. **Customize** the chatbot for your specific domain
2. **Integrate** with your existing systems (CRM, helpdesk)
3. **Scale** with additional features (voice, multilingual)
4. **Analyze** conversations to improve performance

## Support

- 📖 [Full Documentation](https://github.com/aws-samples/simple-chatbot-use-case/wiki)
- 🐛 [Report Issues](https://github.com/aws-samples/simple-chatbot-use-case/issues)
- 💬 [Community Discussions](https://github.com/aws-samples/simple-chatbot-use-case/discussions)

---

*This use case demonstrates a complete chatbot implementation. Use it as a starting point and customize for your specific needs.*