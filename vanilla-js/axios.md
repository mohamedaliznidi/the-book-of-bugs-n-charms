---
description: >-
  Axios is a promise-based HTTP client for JavaScript that works in browsers and Node.js,
  providing an easy-to-use API for making HTTP requests with features like interceptors and automatic JSON parsing.

theme: "magic"
---

# The Axios HTTP Communication Sorcery Grimoire

## The Ancient Knowledge

Axios is a popular promise-based HTTP client for JavaScript that works in both browsers and Node.js environments. It provides a simple and intuitive API for making HTTP requests through mystical communication spells, with built-in support for request and response interceptors, automatic JSON data transformation, request and response timeouts, and comprehensive error handling enchantments.

## When to Cast These Spells

1. **API Communication Rituals**: Make REST API calls to backend services through mystical interfaces
2. **Data Fetching Enchantments**: Retrieve data from external APIs and services using powerful summoning spells
3. **Form Submission Sorcery**: Send form data and file uploads to servers through secure transmission magic
4. **Authentication Mysticism**: Handle authentication tokens and headers with protective enchantments
5. **Error Handling Divination**: Centralized error handling for HTTP requests with prophetic insights
6. **Request/Response Transformation**: Modify requests and responses globally through data manipulation spells

## Summoning the HTTP Spirits

### CDN Invocation

```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script>
    // Axios is available globally
    axios.get('https://api.example.com/data')
        .then(response => console.log(response.data))
        .catch(error => console.error(error));
</script>
```

### NPM Ritual

```bash
npm install axios
```

```javascript
// ES6 Modules
import axios from 'axios';

// CommonJS
const axios = require('axios');

// Basic usage
axios.get('https://api.example.com/data')
    .then(response => {
        console.log(response.data);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```

## Basic HTTP Communication Rituals

### GET Request Summoning Spells

```javascript
// Simple GET request
axios.get('https://jsonplaceholder.typicode.com/posts/1')
    .then(response => {
        console.log('Data:', response.data);
        console.log('Status:', response.status);
        console.log('Headers:', response.headers);
    })
    .catch(error => {
        console.error('Error:', error);
    });

// GET with query parameters
axios.get('https://jsonplaceholder.typicode.com/posts', {
    params: {
        userId: 1,
        _limit: 5
    }
})
.then(response => {
    console.log('Posts:', response.data);
});

// GET with custom headers
axios.get('https://api.example.com/data', {
    headers: {
        'Authorization': 'Bearer your-token-here',
        'Content-Type': 'application/json'
    }
})
.then(response => {
    console.log('Protected data:', response.data);
});

// Using async/await
async function fetchData() {
    try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts/1');
        console.log('Data:', response.data);
        return response.data;
    } catch (error) {
        console.error('Error fetching data:', error);
        throw error;
    }
}
```

### POST Request Manifestation Sorcery

```javascript
// Simple POST request
const newPost = {
    title: 'My New Post',
    body: 'This is the content of my post',
    userId: 1
};

axios.post('https://jsonplaceholder.typicode.com/posts', newPost)
    .then(response => {
        console.log('Created post:', response.data);
        console.log('Status:', response.status);
    })
    .catch(error => {
        console.error('Error creating post:', error);
    });

// POST with custom headers
axios.post('https://api.example.com/users', {
    name: 'John Doe',
    email: 'john@example.com'
}, {
    headers: {
        'Authorization': 'Bearer your-token-here',
        'Content-Type': 'application/json'
    }
})
.then(response => {
    console.log('User created:', response.data);
});

// POST form data
const formData = new FormData();
formData.append('name', 'John Doe');
formData.append('email', 'john@example.com');
formData.append('avatar', fileInput.files[0]);

axios.post('https://api.example.com/users', formData, {
    headers: {
        'Content-Type': 'multipart/form-data'
    }
})
.then(response => {
    console.log('User with avatar created:', response.data);
});

// POST with URL-encoded data
const params = new URLSearchParams();
params.append('name', 'John Doe');
params.append('email', 'john@example.com');

axios.post('https://api.example.com/users', params, {
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    }
})
.then(response => {
    console.log('User created:', response.data);
});
```

### PUT and PATCH Request Transformation Spells

```javascript
// PUT request (full update)
const updatedPost = {
    id: 1,
    title: 'Updated Post Title',
    body: 'Updated post content',
    userId: 1
};

axios.put('https://jsonplaceholder.typicode.com/posts/1', updatedPost)
    .then(response => {
        console.log('Updated post:', response.data);
    })
    .catch(error => {
        console.error('Error updating post:', error);
    });

// PATCH request (partial update)
const partialUpdate = {
    title: 'New Title Only'
};

axios.patch('https://jsonplaceholder.typicode.com/posts/1', partialUpdate)
    .then(response => {
        console.log('Partially updated post:', response.data);
    })
    .catch(error => {
        console.error('Error updating post:', error);
    });

// PUT with authentication
axios.put('https://api.example.com/users/123', {
    name: 'Jane Doe',
    email: 'jane@example.com'
}, {
    headers: {
        'Authorization': 'Bearer your-token-here'
    }
})
.then(response => {
    console.log('User updated:', response.data);
});
```

### DELETE Request Banishment Spells

```javascript
// Simple DELETE request
axios.delete('https://jsonplaceholder.typicode.com/posts/1')
    .then(response => {
        console.log('Post deleted:', response.status);
    })
    .catch(error => {
        console.error('Error deleting post:', error);
    });

// DELETE with authentication
axios.delete('https://api.example.com/users/123', {
    headers: {
        'Authorization': 'Bearer your-token-here'
    }
})
.then(response => {
    console.log('User deleted:', response.status);
});

// DELETE with confirmation
async function deleteUser(userId) {
    try {
        const confirmation = confirm('Are you sure you want to delete this user?');
        if (!confirmation) return;

        const response = await axios.delete(`https://api.example.com/users/${userId}`, {
            headers: {
                'Authorization': 'Bearer your-token-here'
            }
        });

        console.log('User deleted successfully');
        return response;
    } catch (error) {
        console.error('Error deleting user:', error);
        throw error;
    }
}
```

## Configuration and Instance Mysticism

### Global Configuration Ritual Spells

```javascript
// Set global defaults
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = 'Bearer your-token-here';
axios.defaults.headers.post['Content-Type'] = 'application/json';
axios.defaults.timeout = 10000; // 10 seconds

// Now all requests will use these defaults
axios.get('/users') // Actually requests https://api.example.com/users
    .then(response => console.log(response.data));

// Override defaults for specific request
axios.get('/users', {
    timeout: 5000, // Override global timeout
    headers: {
        'Custom-Header': 'custom-value'
    }
})
.then(response => console.log(response.data));
```

### Creating Mystical Instance Conjurations

```javascript
// Create custom instance with specific configuration
const apiClient = axios.create({
    baseURL: 'https://api.example.com',
    timeout: 10000,
    headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
    }
});

// Add authentication token to instance
apiClient.defaults.headers.common['Authorization'] = 'Bearer your-token-here';

// Use the instance
apiClient.get('/users')
    .then(response => console.log(response.data));

apiClient.post('/users', {
    name: 'John Doe',
    email: 'john@example.com'
})
.then(response => console.log(response.data));

// Create multiple instances for different APIs
const authAPI = axios.create({
    baseURL: 'https://auth.example.com',
    timeout: 5000
});

const dataAPI = axios.create({
    baseURL: 'https://data.example.com',
    timeout: 15000,
    headers: {
        'API-Key': 'your-api-key'
    }
});

// Use different instances
authAPI.post('/login', { username, password });
dataAPI.get('/analytics');
```

## Request and Response Interceptor Mysticism

### Request Interceptor Guardian Spells

```javascript
// Add request interceptor
axios.interceptors.request.use(
    config => {
        // Do something before request is sent
        console.log('Sending request:', config);

        // Add timestamp to all requests
        config.metadata = { startTime: new Date() };

        // Add auth token if available
        const token = localStorage.getItem('authToken');
        if (token) {
            config.headers.Authorization = `Bearer ${token}`;
        }

        // Add loading indicator
        showLoadingSpinner();

        return config;
    },
    error => {
        // Do something with request error
        console.error('Request error:', error);
        hideLoadingSpinner();
        return Promise.reject(error);
    }
);

// Multiple request interceptors
axios.interceptors.request.use(config => {
    // Add API version header
    config.headers['API-Version'] = '1.0';
    return config;
});

axios.interceptors.request.use(config => {
    // Add request ID for tracking
    config.headers['Request-ID'] = generateRequestId();
    return config;
});
```

### Response Interceptor Oracle Enchantments

```javascript
// Add response interceptor
axios.interceptors.response.use(
    response => {
        // Do something with response data
        console.log('Received response:', response);
        
        // Calculate request duration
        if (response.config.metadata) {
            const duration = new Date() - response.config.metadata.startTime;
            console.log(`Request took ${duration}ms`);
        }
        
        // Hide loading indicator
        hideLoadingSpinner();
        
        // Transform response data if needed
        if (response.data && response.data.data) {
            response.data = response.data.data; // Unwrap nested data
        }
        
        return response;
    },
    error => {
        // Handle response errors
        console.error('Response error:', error);
        
        hideLoadingSpinner();
        
        // Handle specific error cases
        if (error.response) {
            const { status, data } = error.response;
            
            switch (status) {
                case 401:
                    // Unauthorized - redirect to login
                    localStorage.removeItem('authToken');
                    window.location.href = '/login';
                    break;
                case 403:
                    // Forbidden
                    showNotification('Access denied', 'error');
                    break;
                case 404:
                    // Not found
                    showNotification('Resource not found', 'error');
                    break;
                case 500:
                    // Server error
                    showNotification('Server error. Please try again later.', 'error');
                    break;
                default:
                    showNotification(data.message || 'An error occurred', 'error');
            }
        } else if (error.request) {
            // Network error
            showNotification('Network error. Please check your connection.', 'error');
        }
        
        return Promise.reject(error);
    }
);

// Remove interceptor if needed
const interceptorId = axios.interceptors.request.use(config => config);
axios.interceptors.request.eject(interceptorId);
```

### Instance-specific Interceptor Bindings

```javascript
// Create instance with interceptors
const apiClient = axios.create({
    baseURL: 'https://api.example.com'
});

// Add interceptors to specific instance
apiClient.interceptors.request.use(config => {
    // Instance-specific request handling
    config.headers['Client-Version'] = '1.2.3';
    return config;
});

apiClient.interceptors.response.use(
    response => {
        // Instance-specific response handling
        return response;
    },
    error => {
        // Instance-specific error handling
        if (error.response?.status === 429) {
            // Handle rate limiting for this API
            return retryAfterDelay(error.config, 1000);
        }
        return Promise.reject(error);
    }
);

## Error Handling Protective Enchantments

### Understanding Axios Error Manifestations

```javascript
// Comprehensive error handling
async function makeRequest() {
    try {
        const response = await axios.get('https://api.example.com/data');
        return response.data;
    } catch (error) {
        if (error.response) {
            // Server responded with error status
            console.error('Response Error:');
            console.error('Status:', error.response.status);
            console.error('Data:', error.response.data);
            console.error('Headers:', error.response.headers);

            // Handle specific status codes
            switch (error.response.status) {
                case 400:
                    throw new Error('Bad Request: ' + error.response.data.message);
                case 401:
                    throw new Error('Unauthorized: Please log in');
                case 403:
                    throw new Error('Forbidden: Access denied');
                case 404:
                    throw new Error('Not Found: Resource does not exist');
                case 500:
                    throw new Error('Server Error: Please try again later');
                default:
                    throw new Error(`HTTP Error: ${error.response.status}`);
            }
        } else if (error.request) {
            // Request was made but no response received
            console.error('Request Error:', error.request);
            throw new Error('Network Error: No response from server');
        } else {
            // Something else happened
            console.error('Error:', error.message);
            throw new Error('Request Setup Error: ' + error.message);
        }
    }
}
```

### Custom Error Class Conjurations

```javascript
// Custom error classes for better error handling
class APIError extends Error {
    constructor(message, status, data) {
        super(message);
        this.name = 'APIError';
        this.status = status;
        this.data = data;
    }
}

class NetworkError extends Error {
    constructor(message) {
        super(message);
        this.name = 'NetworkError';
    }
}

class ValidationError extends Error {
    constructor(message, errors) {
        super(message);
        this.name = 'ValidationError';
        this.errors = errors;
    }
}

// Enhanced error handling function
function handleAxiosError(error) {
    if (error.response) {
        const { status, data } = error.response;

        switch (status) {
            case 400:
                if (data.errors) {
                    throw new ValidationError('Validation failed', data.errors);
                }
                throw new APIError(data.message || 'Bad Request', status, data);
            case 401:
                throw new APIError('Authentication required', status, data);
            case 403:
                throw new APIError('Access forbidden', status, data);
            case 404:
                throw new APIError('Resource not found', status, data);
            case 422:
                throw new ValidationError('Validation failed', data.errors);
            case 500:
                throw new APIError('Internal server error', status, data);
            default:
                throw new APIError(`HTTP Error ${status}`, status, data);
        }
    } else if (error.request) {
        throw new NetworkError('Network error - no response received');
    } else {
        throw new Error('Request configuration error: ' + error.message);
    }
}

// Usage with custom error handling
async function fetchUserData(userId) {
    try {
        const response = await axios.get(`/api/users/${userId}`);
        return response.data;
    } catch (error) {
        handleAxiosError(error);
    }
}

// Using the custom errors
fetchUserData(123)
    .then(user => console.log('User:', user))
    .catch(error => {
        if (error instanceof ValidationError) {
            console.error('Validation errors:', error.errors);
        } else if (error instanceof APIError) {
            console.error(`API Error ${error.status}:`, error.message);
        } else if (error instanceof NetworkError) {
            console.error('Network issue:', error.message);
        } else {
            console.error('Unexpected error:', error.message);
        }
    });
```

### Retry Logic Persistence Spells

```javascript
// Retry function with exponential backoff
async function retryRequest(requestFn, maxRetries = 3, baseDelay = 1000) {
    let lastError;

    for (let attempt = 1; attempt <= maxRetries; attempt++) {
        try {
            return await requestFn();
        } catch (error) {
            lastError = error;

            // Don't retry on client errors (4xx)
            if (error.response && error.response.status >= 400 && error.response.status < 500) {
                throw error;
            }

            if (attempt === maxRetries) {
                break;
            }

            // Exponential backoff
            const delay = baseDelay * Math.pow(2, attempt - 1);
            console.log(`Request failed, retrying in ${delay}ms... (attempt ${attempt}/${maxRetries})`);
            await new Promise(resolve => setTimeout(resolve, delay));
        }
    }

    throw lastError;
}

// Usage
async function fetchDataWithRetry() {
    return retryRequest(
        () => axios.get('https://api.example.com/unreliable-endpoint'),
        3, // max retries
        1000 // base delay in ms
    );
}

// Axios interceptor with retry logic
axios.interceptors.response.use(
    response => response,
    async error => {
        const config = error.config;

        // Don't retry if already retried max times
        if (!config || config.__retryCount >= 3) {
            return Promise.reject(error);
        }

        // Don't retry on client errors
        if (error.response && error.response.status >= 400 && error.response.status < 500) {
            return Promise.reject(error);
        }

        // Increment retry count
        config.__retryCount = config.__retryCount || 0;
        config.__retryCount++;

        // Calculate delay
        const delay = Math.pow(2, config.__retryCount) * 1000;

        console.log(`Retrying request in ${delay}ms...`);

        // Wait and retry
        await new Promise(resolve => setTimeout(resolve, delay));
        return axios(config);
    }
);

## Advanced Mystical Features

### Request and Response Transformation Alchemy

```javascript
// Custom request transformer
axios.defaults.transformRequest = [
    function (data, headers) {
        // Transform request data
        if (data && typeof data === 'object') {
            // Add timestamp to all requests
            data.timestamp = new Date().toISOString();

            // Convert camelCase to snake_case
            data = convertKeysToSnakeCase(data);
        }

        return JSON.stringify(data);
    }
];

// Custom response transformer
axios.defaults.transformResponse = [
    function (data) {
        // Transform response data
        try {
            const parsed = JSON.parse(data);

            // Convert snake_case to camelCase
            return convertKeysToCamelCase(parsed);
        } catch (error) {
            return data;
        }
    }
];

// Helper functions for case conversion
function convertKeysToSnakeCase(obj) {
    if (Array.isArray(obj)) {
        return obj.map(convertKeysToSnakeCase);
    } else if (obj !== null && typeof obj === 'object') {
        return Object.keys(obj).reduce((result, key) => {
            const snakeKey = key.replace(/[A-Z]/g, letter => `_${letter.toLowerCase()}`);
            result[snakeKey] = convertKeysToSnakeCase(obj[key]);
            return result;
        }, {});
    }
    return obj;
}

function convertKeysToCamelCase(obj) {
    if (Array.isArray(obj)) {
        return obj.map(convertKeysToCamelCase);
    } else if (obj !== null && typeof obj === 'object') {
        return Object.keys(obj).reduce((result, key) => {
            const camelKey = key.replace(/_([a-z])/g, (match, letter) => letter.toUpperCase());
            result[camelKey] = convertKeysToCamelCase(obj[key]);
            return result;
        }, {});
    }
    return obj;
}
```

### Request Cancellation Banishment Rituals

```javascript
// Using AbortController (modern approach)
const controller = new AbortController();

axios.get('https://api.example.com/data', {
    signal: controller.signal
})
.then(response => {
    console.log('Data:', response.data);
})
.catch(error => {
    if (axios.isCancel(error)) {
        console.log('Request cancelled:', error.message);
    } else {
        console.error('Error:', error);
    }
});

// Cancel the request
setTimeout(() => {
    controller.abort('Request timeout');
}, 5000);

// Using Axios CancelToken (legacy approach)
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('https://api.example.com/data', {
    cancelToken: source.token
})
.then(response => {
    console.log('Data:', response.data);
})
.catch(error => {
    if (axios.isCancel(error)) {
        console.log('Request cancelled:', error.message);
    } else {
        console.error('Error:', error);
    }
});

// Cancel the request
source.cancel('Operation cancelled by user');

// Practical example: Search with cancellation
class SearchManager {
    constructor() {
        this.currentController = null;
    }

    async search(query) {
        // Cancel previous request if still pending
        if (this.currentController) {
            this.currentController.abort('New search initiated');
        }

        // Create new controller for this request
        this.currentController = new AbortController();

        try {
            const response = await axios.get('/api/search', {
                params: { q: query },
                signal: this.currentController.signal
            });

            this.currentController = null;
            return response.data;
        } catch (error) {
            if (axios.isCancel(error)) {
                console.log('Search cancelled');
                return null;
            }
            throw error;
        }
    }
}

const searchManager = new SearchManager();

// Usage in search input
document.getElementById('search-input').addEventListener('input', async (e) => {
    const query = e.target.value;
    if (query.length > 2) {
        try {
            const results = await searchManager.search(query);
            if (results) {
                displaySearchResults(results);
            }
        } catch (error) {
            console.error('Search error:', error);
        }
    }
});
```

### File Upload with Progress Manifestation

```javascript
// File upload with progress tracking
async function uploadFile(file, onProgress) {
    const formData = new FormData();
    formData.append('file', file);
    formData.append('description', 'Uploaded file');

    try {
        const response = await axios.post('/api/upload', formData, {
            headers: {
                'Content-Type': 'multipart/form-data'
            },
            onUploadProgress: (progressEvent) => {
                const percentCompleted = Math.round(
                    (progressEvent.loaded * 100) / progressEvent.total
                );

                if (onProgress) {
                    onProgress(percentCompleted);
                }

                console.log(`Upload progress: ${percentCompleted}%`);
            }
        });

        return response.data;
    } catch (error) {
        console.error('Upload failed:', error);
        throw error;
    }
}

// Usage with progress bar
const fileInput = document.getElementById('file-input');
const progressBar = document.getElementById('progress-bar');
const uploadButton = document.getElementById('upload-button');

uploadButton.addEventListener('click', async () => {
    const file = fileInput.files[0];
    if (!file) {
        alert('Please select a file');
        return;
    }

    try {
        uploadButton.disabled = true;
        progressBar.style.display = 'block';

        const result = await uploadFile(file, (progress) => {
            progressBar.style.width = `${progress}%`;
            progressBar.textContent = `${progress}%`;
        });

        console.log('Upload successful:', result);
        alert('File uploaded successfully!');
    } catch (error) {
        alert('Upload failed: ' + error.message);
    } finally {
        uploadButton.disabled = false;
        progressBar.style.display = 'none';
    }
});

## Practical Communication Manifestations

### API Client Class Conjuration

```javascript
class APIClient {
    constructor(baseURL, options = {}) {
        this.client = axios.create({
            baseURL,
            timeout: options.timeout || 10000,
            headers: {
                'Content-Type': 'application/json',
                ...options.headers
            }
        });

        this.setupInterceptors();
    }

    setupInterceptors() {
        // Request interceptor
        this.client.interceptors.request.use(
            config => {
                const token = this.getAuthToken();
                if (token) {
                    config.headers.Authorization = `Bearer ${token}`;
                }
                return config;
            },
            error => Promise.reject(error)
        );

        // Response interceptor
        this.client.interceptors.response.use(
            response => response,
            async error => {
                if (error.response?.status === 401) {
                    await this.handleUnauthorized();
                }
                return Promise.reject(error);
            }
        );
    }

    getAuthToken() {
        return localStorage.getItem('authToken');
    }

    setAuthToken(token) {
        localStorage.setItem('authToken', token);
    }

    async handleUnauthorized() {
        localStorage.removeItem('authToken');
        window.location.href = '/login';
    }

    // Generic CRUD operations
    async get(endpoint, params = {}) {
        const response = await this.client.get(endpoint, { params });
        return response.data;
    }

    async post(endpoint, data = {}) {
        const response = await this.client.post(endpoint, data);
        return response.data;
    }

    async put(endpoint, data = {}) {
        const response = await this.client.put(endpoint, data);
        return response.data;
    }

    async patch(endpoint, data = {}) {
        const response = await this.client.patch(endpoint, data);
        return response.data;
    }

    async delete(endpoint) {
        const response = await this.client.delete(endpoint);
        return response.data;
    }

    // Specialized methods
    async login(credentials) {
        const response = await this.post('/auth/login', credentials);
        if (response.token) {
            this.setAuthToken(response.token);
        }
        return response;
    }

    async logout() {
        try {
            await this.post('/auth/logout');
        } finally {
            localStorage.removeItem('authToken');
        }
    }

    async uploadFile(file, onProgress) {
        const formData = new FormData();
        formData.append('file', file);

        const response = await this.client.post('/upload', formData, {
            headers: {
                'Content-Type': 'multipart/form-data'
            },
            onUploadProgress: onProgress
        });

        return response.data;
    }
}

// Usage
const api = new APIClient('https://api.example.com');

// Login
await api.login({ username: 'user', password: 'pass' });

// Fetch data
const users = await api.get('/users', { page: 1, limit: 10 });

// Create user
const newUser = await api.post('/users', { name: 'John', email: 'john@example.com' });

// Update user
const updatedUser = await api.put('/users/123', { name: 'Jane' });

// Delete user
await api.delete('/users/123');
```

### Authentication Manager Enchantments

```javascript
class AuthManager {
    constructor(apiClient) {
        this.api = apiClient;
        this.refreshPromise = null;
    }

    async login(credentials) {
        try {
            const response = await this.api.post('/auth/login', credentials);

            this.setTokens(response.accessToken, response.refreshToken);
            this.scheduleTokenRefresh(response.expiresIn);

            return response.user;
        } catch (error) {
            throw new Error('Login failed: ' + error.message);
        }
    }

    async refreshToken() {
        // Prevent multiple simultaneous refresh attempts
        if (this.refreshPromise) {
            return this.refreshPromise;
        }

        this.refreshPromise = this._performTokenRefresh();

        try {
            const result = await this.refreshPromise;
            return result;
        } finally {
            this.refreshPromise = null;
        }
    }

    async _performTokenRefresh() {
        const refreshToken = localStorage.getItem('refreshToken');

        if (!refreshToken) {
            throw new Error('No refresh token available');
        }

        try {
            const response = await this.api.post('/auth/refresh', {
                refreshToken
            });

            this.setTokens(response.accessToken, response.refreshToken);
            this.scheduleTokenRefresh(response.expiresIn);

            return response.accessToken;
        } catch (error) {
            this.logout();
            throw new Error('Token refresh failed');
        }
    }

    setTokens(accessToken, refreshToken) {
        localStorage.setItem('accessToken', accessToken);
        localStorage.setItem('refreshToken', refreshToken);

        // Update API client headers
        this.api.client.defaults.headers.common['Authorization'] = `Bearer ${accessToken}`;
    }

    scheduleTokenRefresh(expiresIn) {
        // Refresh token 5 minutes before expiry
        const refreshTime = (expiresIn - 300) * 1000;

        setTimeout(() => {
            this.refreshToken().catch(console.error);
        }, refreshTime);
    }

    logout() {
        localStorage.removeItem('accessToken');
        localStorage.removeItem('refreshToken');
        delete this.api.client.defaults.headers.common['Authorization'];

        // Redirect to login page
        window.location.href = '/login';
    }

    isAuthenticated() {
        return !!localStorage.getItem('accessToken');
    }

    getUser() {
        const token = localStorage.getItem('accessToken');
        if (!token) return null;

        try {
            // Decode JWT token (simplified)
            const payload = JSON.parse(atob(token.split('.')[1]));
            return payload.user;
        } catch (error) {
            return null;
        }
    }
}

// Setup with automatic token refresh
const api = new APIClient('https://api.example.com');
const auth = new AuthManager(api);

// Add interceptor to handle token refresh
api.client.interceptors.response.use(
    response => response,
    async error => {
        if (error.response?.status === 401 && auth.isAuthenticated()) {
            try {
                await auth.refreshToken();
                // Retry the original request
                return api.client.request(error.config);
            } catch (refreshError) {
                auth.logout();
                return Promise.reject(refreshError);
            }
        }
        return Promise.reject(error);
    }
);
```

### Data Fetching Hook Enchantments (React-style)

```javascript
// Custom hook for data fetching with Axios
function useAxios(url, options = {}) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        let cancelled = false;
        const controller = new AbortController();

        const fetchData = async () => {
            try {
                setLoading(true);
                setError(null);

                const response = await axios.get(url, {
                    ...options,
                    signal: controller.signal
                });

                if (!cancelled) {
                    setData(response.data);
                }
            } catch (err) {
                if (!cancelled && !axios.isCancel(err)) {
                    setError(err);
                }
            } finally {
                if (!cancelled) {
                    setLoading(false);
                }
            }
        };

        fetchData();

        return () => {
            cancelled = true;
            controller.abort();
        };
    }, [url]);

    return { data, loading, error };
}

// Usage in React component
function UserProfile({ userId }) {
    const { data: user, loading, error } = useAxios(`/api/users/${userId}`);

    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error.message}</div>;
    if (!user) return <div>User not found</div>;

    return (
        <div>
            <h1>{user.name}</h1>
            <p>{user.email}</p>
        </div>
    );
}

// Advanced hook with caching
function useAxiosWithCache(url, options = {}) {
    const [cache, setCache] = useState(new Map());
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        const cacheKey = `${url}_${JSON.stringify(options)}`;

        // Check cache first
        if (cache.has(cacheKey)) {
            setData(cache.get(cacheKey));
            setLoading(false);
            return;
        }

        let cancelled = false;
        const controller = new AbortController();

        const fetchData = async () => {
            try {
                setLoading(true);
                setError(null);

                const response = await axios.get(url, {
                    ...options,
                    signal: controller.signal
                });

                if (!cancelled) {
                    setData(response.data);
                    setCache(prev => new Map(prev).set(cacheKey, response.data));
                }
            } catch (err) {
                if (!cancelled && !axios.isCancel(err)) {
                    setError(err);
                }
            } finally {
                if (!cancelled) {
                    setLoading(false);
                }
            }
        };

        fetchData();

        return () => {
            cancelled = true;
            controller.abort();
        };
    }, [url, cache]);

    return { data, loading, error };
}
```

## Wisdom of the Ancients

### Configuration Management Sacred Rituals

```javascript
// Environment-based configuration
const config = {
    development: {
        baseURL: 'http://localhost:3000/api',
        timeout: 10000,
        headers: {
            'X-Environment': 'development'
        }
    },
    production: {
        baseURL: 'https://api.example.com',
        timeout: 5000,
        headers: {
            'X-Environment': 'production'
        }
    }
};

const environment = process.env.NODE_ENV || 'development';
const apiConfig = config[environment];

// Create configured instance
const api = axios.create(apiConfig);

// Centralized error handling
const handleError = (error) => {
    if (error.response) {
        // Log error details
        console.error('API Error:', {
            status: error.response.status,
            data: error.response.data,
            url: error.config.url,
            method: error.config.method
        });

        // Show user-friendly messages
        const message = error.response.data?.message || 'An error occurred';
        showNotification(message, 'error');
    } else if (error.request) {
        console.error('Network Error:', error.request);
        showNotification('Network error. Please check your connection.', 'error');
    } else {
        console.error('Request Error:', error.message);
        showNotification('Request failed. Please try again.', 'error');
    }
};

// Apply error handling globally
api.interceptors.response.use(
    response => response,
    error => {
        handleError(error);
        return Promise.reject(error);
    }
);
```

### Request Optimization Performance Spells

```javascript
// Request deduplication
class RequestDeduplicator {
    constructor() {
        this.pendingRequests = new Map();
    }

    async request(key, requestFn) {
        // If request is already pending, return the existing promise
        if (this.pendingRequests.has(key)) {
            return this.pendingRequests.get(key);
        }

        // Create new request
        const promise = requestFn()
            .finally(() => {
                // Clean up after request completes
                this.pendingRequests.delete(key);
            });

        this.pendingRequests.set(key, promise);
        return promise;
    }
}

const deduplicator = new RequestDeduplicator();

// Usage
async function fetchUser(userId) {
    const key = `user_${userId}`;
    return deduplicator.request(key, () =>
        axios.get(`/api/users/${userId}`)
    );
}

// Multiple calls to fetchUser(123) will only make one HTTP request
Promise.all([
    fetchUser(123),
    fetchUser(123),
    fetchUser(123)
]).then(results => {
    console.log('All requests completed:', results);
});

// Request batching
class RequestBatcher {
    constructor(batchSize = 10, delay = 100) {
        this.batchSize = batchSize;
        this.delay = delay;
        this.queue = [];
        this.timeoutId = null;
    }

    add(request) {
        return new Promise((resolve, reject) => {
            this.queue.push({ request, resolve, reject });

            if (this.queue.length >= this.batchSize) {
                this.flush();
            } else if (!this.timeoutId) {
                this.timeoutId = setTimeout(() => this.flush(), this.delay);
            }
        });
    }

    async flush() {
        if (this.queue.length === 0) return;

        const batch = this.queue.splice(0, this.batchSize);

        if (this.timeoutId) {
            clearTimeout(this.timeoutId);
            this.timeoutId = null;
        }

        try {
            const requests = batch.map(item => item.request());
            const results = await Promise.allSettled(requests);

            results.forEach((result, index) => {
                const { resolve, reject } = batch[index];
                if (result.status === 'fulfilled') {
                    resolve(result.value);
                } else {
                    reject(result.reason);
                }
            });
        } catch (error) {
            batch.forEach(({ reject }) => reject(error));
        }
    }
}

const batcher = new RequestBatcher(5, 200);

// Usage
async function batchedFetch(url) {
    return batcher.add(() => axios.get(url));
}
```

## Common Curses & Their Remedies

### Curse 1: Not Handling Network Error Hexes
Only checking for `error.response` and ignoring network errors.

**Remedy:**
```javascript
// ❌ Bad: Only handling response errors
axios.get('/api/data')
    .catch(error => {
        if (error.response.status === 404) {
            // This will throw if error.response is undefined
        }
    });

// ✅ Good: Handle all error types
axios.get('/api/data')
    .catch(error => {
        if (error.response) {
            // Server error
            console.error('Server error:', error.response.status);
        } else if (error.request) {
            // Network error
            console.error('Network error');
        } else {
            // Request setup error
            console.error('Request error:', error.message);
        }
    });
```

### Curse 2: Memory Leak Interceptor Corruption
Not cleaning up interceptors when components unmount.

**Remedy:**
```javascript
// ✅ Good: Clean up interceptors
useEffect(() => {
    const requestInterceptor = axios.interceptors.request.use(config => config);
    const responseInterceptor = axios.interceptors.response.use(response => response);

    return () => {
        axios.interceptors.request.eject(requestInterceptor);
        axios.interceptors.response.eject(responseInterceptor);
    };
}, []);
```

### Curse 3: Not Cancelling Request Phantoms
Requests continuing after component unmount.

**Remedy:**
```javascript
// ✅ Good: Cancel requests on cleanup
useEffect(() => {
    const controller = new AbortController();

    axios.get('/api/data', { signal: controller.signal })
        .then(response => setData(response.data))
        .catch(error => {
            if (!axios.isCancel(error)) {
                setError(error);
            }
        });

    return () => controller.abort();
}, []);
```

### Curse 4: Hardcoded URL Binding Chains
Using hardcoded URLs instead of configuration.

**Remedy:**
```javascript
// ❌ Bad: Hardcoded URLs
axios.get('https://api.example.com/users');

// ✅ Good: Use base URL configuration
const api = axios.create({
    baseURL: process.env.REACT_APP_API_URL || 'https://api.example.com'
});

api.get('/users');
```

## Sacred Texts & Mystical Sources

{% embed url="https://axios-http.com/" %}

{% embed url="https://github.com/axios/axios" %}

{% embed url="https://axios-http.com/docs/intro" %}
