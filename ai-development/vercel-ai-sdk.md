---
description: >-
  Complete guide to the Vercel AI SDK for building AI-powered applications with
  streaming, chat interfaces, and modern React patterns. Covers installation,
  core concepts, and integration with Next.js.
---

# Vercel AI SDK

## Introduction

The Vercel AI SDK is a TypeScript-first library designed to help developers build AI-powered applications with React, Next.js, and other modern frameworks. It provides streaming capabilities, chat interfaces, and seamless integration with various AI providers like OpenAI, Anthropic, and others.

## Use Cases

1. **Chat Applications**: Build conversational interfaces with streaming responses and message history
2. **Content Generation**: Create tools for text generation, summarization, and content creation
3. **AI-Powered Forms**: Enhance forms with AI assistance and real-time suggestions
4. **Streaming Interfaces**: Implement real-time AI responses with progressive loading

## Installation

### Using pnpm (Recommended)

```bash
pnpm add ai
```

### Using yarn

```bash
yarn add ai
```

### Additional Dependencies for Next.js

```bash
pnpm add openai
# or for other providers
pnpm add @anthropic-ai/sdk
```

## Basic Setup

### 1. Environment Configuration

Create a `.env.local` file in your Next.js project:

```env
OPENAI_API_KEY=your_openai_api_key_here
# or for Anthropic
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

### 2. API Route Setup (Next.js App Router)

Create `app/api/chat/route.ts`:

```typescript
import { openai } from '@ai-sdk/openai';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = await streamText({
    model: openai('gpt-4'),
    messages,
  });

  return result.toAIStreamResponse();
}
```

## Core Concepts

### 1. useChat Hook

The `useChat` hook provides everything needed for chat interfaces:

```typescript
import { useChat } from 'ai/react';

export default function ChatComponent() {
  const { messages, input, handleInputChange, handleSubmit, isLoading } = useChat();

  return (
    <div className="chat-container">
      {/* Messages display */}
      <div className="messages">
        {messages.map((message) => (
          <div key={message.id} className={`message ${message.role}`}>
            <strong>{message.role}:</strong> {message.content}
          </div>
        ))}
      </div>

      {/* Input form */}
      <form onSubmit={handleSubmit} className="chat-form">
        <input
          value={input}
          onChange={handleInputChange}
          placeholder="Type your message..."
          disabled={isLoading}
        />
        <button type="submit" disabled={isLoading}>
          {isLoading ? 'Sending...' : 'Send'}
        </button>
      </form>
    </div>
  );
}
```

### 2. useCompletion Hook

For single completion requests:

```typescript
import { useCompletion } from 'ai/react';

export default function CompletionComponent() {
  const { completion, input, handleInputChange, handleSubmit, isLoading } = useCompletion();

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          value={input}
          onChange={handleInputChange}
          placeholder="Enter your prompt..."
        />
        <button type="submit" disabled={isLoading}>
          Generate
        </button>
      </form>
      
      {completion && (
        <div className="completion-result">
          <h3>Generated Content:</h3>
          <p>{completion}</p>
        </div>
      )}
    </div>
  );
}
```

## Advanced Usage

### 1. Custom Message Handling

```typescript
import { useChat } from 'ai/react';
import { Message } from 'ai';

export default function AdvancedChat() {
  const { messages, append, isLoading } = useChat();

  const handleCustomMessage = async () => {
    const customMessage: Message = {
      id: Date.now().toString(),
      role: 'user',
      content: 'Explain quantum computing in simple terms',
    };
    
    await append(customMessage);
  };

  return (
    <div>
      <button onClick={handleCustomMessage} disabled={isLoading}>
        Ask about Quantum Computing
      </button>
      
      <div className="messages">
        {messages.map((message) => (
          <div key={message.id}>
            {message.content}
          </div>
        ))}
      </div>
    </div>
  );
}
```

### 2. Streaming with Custom UI

```typescript
import { useChat } from 'ai/react';

export default function StreamingChat() {
  const { messages, input, handleInputChange, handleSubmit, isLoading } = useChat({
    onResponse: (response) => {
      console.log('Response started:', response.status);
    },
    onFinish: (message) => {
      console.log('Message completed:', message);
    },
  });

  return (
    <div className="streaming-chat">
      <div className="messages-container">
        {messages.map((message) => (
          <div key={message.id} className={`message-bubble ${message.role}`}>
            <div className="message-content">
              {message.content}
            </div>
            {isLoading && message === messages[messages.length - 1] && (
              <div className="typing-indicator">
                <span></span>
                <span></span>
                <span></span>
              </div>
            )}
          </div>
        ))}
      </div>
      
      <form onSubmit={handleSubmit} className="input-form">
        <input
          value={input}
          onChange={handleInputChange}
          placeholder="Type a message..."
          className="message-input"
        />
        <button type="submit" disabled={isLoading}>
          Send
        </button>
      </form>
    </div>
  );
}
```

## Integration Patterns

### 1. With Next.js Server Actions

```typescript
// app/actions.ts
'use server';

import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';

export async function generateSummary(content: string) {
  const { text } = await generateText({
    model: openai('gpt-4'),
    prompt: `Summarize the following content: ${content}`,
  });

  return text;
}
```

### 2. With React Server Components

```typescript
// app/summary/page.tsx
import { generateSummary } from '../actions';

export default async function SummaryPage({
  searchParams,
}: {
  searchParams: { content?: string };
}) {
  const content = searchParams.content;
  const summary = content ? await generateSummary(content) : null;

  return (
    <div>
      <h1>Content Summary</h1>
      {summary ? (
        <div className="summary">
          <h2>Summary:</h2>
          <p>{summary}</p>
        </div>
      ) : (
        <p>No content to summarize</p>
      )}
    </div>
  );
}
```

## Best Practices

- **Error Handling**: Always implement proper error handling for API failures
- **Rate Limiting**: Implement client-side rate limiting to prevent API abuse
- **Message Persistence**: Store chat history in local storage or database
- **Loading States**: Provide clear loading indicators during AI responses
- **Streaming UX**: Use streaming for better user experience with long responses
- **Security**: Never expose API keys in client-side code
- **Caching**: Implement response caching for repeated queries

## Common Pitfalls

### Issue 1: API Key Exposure
Never include API keys in client-side code.

**Solution:**
Always use API routes or server actions to handle AI provider communication.

### Issue 2: Unhandled Streaming Errors
Streaming responses can fail mid-stream.

**Solution:**
```typescript
const { messages, error } = useChat({
  onError: (error) => {
    console.error('Chat error:', error);
    // Handle error appropriately
  },
});

if (error) {
  return <div>Error: {error.message}</div>;
}
```

### Issue 3: Memory Leaks with Long Conversations
Long chat sessions can consume excessive memory.

**Solution:**
Implement message pagination or conversation limits:

```typescript
const { messages } = useChat({
  maxToolRoundtrips: 5,
  // Limit conversation length
});
```

## References

{% embed url="https://sdk.vercel.ai/docs" %}

{% embed url="https://github.com/vercel/ai" %}
