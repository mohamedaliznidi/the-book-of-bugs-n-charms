---
description: >-
  Complete grimoire for Origin UI - a massive, free collection of React/Tailwind enchantments
  and full-page mystical layouts following shadcn conventions. Master enchantment categories,
  mystical layout templates, and integration patterns for building modern mystical applications.

theme: "magic"
---

# Origin UI - The Comprehensive Layout Mastery

## The Ancient Knowledge

Origin UI is a massive, free collection of React/Tailwind components and full-page layouts that follow the shadcn conventions. It's entirely open-source and covers everything from basic UI widgets to complete layout templates, organized by component type with dozens of examples each.

## Key Features

- **Comprehensive Library**: 500+ components across 35+ categories
- **shadcn/ui Compatible**: Built following shadcn design principles and conventions
- **Complete Layouts**: Full-page templates for dashboards, landing pages, and applications
- **Copy-Paste Ready**: No installation required, just copy the components you need
- **Open Source**: MIT licensed and completely free to use
- **High Quality**: Professional-grade components with attention to detail
- **Responsive Design**: Mobile-first approach with responsive layouts

## Use Cases

1. **Admin Dashboards**: Complete dashboard layouts with charts, tables, and navigation
2. **SaaS Applications**: User interfaces for software-as-a-service products
3. **E-commerce Sites**: Product listings, shopping carts, and checkout flows
4. **Content Management**: CRM systems and data management interfaces
5. **Landing Pages**: Marketing pages with hero sections and feature highlights
6. **AI Chat Interfaces**: Modern chat UIs for AI applications

## Component Categories

### 1. Navigation Components

Professional navigation patterns for web applications:

```typescript
import { Button } from "@/components/ui/button"
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"
import { 
  DropdownMenu, 
  DropdownMenuContent, 
  DropdownMenuItem, 
  DropdownMenuTrigger 
} from "@/components/ui/dropdown-menu"
import { Bell, Search, Settings, LogOut } from "lucide-react"

export default function TopNavigation() {
  return (
    <header className="border-b bg-white">
      <div className="flex h-16 items-center px-4">
        <div className="flex items-center space-x-4">
          <h1 className="text-xl font-semibold">Dashboard</h1>
        </div>
        
        <div className="ml-auto flex items-center space-x-4">
          <Button variant="ghost" size="icon">
            <Search className="h-4 w-4" />
          </Button>
          
          <Button variant="ghost" size="icon">
            <Bell className="h-4 w-4" />
          </Button>
          
          <DropdownMenu>
            <DropdownMenuTrigger asChild>
              <Button variant="ghost" className="relative h-8 w-8 rounded-full">
                <Avatar className="h-8 w-8">
                  <AvatarImage src="/avatar.jpg" alt="User" />
                  <AvatarFallback>JD</AvatarFallback>
                </Avatar>
              </Button>
            </DropdownMenuTrigger>
            <DropdownMenuContent className="w-56" align="end" forceMount>
              <DropdownMenuItem>
                <Settings className="mr-2 h-4 w-4" />
                Settings
              </DropdownMenuItem>
              <DropdownMenuItem>
                <LogOut className="mr-2 h-4 w-4" />
                Log out
              </DropdownMenuItem>
            </DropdownMenuContent>
          </DropdownMenu>
        </div>
      </div>
    </header>
  )
}
```

### 2. Data Display Components

Advanced tables and data visualization components:

```typescript
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table"
import { MoreHorizontal, ArrowUpDown } from "lucide-react"

const users = [
  {
    id: 1,
    name: "John Doe",
    email: "john@example.com",
    role: "Admin",
    status: "Active",
    lastSeen: "2 hours ago"
  },
  {
    id: 2,
    name: "Jane Smith",
    email: "jane@example.com",
    role: "User",
    status: "Inactive",
    lastSeen: "1 day ago"
  },
  // More users...
]

export default function UsersTable() {
  return (
    <div className="rounded-md border">
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead className="w-[100px]">
              <Button variant="ghost" className="h-auto p-0 font-medium">
                Name
                <ArrowUpDown className="ml-2 h-4 w-4" />
              </Button>
            </TableHead>
            <TableHead>Email</TableHead>
            <TableHead>Role</TableHead>
            <TableHead>Status</TableHead>
            <TableHead>Last Seen</TableHead>
            <TableHead className="text-right">Actions</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {users.map((user) => (
            <TableRow key={user.id}>
              <TableCell className="font-medium">{user.name}</TableCell>
              <TableCell>{user.email}</TableCell>
              <TableCell>{user.role}</TableCell>
              <TableCell>
                <Badge variant={user.status === "Active" ? "default" : "secondary"}>
                  {user.status}
                </Badge>
              </TableCell>
              <TableCell>{user.lastSeen}</TableCell>
              <TableCell className="text-right">
                <Button variant="ghost" size="icon">
                  <MoreHorizontal className="h-4 w-4" />
                </Button>
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </div>
  )
}
```

### 3. Dashboard Layouts

Complete dashboard templates with sidebar navigation:

```typescript
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { Progress } from "@/components/ui/progress"
import { 
  BarChart3, 
  Users, 
  DollarSign, 
  Activity,
  Home,
  Settings,
  FileText,
  PieChart
} from "lucide-react"

const sidebarItems = [
  { icon: Home, label: "Dashboard", active: true },
  { icon: Users, label: "Users" },
  { icon: FileText, label: "Reports" },
  { icon: PieChart, label: "Analytics" },
  { icon: Settings, label: "Settings" },
]

export default function DashboardLayout() {
  return (
    <div className="flex h-screen bg-gray-100">
      {/* Sidebar */}
      <div className="w-64 bg-white shadow-sm">
        <div className="p-6">
          <h2 className="text-2xl font-bold">Admin Panel</h2>
        </div>
        <nav className="mt-6">
          {sidebarItems.map((item) => (
            <Button
              key={item.label}
              variant={item.active ? "secondary" : "ghost"}
              className="w-full justify-start px-6 py-3"
            >
              <item.icon className="mr-3 h-4 w-4" />
              {item.label}
            </Button>
          ))}
        </nav>
      </div>

      {/* Main Content */}
      <div className="flex-1 overflow-auto">
        <div className="p-6">
          <h1 className="text-3xl font-bold mb-6">Dashboard Overview</h1>
          
          {/* Stats Cards */}
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-6">
            <Card>
              <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
                <CardTitle className="text-sm font-medium">Total Revenue</CardTitle>
                <DollarSign className="h-4 w-4 text-muted-foreground" />
              </CardHeader>
              <CardContent>
                <div className="text-2xl font-bold">$45,231.89</div>
                <p className="text-xs text-muted-foreground">+20.1% from last month</p>
              </CardContent>
            </Card>
            
            <Card>
              <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
                <CardTitle className="text-sm font-medium">Active Users</CardTitle>
                <Users className="h-4 w-4 text-muted-foreground" />
              </CardHeader>
              <CardContent>
                <div className="text-2xl font-bold">2,350</div>
                <p className="text-xs text-muted-foreground">+180.1% from last month</p>
              </CardContent>
            </Card>
            
            <Card>
              <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
                <CardTitle className="text-sm font-medium">Sales</CardTitle>
                <BarChart3 className="h-4 w-4 text-muted-foreground" />
              </CardHeader>
              <CardContent>
                <div className="text-2xl font-bold">12,234</div>
                <p className="text-xs text-muted-foreground">+19% from last month</p>
              </CardContent>
            </Card>
            
            <Card>
              <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
                <CardTitle className="text-sm font-medium">Active Now</CardTitle>
                <Activity className="h-4 w-4 text-muted-foreground" />
              </CardHeader>
              <CardContent>
                <div className="text-2xl font-bold">573</div>
                <p className="text-xs text-muted-foreground">+201 since last hour</p>
              </CardContent>
            </Card>
          </div>

          {/* Charts and Tables */}
          <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
            <Card>
              <CardHeader>
                <CardTitle>Recent Activity</CardTitle>
                <CardDescription>Your recent activity overview</CardDescription>
              </CardHeader>
              <CardContent>
                <div className="space-y-4">
                  <div className="flex items-center">
                    <div className="w-2 h-2 bg-blue-600 rounded-full mr-3"></div>
                    <div className="flex-1">
                      <p className="text-sm font-medium">New user registered</p>
                      <p className="text-xs text-muted-foreground">2 minutes ago</p>
                    </div>
                  </div>
                  <div className="flex items-center">
                    <div className="w-2 h-2 bg-green-600 rounded-full mr-3"></div>
                    <div className="flex-1">
                      <p className="text-sm font-medium">Payment received</p>
                      <p className="text-xs text-muted-foreground">5 minutes ago</p>
                    </div>
                  </div>
                  <div className="flex items-center">
                    <div className="w-2 h-2 bg-yellow-600 rounded-full mr-3"></div>
                    <div className="flex-1">
                      <p className="text-sm font-medium">System update completed</p>
                      <p className="text-xs text-muted-foreground">1 hour ago</p>
                    </div>
                  </div>
                </div>
              </CardContent>
            </Card>

            <Card>
              <CardHeader>
                <CardTitle>Progress Overview</CardTitle>
                <CardDescription>Current project status</CardDescription>
              </CardHeader>
              <CardContent className="space-y-4">
                <div>
                  <div className="flex justify-between text-sm mb-2">
                    <span>Website Redesign</span>
                    <span>75%</span>
                  </div>
                  <Progress value={75} />
                </div>
                <div>
                  <div className="flex justify-between text-sm mb-2">
                    <span>Mobile App</span>
                    <span>45%</span>
                  </div>
                  <Progress value={45} />
                </div>
                <div>
                  <div className="flex justify-between text-sm mb-2">
                    <span>API Development</span>
                    <span>90%</span>
                  </div>
                  <Progress value={90} />
                </div>
              </CardContent>
            </Card>
          </div>
        </div>
      </div>
    </div>
  )
}
```

## Integration with APIs

### Real-time Dashboard Example

Connect Origin UI components with live data:

```typescript
"use client"

import { useEffect, useState } from "react"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"

interface ApiData {
  users: number
  revenue: number
  orders: number
  growth: number
}

export default function LiveDashboard() {
  const [data, setData] = useState<ApiData | null>(null)
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    // Simulate API call - replace with your actual API
    const fetchData = async () => {
      try {
        // Example: fetch from a mock API or your backend
        const response = await fetch('/api/dashboard-stats')
        const result = await response.json()
        setData(result)
      } catch (error) {
        // Fallback to mock data
        setData({
          users: Math.floor(Math.random() * 10000),
          revenue: Math.floor(Math.random() * 100000),
          orders: Math.floor(Math.random() * 1000),
          growth: Math.floor(Math.random() * 100)
        })
      } finally {
        setLoading(false)
      }
    }

    fetchData()
    // Refresh every 30 seconds
    const interval = setInterval(fetchData, 30000)
    return () => clearInterval(interval)
  }, [])

  if (loading) {
    return <div className="text-center py-12">Loading dashboard...</div>
  }

  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <Card>
        <CardHeader>
          <CardTitle className="text-sm font-medium">Total Users</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">{data?.users.toLocaleString()}</div>
          <Badge variant="secondary" className="mt-2">
            Live Data
          </Badge>
        </CardContent>
      </Card>

      <Card>
        <CardHeader>
          <CardTitle className="text-sm font-medium">Revenue</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">${data?.revenue.toLocaleString()}</div>
          <p className="text-xs text-muted-foreground">+{data?.growth}% this month</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader>
          <CardTitle className="text-sm font-medium">Orders</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">{data?.orders.toLocaleString()}</div>
          <Badge variant="outline" className="mt-2">
            Updated now
          </Badge>
        </CardContent>
      </Card>

      <Card>
        <CardHeader>
          <CardTitle className="text-sm font-medium">Growth Rate</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">{data?.growth}%</div>
          <p className="text-xs text-green-600">↗ Trending up</p>
        </CardContent>
      </Card>
    </div>
  )
}
```

## Best Practices

1. **Component Organization**: Group related components in logical folders
2. **Consistent Styling**: Use shadcn/ui design tokens for consistency
3. **Responsive Design**: Test components on various screen sizes
4. **Performance**: Lazy load heavy components and optimize re-renders
5. **Accessibility**: Ensure proper ARIA labels and keyboard navigation
6. **Data Integration**: Plan for loading states and error handling

## Common Pitfalls

### Issue 1: Missing shadcn/ui Components
Origin UI components may require specific shadcn/ui dependencies.

**Solution:**
```bash
# Install commonly required components
pnpm dlx shadcn-ui@latest add card button badge table progress avatar dropdown-menu
```

### Issue 2: Layout Responsiveness
Complex layouts may break on smaller screens.

**Solution:**
```typescript
// Use responsive grid classes
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
  {/* Components */}
</div>
```

### Issue 3: Performance with Large Datasets
Tables and lists with many items can cause performance issues.

**Solution:**
```typescript
// Implement pagination or virtualization
import { useMemo } from "react"

const paginatedData = useMemo(() => {
  const startIndex = (currentPage - 1) * itemsPerPage
  return data.slice(startIndex, startIndex + itemsPerPage)
}, [data, currentPage, itemsPerPage])
```

## References

{% embed url="https://originui.com/" %}

{% embed url="https://github.com/shadcn-ui/ui" %}

{% embed url="https://tailwindcss.com/" %}
