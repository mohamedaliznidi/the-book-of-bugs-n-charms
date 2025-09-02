---
description: >-
  Complete guide to Shadcn Form Builder - an interactive tool for rapidly creating
  complex forms with shadcn/UI, React Hook Form, and validation. Learn drag-and-drop
  form building, code generation, and integration patterns.
---

# Shadcn Form Builder

## Introduction

The Shadcn Form Builder is an interactive tool and open-source library for rapidly creating complex forms with shadcn/UI, React Hook Form, and validation libraries like Zod or Yup. Using a drag-and-drop interface, you can define form fields and settings, and the tool auto-generates React code with built-in validation logic.

## Key Features

- **Drag-and-drop interface**: Visual form builder with intuitive controls
- **Code generation**: Automatically generates React components with validation
- **shadcn/UI integration**: Uses shadcn/UI components for consistent styling
- **React Hook Form**: Built-in form state management and validation
- **Multiple validation**: Support for Zod, Yup, and custom validation schemas
- **TypeScript support**: Generated code includes full TypeScript definitions
- **Copy-paste ready**: Generated components are ready to use in your project

## Use Cases

1. **User Registration**: Complex signup forms with validation rules
2. **Surveys & Questionnaires**: Multi-step forms with conditional logic
3. **Admin Panels**: Data entry forms for content management
4. **Customer Onboarding**: Progressive forms for user setup
5. **Contact Forms**: Professional contact and inquiry forms
6. **Settings Pages**: User preference and configuration forms

## Getting Started

### 1. Access the Form Builder

Visit [shadcn-form.com](https://shadcn-form.com) to access the interactive form builder.

### 2. Prerequisites for Generated Code

Ensure your Next.js project has the required dependencies:

```bash
# Create Next.js project with shadcn/ui
pnpm create next-app@latest my-app --typescript --tailwind --eslint
cd my-app
pnpm dlx shadcn-ui@latest init

# Install form dependencies
pnpm add react-hook-form @hookform/resolvers zod
pnpm dlx shadcn-ui@latest add form input label button textarea select checkbox
```

### 3. Using the Form Builder

1. **Add Fields**: Drag form elements from the sidebar
2. **Configure Properties**: Set labels, validation rules, and options
3. **Preview Form**: See real-time preview of your form
4. **Generate Code**: Copy the generated React component
5. **Integrate**: Paste into your Next.js project

## Form Builder Interface

### Available Form Elements

- **Text Input**: Single-line text fields with validation
- **Textarea**: Multi-line text input for longer content
- **Select**: Dropdown menus with custom options
- **Checkbox**: Boolean inputs for agreements and preferences
- **Radio Group**: Single-choice selection from multiple options
- **Date Picker**: Date selection with calendar interface
- **Number Input**: Numeric inputs with min/max validation
- **Email Input**: Email fields with built-in validation
- **Password Input**: Secure password fields with strength indicators

### Configuration Options

Each form element can be configured with:

- **Label**: Display text for the field
- **Placeholder**: Hint text for user guidance
- **Validation Rules**: Required, min/max length, patterns
- **Default Values**: Pre-populated field values
- **Help Text**: Additional guidance for users
- **Conditional Logic**: Show/hide based on other field values

## Generated Code Examples

### Basic Contact Form

Generated code for a simple contact form:

```typescript
"use client"

import { zodResolver } from "@hookform/resolvers/zod"
import { useForm } from "react-hook-form"
import * as z from "zod"
import { Button } from "@/components/ui/button"
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form"
import { Input } from "@/components/ui/input"
import { Textarea } from "@/components/ui/textarea"

const formSchema = z.object({
  name: z.string().min(2, {
    message: "Name must be at least 2 characters.",
  }),
  email: z.string().email({
    message: "Please enter a valid email address.",
  }),
  subject: z.string().min(5, {
    message: "Subject must be at least 5 characters.",
  }),
  message: z.string().min(10, {
    message: "Message must be at least 10 characters.",
  }),
})

export default function ContactForm() {
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      name: "",
      email: "",
      subject: "",
      message: "",
    },
  })

  function onSubmit(values: z.infer<typeof formSchema>) {
    console.log(values)
    // Handle form submission here
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-6">
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Full Name</FormLabel>
              <FormControl>
                <Input placeholder="Enter your full name" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Email Address</FormLabel>
              <FormControl>
                <Input type="email" placeholder="Enter your email" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        
        <FormField
          control={form.control}
          name="subject"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Subject</FormLabel>
              <FormControl>
                <Input placeholder="What is this about?" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        
        <FormField
          control={form.control}
          name="message"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Message</FormLabel>
              <FormControl>
                <Textarea 
                  placeholder="Tell us more about your inquiry"
                  className="min-h-[120px]"
                  {...field} 
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        
        <Button type="submit" className="w-full">
          Send Message
        </Button>
      </form>
    </Form>
  )
}
```

### Multi-Step Registration Form

Generated code for a complex registration form:

```typescript
"use client"

import { useState } from "react"
import { zodResolver } from "@hookform/resolvers/zod"
import { useForm } from "react-hook-form"
import * as z from "zod"
import { Button } from "@/components/ui/button"
import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from "@/components/ui/form"
import { Input } from "@/components/ui/input"
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select"
import { Checkbox } from "@/components/ui/checkbox"
import { Progress } from "@/components/ui/progress"

const registrationSchema = z.object({
  // Step 1: Personal Info
  firstName: z.string().min(1, "First name is required"),
  lastName: z.string().min(1, "Last name is required"),
  email: z.string().email("Invalid email address"),
  
  // Step 2: Account Details
  username: z.string().min(3, "Username must be at least 3 characters"),
  password: z.string().min(8, "Password must be at least 8 characters"),
  confirmPassword: z.string(),
  
  // Step 3: Preferences
  role: z.string().min(1, "Please select a role"),
  notifications: z.boolean().default(false),
  terms: z.boolean().refine(val => val === true, "You must accept the terms"),
}).refine((data) => data.password === data.confirmPassword, {
  message: "Passwords don't match",
  path: ["confirmPassword"],
})

export default function RegistrationForm() {
  const [step, setStep] = useState(1)
  const totalSteps = 3

  const form = useForm<z.infer<typeof registrationSchema>>({
    resolver: zodResolver(registrationSchema),
    defaultValues: {
      firstName: "",
      lastName: "",
      email: "",
      username: "",
      password: "",
      confirmPassword: "",
      role: "",
      notifications: false,
      terms: false,
    },
  })

  function onSubmit(values: z.infer<typeof registrationSchema>) {
    console.log(values)
    // Handle registration logic
  }

  const nextStep = () => setStep(Math.min(step + 1, totalSteps))
  const prevStep = () => setStep(Math.max(step - 1, 1))

  return (
    <div className="max-w-md mx-auto">
      <div className="mb-8">
        <Progress value={(step / totalSteps) * 100} className="mb-2" />
        <p className="text-sm text-gray-600">Step {step} of {totalSteps}</p>
      </div>

      <Form {...form}>
        <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-6">
          {step === 1 && (
            <>
              <h2 className="text-2xl font-bold mb-4">Personal Information</h2>
              <FormField
                control={form.control}
                name="firstName"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>First Name</FormLabel>
                    <FormControl>
                      <Input {...field} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="lastName"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Last Name</FormLabel>
                    <FormControl>
                      <Input {...field} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="email"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Email</FormLabel>
                    <FormControl>
                      <Input type="email" {...field} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
            </>
          )}

          {step === 2 && (
            <>
              <h2 className="text-2xl font-bold mb-4">Account Details</h2>
              <FormField
                control={form.control}
                name="username"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Username</FormLabel>
                    <FormControl>
                      <Input {...field} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="password"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Password</FormLabel>
                    <FormControl>
                      <Input type="password" {...field} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="confirmPassword"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Confirm Password</FormLabel>
                    <FormControl>
                      <Input type="password" {...field} />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
            </>
          )}

          {step === 3 && (
            <>
              <h2 className="text-2xl font-bold mb-4">Preferences</h2>
              <FormField
                control={form.control}
                name="role"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel>Role</FormLabel>
                    <Select onValueChange={field.onChange} defaultValue={field.value}>
                      <FormControl>
                        <SelectTrigger>
                          <SelectValue placeholder="Select your role" />
                        </SelectTrigger>
                      </FormControl>
                      <SelectContent>
                        <SelectItem value="developer">Developer</SelectItem>
                        <SelectItem value="designer">Designer</SelectItem>
                        <SelectItem value="manager">Manager</SelectItem>
                      </SelectContent>
                    </Select>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="notifications"
                render={({ field }) => (
                  <FormItem className="flex flex-row items-start space-x-3 space-y-0">
                    <FormControl>
                      <Checkbox
                        checked={field.value}
                        onCheckedChange={field.onChange}
                      />
                    </FormControl>
                    <div className="space-y-1 leading-none">
                      <FormLabel>Email notifications</FormLabel>
                    </div>
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="terms"
                render={({ field }) => (
                  <FormItem className="flex flex-row items-start space-x-3 space-y-0">
                    <FormControl>
                      <Checkbox
                        checked={field.value}
                        onCheckedChange={field.onChange}
                      />
                    </FormControl>
                    <div className="space-y-1 leading-none">
                      <FormLabel>I accept the terms and conditions</FormLabel>
                    </div>
                    <FormMessage />
                  </FormItem>
                )}
              />
            </>
          )}

          <div className="flex justify-between">
            {step > 1 && (
              <Button type="button" variant="outline" onClick={prevStep}>
                Previous
              </Button>
            )}
            {step < totalSteps ? (
              <Button type="button" onClick={nextStep} className="ml-auto">
                Next
              </Button>
            ) : (
              <Button type="submit" className="ml-auto">
                Register
              </Button>
            )}
          </div>
        </form>
      </Form>
    </div>
  )
}
```

## Advanced Features

### Custom Validation Rules

The form builder supports custom validation patterns:

```typescript
const customSchema = z.object({
  phoneNumber: z.string().regex(/^\+?[1-9]\d{1,14}$/, {
    message: "Please enter a valid phone number",
  }),
  website: z.string().url().optional().or(z.literal("")),
  age: z.number().min(18, "Must be at least 18 years old").max(120),
})
```

### Conditional Field Logic

Show/hide fields based on other field values:

```typescript
const conditionalSchema = z.object({
  accountType: z.enum(["personal", "business"]),
  companyName: z.string().optional(),
}).refine((data) => {
  if (data.accountType === "business" && !data.companyName) {
    return false
  }
  return true
}, {
  message: "Company name is required for business accounts",
  path: ["companyName"],
})
```

## Best Practices

1. **Validation First**: Define clear validation rules before building the form
2. **User Experience**: Use appropriate input types and helpful error messages
3. **Progressive Enhancement**: Break complex forms into logical steps
4. **Accessibility**: Ensure proper labels and ARIA attributes
5. **Performance**: Validate on blur rather than on every keystroke for better UX
6. **Error Handling**: Provide clear, actionable error messages

## Common Pitfalls

### Issue 1: Missing Form Dependencies
Generated forms require specific shadcn/ui components.

**Solution:**
```bash
# Install all required form components
pnpm dlx shadcn-ui@latest add form input label button textarea select checkbox
```

### Issue 2: Validation Schema Conflicts
Complex validation rules may conflict with form state.

**Solution:**
```typescript
// Use refine for complex cross-field validation
const schema = z.object({
  password: z.string().min(8),
  confirmPassword: z.string(),
}).refine((data) => data.password === data.confirmPassword, {
  message: "Passwords don't match",
  path: ["confirmPassword"],
})
```

### Issue 3: Form State Management
Large forms may have performance issues with frequent re-renders.

**Solution:**
```typescript
// Use mode: "onBlur" for better performance
const form = useForm({
  resolver: zodResolver(schema),
  mode: "onBlur", // Validate on blur instead of onChange
})
```

## References

{% embed url="https://shadcn-form.com/" %}

{% embed url="https://react-hook-form.com/" %}

{% embed url="https://zod.dev/" %}
