---
description: >-
  Complete grimoire for AI Elements - a Vercel-supported UI library for building AI/chat mystical interfaces
  with shadcn/ui magic. Master chat enchantments, Vercel AI SDK integration, and conversation UI mystical patterns
  for modern AI mystical applications.

theme: "magic"
---

# AI Elements - The AI Interface Mastery

## The Ancient Knowledge

AI Elements is a Vercel-supported UI library specifically designed for building AI/chat interfaces, built on shadcn/ui. It provides ready-made React components for common AI app primitives including message threads, chat bubbles, input boxes, action buttons, and reasoning panels, serving as the UI front-end kit for the Vercel AI SDK.

## Key Features

- **AI-Focused Components**: Purpose-built for conversational AI interfaces
- **Vercel AI SDK Integration**: Seamless integration with Vercel's AI toolkit
- **shadcn/ui Foundation**: Built on the reliable shadcn/ui component system
- **Real-time Chat**: Support for streaming responses and real-time updates
- **Customizable Themes**: Adaptable to different AI application designs
- **TypeScript Support**: Full type safety for AI conversation flows
- **Accessibility**: Screen reader friendly chat interfaces

## Use Cases

1. **AI Chatbots**: Customer support and general-purpose chat interfaces
2. **AI Assistants**: Personal productivity and task management tools
3. **Code Assistants**: Developer tools with AI-powered code generation
4. **Educational AI**: Tutoring systems and interactive learning platforms
5. **Content Generation**: AI writing tools and creative assistants
6. **FAQ Systems**: Automated question-answering interfaces

## Installation and Setup

### 1. Prerequisites

Set up a Next.js project with shadcn/ui and Vercel AI SDK:

```bash
# Create Next.js project
pnpm create next-app@latest my-ai-app --typescript --tailwind --eslint
cd my-ai-app

# Initialize shadcn/ui
pnpm dlx shadcn-ui@latest init

# Install Vercel AI SDK
pnpm add ai @ai-sdk/openai @ai-sdk/anthropic
pnpm add zod # For schema validation
```

### 2. Install AI Elements

```bash
# Install AI Elements
pnpm add @vercel/ai-elements

# Install required shadcn/ui components
pnpm dlx shadcn-ui@latest add button input textarea card avatar badge
```

### 3. Configure Environment

Set up your AI provider API keys:

```bash
# .env.local
OPENAI_API_KEY=your_openai_api_key
ANTHROPIC_API_KEY=your_anthropic_api_key
```

## Core Components

### 1. Chat Interface

Basic chat interface with message history:

```typescript
"use client"

import { useChat } from 'ai/react'
import { ChatMessage } from '@vercel/ai-elements/chat-message'
import { ChatInput } from '@vercel/ai-elements/chat-input'
import { ChatContainer } from '@vercel/ai-elements/chat-container'
import { Button } from '@/components/ui/button'
import { Card } from '@/components/ui/card'

export default function ChatInterface() {
  const { messages, input, handleInputChange, handleSubmit, isLoading } = useChat({
    api: '/api/chat',
  })

  return (
    <Card className="w-full max-w-2xl mx-auto h-[600px] flex flex-col">
      <div className="p-4 border-b">
        <h2 className="text-lg font-semibold">AI Assistant</h2>
      </div>
      
      <ChatContainer className="flex-1 overflow-y-auto p-4 space-y-4">
        {messages.length === 0 && (
          <div className="text-center text-gray-500 py-8">
            <p>Start a conversation with your AI assistant</p>
          </div>
        )}
        
        {messages.map((message) => (
          <ChatMessage
            key={message.id}
            message={message}
            isUser={message.role === 'user'}
            className="max-w-[80%]"
          />
        ))}
        
        {isLoading && (
          <ChatMessage
            message={{ role: 'assistant', content: 'Thinking...' }}
            isUser={false}
            isLoading={true}
          />
        )}
      </ChatContainer>
      
      <div className="p-4 border-t">
        <form onSubmit={handleSubmit} className="flex gap-2">
          <ChatInput
            value={input}
            onChange={handleInputChange}
            placeholder="Type your message..."
            disabled={isLoading}
            className="flex-1"
          />
          <Button type="submit" disabled={isLoading || !input.trim()}>
            Send
          </Button>
        </form>
      </div>
    </Card>
  )
}
```

### 2. Advanced Chat with Actions

Chat interface with action buttons and file uploads:

```typescript
"use client"

import { useChat } from 'ai/react'
import { ChatMessage } from '@vercel/ai-elements/chat-message'
import { ChatInput } from '@vercel/ai-elements/chat-input'
import { ActionButton } from '@vercel/ai-elements/action-button'
import { FileUpload } from '@vercel/ai-elements/file-upload'
import { Button } from '@/components/ui/button'
import { Card } from '@/components/ui/card'
import { Badge } from '@/components/ui/badge'
import { Paperclip, Mic, Send } from 'lucide-react'

const quickActions = [
  { label: "Explain this code", action: "explain" },
  { label: "Write tests", action: "test" },
  { label: "Optimize performance", action: "optimize" },
  { label: "Add documentation", action: "document" }
]

export default function AdvancedChat() {
  const { 
    messages, 
    input, 
    handleInputChange, 
    handleSubmit, 
    isLoading,
    append 
  } = useChat({
    api: '/api/chat',
  })

  const handleQuickAction = (action: string) => {
    append({
      role: 'user',
      content: `Please ${action} the following code.`
    })
  }

  const handleFileUpload = (file: File) => {
    // Handle file upload logic
    const reader = new FileReader()
    reader.onload = (e) => {
      const content = e.target?.result as string
      append({
        role: 'user',
        content: `Please analyze this file: ${file.name}\n\n${content}`
      })
    }
    reader.readAsText(file)
  }

  return (
    <div className="w-full max-w-4xl mx-auto">
      {/* Quick Actions */}
      <div className="mb-4 flex flex-wrap gap-2">
        {quickActions.map((action) => (
          <ActionButton
            key={action.action}
            onClick={() => handleQuickAction(action.action)}
            variant="outline"
            size="sm"
          >
            {action.label}
          </ActionButton>
        ))}
      </div>

      <Card className="h-[700px] flex flex-col">
        <div className="p-4 border-b flex justify-between items-center">
          <h2 className="text-lg font-semibold">Code Assistant</h2>
          <Badge variant="secondary">GPT-4</Badge>
        </div>
        
        <div className="flex-1 overflow-y-auto p-4 space-y-4">
          {messages.map((message) => (
            <ChatMessage
              key={message.id}
              message={message}
              isUser={message.role === 'user'}
              showAvatar={true}
              showTimestamp={true}
              className="max-w-[85%]"
            />
          ))}
          
          {isLoading && (
            <div className="flex items-center gap-2 text-gray-500">
              <div className="animate-spin rounded-full h-4 w-4 border-b-2 border-blue-600"></div>
              <span>AI is thinking...</span>
            </div>
          )}
        </div>
        
        <div className="p-4 border-t">
          <form onSubmit={handleSubmit} className="space-y-3">
            <div className="flex gap-2">
              <FileUpload
                onFileSelect={handleFileUpload}
                accept=".js,.ts,.jsx,.tsx,.py,.java,.cpp"
                className="flex-shrink-0"
              >
                <Button type="button" variant="outline" size="icon">
                  <Paperclip className="h-4 w-4" />
                </Button>
              </FileUpload>
              
              <ChatInput
                value={input}
                onChange={handleInputChange}
                placeholder="Ask about your code, request explanations, or get help..."
                disabled={isLoading}
                className="flex-1"
                multiline={true}
                maxRows={4}
              />
              
              <Button type="button" variant="outline" size="icon">
                <Mic className="h-4 w-4" />
              </Button>
              
              <Button type="submit" disabled={isLoading || !input.trim()}>
                <Send className="h-4 w-4" />
              </Button>
            </div>
          </form>
        </div>
      </Card>
    </div>
  )
}
```

### 3. Reasoning Panel

Display AI reasoning process and thought chains:

```typescript
import { ReasoningPanel } from '@vercel/ai-elements/reasoning-panel'
import { ThoughtChain } from '@vercel/ai-elements/thought-chain'
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card'
import { Badge } from '@/components/ui/badge'
import { Brain, CheckCircle, AlertCircle } from 'lucide-react'

interface ReasoningStep {
  id: string
  title: string
  content: string
  status: 'thinking' | 'complete' | 'error'
  confidence?: number
}

const reasoningSteps: ReasoningStep[] = [
  {
    id: '1',
    title: 'Understanding the Problem',
    content: 'Analyzing the user\'s request to identify the core requirements and constraints.',
    status: 'complete',
    confidence: 95
  },
  {
    id: '2', 
    title: 'Exploring Solutions',
    content: 'Considering multiple approaches: recursive solution, iterative approach, and dynamic programming.',
    status: 'complete',
    confidence: 88
  },
  {
    id: '3',
    title: 'Evaluating Trade-offs',
    content: 'Comparing time complexity O(n) vs space complexity O(1) for different implementations.',
    status: 'thinking',
    confidence: 70
  }
]

export default function ReasoningInterface() {
  return (
    <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
      {/* Main Chat Area */}
      <div className="lg:col-span-2">
        <Card className="h-[600px]">
          <CardHeader>
            <CardTitle>AI Assistant</CardTitle>
            <CardDescription>
              Ask complex questions and see the reasoning process
            </CardDescription>
          </CardHeader>
          <CardContent>
            {/* Chat messages would go here */}
            <div className="text-center text-gray-500 py-8">
              <Brain className="h-12 w-12 mx-auto mb-4 text-blue-500" />
              <p>Ask a complex question to see AI reasoning in action</p>
            </div>
          </CardContent>
        </Card>
      </div>

      {/* Reasoning Panel */}
      <div className="lg:col-span-1">
        <ReasoningPanel className="h-[600px]">
          <Card className="h-full">
            <CardHeader>
              <CardTitle className="flex items-center gap-2">
                <Brain className="h-5 w-5" />
                Reasoning Process
              </CardTitle>
            </CardHeader>
            <CardContent className="space-y-4">
              <ThoughtChain steps={reasoningSteps}>
                {reasoningSteps.map((step) => (
                  <div key={step.id} className="border rounded-lg p-4 space-y-2">
                    <div className="flex items-center justify-between">
                      <h4 className="font-medium">{step.title}</h4>
                      {step.status === 'complete' && (
                        <CheckCircle className="h-4 w-4 text-green-500" />
                      )}
                      {step.status === 'thinking' && (
                        <div className="animate-spin rounded-full h-4 w-4 border-b-2 border-blue-500" />
                      )}
                      {step.status === 'error' && (
                        <AlertCircle className="h-4 w-4 text-red-500" />
                      )}
                    </div>
                    
                    <p className="text-sm text-gray-600">{step.content}</p>
                    
                    {step.confidence && (
                      <div className="flex items-center gap-2">
                        <span className="text-xs text-gray-500">Confidence:</span>
                        <Badge variant="secondary" className="text-xs">
                          {step.confidence}%
                        </Badge>
                      </div>
                    )}
                  </div>
                ))}
              </ThoughtChain>
            </CardContent>
          </Card>
        </ReasoningPanel>
      </div>
    </div>
  )
}
```

## API Integration

### Setting up the Chat API Route

Create the backend API for chat functionality:

```typescript
// app/api/chat/route.ts
import { openai } from '@ai-sdk/openai'
import { streamText } from 'ai'

export async function POST(req: Request) {
  const { messages } = await req.json()

  const result = await streamText({
    model: openai('gpt-4-turbo'),
    messages,
    system: 'You are a helpful AI assistant. Provide clear, concise, and accurate responses.',
    maxTokens: 1000,
    temperature: 0.7,
  })

  return result.toAIStreamResponse()
}
```

### Advanced API with Function Calling

Implement function calling for enhanced AI capabilities:

```typescript
// app/api/chat/route.ts
import { openai } from '@ai-sdk/openai'
import { streamText, tool } from 'ai'
import { z } from 'zod'

const tools = {
  searchCode: tool({
    description: 'Search for code examples or documentation',
    parameters: z.object({
      query: z.string().describe('The search query'),
      language: z.string().optional().describe('Programming language filter'),
    }),
    execute: async ({ query, language }) => {
      // Implement code search logic
      return {
        results: [
          {
            title: `${language} example for ${query}`,
            code: `// Example code for ${query}`,
            description: `This shows how to implement ${query} in ${language}`
          }
        ]
      }
    },
  }),
  
  generateTests: tool({
    description: 'Generate unit tests for provided code',
    parameters: z.object({
      code: z.string().describe('The code to generate tests for'),
      framework: z.string().optional().describe('Testing framework preference'),
    }),
    execute: async ({ code, framework = 'jest' }) => {
      // Implement test generation logic
      return {
        tests: `// Generated ${framework} tests\n// for the provided code`,
        coverage: 85
      }
    },
  }),
}

export async function POST(req: Request) {
  const { messages } = await req.json()

  const result = await streamText({
    model: openai('gpt-4-turbo'),
    messages,
    tools,
    system: 'You are a coding assistant. Help users with code-related questions and tasks.',
    maxTokens: 2000,
  })

  return result.toAIStreamResponse()
}
```

## Best Practices

1. **Error Handling**: Implement proper error boundaries and fallback states
2. **Loading States**: Show clear loading indicators during AI responses
3. **Rate Limiting**: Implement rate limiting to prevent API abuse
4. **Message Persistence**: Store conversation history for better UX
5. **Accessibility**: Ensure chat interfaces work with screen readers
6. **Performance**: Optimize for streaming responses and real-time updates

## Common Pitfalls

### Issue 1: Streaming Response Handling
Improper handling of streaming AI responses.

**Solution:**
```typescript
// Properly handle streaming with error boundaries
const { messages, error, isLoading } = useChat({
  api: '/api/chat',
  onError: (error) => {
    console.error('Chat error:', error)
    // Show user-friendly error message
  },
  onFinish: (message) => {
    // Handle completion
  }
})
```

### Issue 2: Memory Management
Chat history growing too large and causing performance issues.

**Solution:**
```typescript
// Implement message limit and cleanup
const MAX_MESSAGES = 50

const { messages } = useChat({
  api: '/api/chat',
  initialMessages: [],
  onFinish: (message, { messages }) => {
    if (messages.length > MAX_MESSAGES) {
      // Keep only recent messages
      const recentMessages = messages.slice(-MAX_MESSAGES)
      // Update state or persist to storage
    }
  }
})
```

### Issue 3: API Key Security
Exposing API keys in client-side code.

**Solution:**
```typescript
// Always use API routes for AI calls
// Never put API keys in client-side code
// Use environment variables on the server

// ❌ Wrong - client-side
const openai = new OpenAI({ apiKey: 'sk-...' })

// ✅ Correct - server-side API route
// app/api/chat/route.ts
const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY })
```

## References

{% embed url="https://sdk.vercel.ai/" %}

{% embed url="https://github.com/vercel/ai" %}

{% embed url="https://ui.shadcn.com/" %}
