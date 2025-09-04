---
description: >-
  Complete grimoire for the Vercel AI SDK - master the art of building intelligent
  enchantments with streaming consciousness, conversational interfaces, and modern
  React alchemy. Covers mystical installation, core magical concepts, and integration
  with Next.js portal magic.
---

# Vercel AI SDK - Intelligent Enchantment Mastery

## The Ancient Knowledge

The Vercel AI SDK is a TypeScript-first mystical library designed to help sorcerers craft AI-powered enchantments with React, Next.js, and other modern magical frameworks. It provides streaming consciousness capabilities, conversational interfaces, and seamless integration with various AI spirits like OpenAI, Anthropic, and other digital oracles.

## When to Cast These Spells

1. **Conversational Enchantments**: Build talking interfaces with streaming responses and memory of past exchanges
2. **Content Conjuration**: Create tools for text generation, summarization, and mystical content creation
3. **AI-Enhanced Ritual Forms**: Enhance forms with intelligent assistance and real-time magical suggestions
4. **Streaming Consciousness Interfaces**: Implement real-time AI responses with progressive manifestation

## Summoning the SDK

### Using pnpm (The Preferred Ritual)

```bash
pnpm add ai
```

### Using yarn

```bash
yarn add ai
```

### Additional Mystical Dependencies for Next.js

```bash
pnpm add openai
# or for other AI spirits
pnpm add @anthropic-ai/sdk
```

## Basic Ritual Setup

### 1. Sacred Environment Configuration

Create a `.env.local` scroll in your Next.js magical project:

```env
OPENAI_API_KEY=your_openai_api_key_here
# or for Anthropic spirits
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

### 2. API Portal Setup (Next.js App Router)

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

## Core Magical Concepts

### 1. useChat Conversational Enchantment

The `useChat` hook provides everything needed for conversational magical interfaces:

```typescript
import { useChat } from 'ai/react';

export default function ChatComponent() {
  const { messages, input, handleInputChange, handleSubmit, isLoading } = useChat();

  return (
    <div className="chat-container">
      {/* Mystical message display */}
      <div className="messages">
        {messages.map((message) => (
          <div key={message.id} className={`message ${message.role}`}>
            <strong>{message.role}:</strong> {message.content}
          </div>
        ))}
      </div>

      {/* Spell input form */}
      <form onSubmit={handleSubmit} className="chat-form">
        <input
          value={input}
          onChange={handleInputChange}
          placeholder="Speak your query to the AI spirit..."
          disabled={isLoading}
        />
        <button type="submit" disabled={isLoading}>
          {isLoading ? 'Channeling...' : 'Cast Spell'}
        </button>
      </form>
    </div>
  );
}
```

### 2. useCompletion Single Conjuration Hook

For single completion magical requests:

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
          placeholder="Enter your mystical prompt..."
        />
        <button type="submit" disabled={isLoading}>
          Conjure Content
        </button>
      </form>

      {completion && (
        <div className="completion-result">
          <h3>Manifested Content:</h3>
          <p>{completion}</p>
        </div>
      )}
    </div>
  );
}
```

## Advanced Sorcery

### 1. Custom Message Weaving

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
        Invoke Quantum Knowledge Spell
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

### 2. Streaming Consciousness with Custom Enchanted UI

```typescript
import { useChat } from 'ai/react';

export default function StreamingChat() {
  const { messages, input, handleInputChange, handleSubmit, isLoading } = useChat({
    onResponse: (response) => {
      console.log('AI spirit awakening:', response.status);
    },
    onFinish: (message) => {
      console.log('Mystical message completed:', message);
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
          placeholder="Whisper to the AI spirit..."
          className="message-input"
        />
        <button type="submit" disabled={isLoading}>
          Channel Message
        </button>
      </form>
    </div>
  );
}
```

## Integration Ritual Patterns

### 1. With Next.js Server Action Spells

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

### 2. With React Server Component Enchantments

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
      <h1>Content Mystical Summary</h1>
      {summary ? (
        <div className="summary">
          <h2>AI-Conjured Summary:</h2>
          <p>{summary}</p>
        </div>
      ) : (
        <p>No content to weave into wisdom</p>
      )}
    </div>
  );
}
```

## Wisdom of the AI Ancients

- **Error Banishment**: Always implement proper error handling for API spirit failures
- **Rate Limiting Shields**: Implement client-side rate limiting to prevent API spirit abuse
- **Message Memory Preservation**: Store chat history in local storage or mystical databases
- **Loading State Divination**: Provide clear loading indicators during AI spirit responses
- **Streaming Consciousness UX**: Use streaming for better user experience with long mystical responses
- **Sacred Security**: Never expose API keys in client-side code - keep them in the server realm
- **Response Caching Magic**: Implement response caching for repeated mystical queries

## Common Curses & Their Remedies

### Curse 1: API Key Exposure to the Mortal Realm
Never include API keys in client-side code where mortals can see them.

**Counter-Spell:**
Always use API routes or server actions to handle AI spirit communication.

### Curse 2: Unhandled Streaming Consciousness Errors
Streaming responses can fail mid-manifestation.

**Banishment Ritual:**
```typescript
const { messages, error } = useChat({
  onError: (error) => {
    console.error('AI spirit disturbance:', error);
    // Handle the mystical error appropriately
  },
});

if (error) {
  return <div>Mystical Error: {error.message}</div>;
}
```

### Curse 3: Memory Drain from Endless Conversations
Long chat sessions can consume excessive mystical memory.

**Memory Cleansing Ritual:**
Implement message pagination or conversation limits:

```typescript
const { messages } = useChat({
  maxToolRoundtrips: 5,
  // Limit conversation length to prevent memory drain
});
```

## Sacred Texts & Mystical Sources

{% embed url="https://sdk.vercel.ai/docs" %}

{% embed url="https://github.com/vercel/ai" %}
