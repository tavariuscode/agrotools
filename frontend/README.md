# 🎨 Frontend - Guia de Integração com Laravel API

Documentação para integração do frontend com a API Laravel do backend.

---

## 📋 Índice

- [Visão Geral](#-visão-geral)
- [Configuração](#-configuração)
- [Autenticação com Laravel Sanctum](#-autenticação-com-laravel-sanctum)
- [Exemplos de Requisições](#-exemplos-de-requisições)
- [CORS](#-cors)
- [Frameworks Suportados](#-frameworks-suportados)
- [Tratamento de Erros](#-tratamento-de-erros)
- [Testando a API](#-testando-a-api)

---

## 🎯 Visão Geral

Este projeto usa **Laravel 12 como backend** e suporta diferentes abordagens para o frontend:

### Opções Disponíveis

**🌐 Frontend Separado via API REST (Principal)**
- ✅ **React** / Next.js
- ✅ **Vue.js** / Nuxt
- ✅ **Angular**
- ✅ **Svelte**
- ✅ **Mobile** (React Native, Flutter)
- Comunicação via **REST API** com autenticação **Laravel Sanctum**

**🔥 Alternativa: Laravel Livewire**
- Para quem prefere fullstack Laravel sem API REST separada
- Componentes reativos em PHP
- Ideal para dashboards e CRUDs rápidos
- [Saiba mais sobre Livewire](https://livewire.laravel.com)

> **Este guia foca em integração via API REST**. Para Livewire, consulte a [documentação oficial](https://livewire.laravel.com).

---

## 🔧 Configuração

### URLs Base

```javascript
// Desenvolvimento
const API_URL = 'http://localhost:8080';
const API_BASE = 'http://localhost:8080/api';

// Produção
const API_URL = 'https://api.seu-dominio.com';
const API_BASE = 'https://api.seu-dominio.com/api';
```

### Variáveis de Ambiente

Crie um arquivo `.env` ou `.env.local`:

```bash
# React / Next.js
REACT_APP_API_URL=http://localhost:8080
REACT_APP_API_BASE=http://localhost:8080/api

# Vue / Nuxt
VITE_API_URL=http://localhost:8080
VITE_API_BASE=http://localhost:8080/api

# Angular
API_URL=http://localhost:8080
API_BASE=http://localhost:8080/api
```

---

## 🔐 Autenticação com Laravel Sanctum

O Laravel Sanctum fornece autenticação baseada em **tokens** para APIs.

### Fluxo de Autenticação

```
1. Frontend faz login → Backend valida credenciais
2. Backend retorna token
3. Frontend armazena token (localStorage/sessionStorage)
4. Requisições subsequentes incluem token no header
```

### 1. Login (Obter Token)

**Endpoint:** `POST /api/login`

```javascript
// JavaScript / Fetch API
async function login(email, password) {
  const response = await fetch('http://localhost:8080/api/login', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
    },
    body: JSON.stringify({
      email: email,
      password: password
    })
  });

  if (!response.ok) {
    throw new Error('Login failed');
  }

  const data = await response.json();
  const token = data.token;
  
  // Armazenar token
  localStorage.setItem('token', token);
  
  return data;
}

// Uso
login('user@example.com', 'password')
  .then(data => console.log('Logged in!', data))
  .catch(error => console.error('Error:', error));
```

### 2. Requisições Autenticadas

Todas as requisições subsequentes devem incluir o token:

```javascript
async function fetchProtectedData() {
  const token = localStorage.getItem('token');
  
  const response = await fetch('http://localhost:8080/api/user', {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
      'Authorization': `Bearer ${token}`
    }
  });

  if (!response.ok) {
    throw new Error('Request failed');
  }

  return await response.json();
}
```

### 3. Logout

**Endpoint:** `POST /api/logout`

```javascript
async function logout() {
  const token = localStorage.getItem('token');
  
  await fetch('http://localhost:8080/api/logout', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
      'Authorization': `Bearer ${token}`
    }
  });
  
  // Remover token
  localStorage.removeItem('token');
}
```

### 4. Verificar Usuário Autenticado

**Endpoint:** `GET /api/user`

```javascript
async function getCurrentUser() {
  const token = localStorage.getItem('token');
  
  const response = await fetch('http://localhost:8080/api/user', {
    headers: {
      'Authorization': `Bearer ${token}`,
      'Accept': 'application/json'
    }
  });
  
  return await response.json();
}
```

---

## 📡 Exemplos de Requisições

### GET - Listar Recursos

```javascript
async function getPosts() {
  const token = localStorage.getItem('token');
  
  const response = await fetch('http://localhost:8080/api/posts', {
    headers: {
      'Authorization': `Bearer ${token}`,
      'Accept': 'application/json'
    }
  });
  
  return await response.json();
}
```

### POST - Criar Recurso

```javascript
async function createPost(title, content) {
  const token = localStorage.getItem('token');
  
  const response = await fetch('http://localhost:8080/api/posts', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`,
      'Accept': 'application/json'
    },
    body: JSON.stringify({
      title: title,
      content: content
    })
  });
  
  return await response.json();
}
```

### PUT/PATCH - Atualizar Recurso

```javascript
async function updatePost(id, title, content) {
  const token = localStorage.getItem('token');
  
  const response = await fetch(`http://localhost:8080/api/posts/${id}`, {
    method: 'PUT', // ou 'PATCH'
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`,
      'Accept': 'application/json'
    },
    body: JSON.stringify({
      title: title,
      content: content
    })
  });
  
  return await response.json();
}
```

### DELETE - Deletar Recurso

```javascript
async function deletePost(id) {
  const token = localStorage.getItem('token');
  
  const response = await fetch(`http://localhost:8080/api/posts/${id}`, {
    method: 'DELETE',
    headers: {
      'Authorization': `Bearer ${token}`,
      'Accept': 'application/json'
    }
  });
  
  return response.ok;
}
```

---

## 🌍 CORS

O backend já está configurado para aceitar requisições de origens externas.

### Configuração no Laravel

O arquivo `config/cors.php` no backend já está configurado:

```php
'paths' => ['api/*', 'sanctum/csrf-cookie'],
'allowed_methods' => ['*'],
'allowed_origins' => ['*'], // Em produção, especifique domínios
'allowed_headers' => ['*'],
'supports_credentials' => true,
```

### Headers Necessários

Sempre inclua estes headers nas requisições:

```javascript
headers: {
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'Authorization': `Bearer ${token}` // Para rotas protegidas
}
```

---

## 🚀 Frameworks Suportados

### React / Next.js

#### Usando Axios

```bash
npm install axios
```

```javascript
// src/services/api.js
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:8080/api',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  }
});

// Interceptor para adicionar token
api.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

**Uso:**

```javascript
import api from './services/api';

// Login
const login = async (email, password) => {
  const { data } = await api.post('/login', { email, password });
  localStorage.setItem('token', data.token);
  return data;
};

// Listar posts
const getPosts = async () => {
  const { data } = await api.get('/posts');
  return data;
};

// Criar post
const createPost = async (post) => {
  const { data } = await api.post('/posts', post);
  return data;
};
```

#### Usando React Query

```bash
npm install @tanstack/react-query axios
```

```javascript
import { useQuery, useMutation } from '@tanstack/react-query';
import api from './services/api';

// Hook para listar posts
export const usePosts = () => {
  return useQuery({
    queryKey: ['posts'],
    queryFn: async () => {
      const { data } = await api.get('/posts');
      return data;
    }
  });
};

// Hook para criar post
export const useCreatePost = () => {
  return useMutation({
    mutationFn: async (post) => {
      const { data } = await api.post('/posts', post);
      return data;
    }
  });
};

// Componente
function Posts() {
  const { data: posts, isLoading } = usePosts();
  const createPost = useCreatePost();

  if (isLoading) return <div>Loading...</div>;

  return (
    <div>
      {posts.map(post => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  );
}
```

---

### Vue.js / Nuxt

#### Usando Axios

```bash
npm install axios
```

```javascript
// plugins/axios.js
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:8080/api'
});

api.interceptors.request.use(config => {
  const token = localStorage.getItem('token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

**Uso em componente Vue:**

```vue
<template>
  <div>
    <div v-for="post in posts" :key="post.id">
      {{ post.title }}
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import api from '@/plugins/axios';

const posts = ref([]);

onMounted(async () => {
  const { data } = await api.get('/posts');
  posts.value = data;
});
</script>
```

---

### Angular

#### Usando HttpClient

```typescript
// services/api.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private apiUrl = 'http://localhost:8080/api';

  constructor(private http: HttpClient) {}

  private getHeaders(): HttpHeaders {
    const token = localStorage.getItem('token');
    return new HttpHeaders({
      'Content-Type': 'application/json',
      'Accept': 'application/json',
      'Authorization': token ? `Bearer ${token}` : ''
    });
  }

  login(email: string, password: string): Observable<any> {
    return this.http.post(`${this.apiUrl}/login`, 
      { email, password },
      { headers: this.getHeaders() }
    );
  }

  getPosts(): Observable<any> {
    return this.http.get(`${this.apiUrl}/posts`, 
      { headers: this.getHeaders() }
    );
  }

  createPost(post: any): Observable<any> {
    return this.http.post(`${this.apiUrl}/posts`, 
      post,
      { headers: this.getHeaders() }
    );
  }
}
```

---

### React Native

```javascript
// services/api.js
import AsyncStorage from '@react-native-async-storage/async-storage';

const API_URL = 'http://localhost:8080/api';

export const api = {
  async request(endpoint, options = {}) {
    const token = await AsyncStorage.getItem('token');
    
    const config = {
      ...options,
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json',
        ...options.headers,
      }
    };

    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }

    const response = await fetch(`${API_URL}${endpoint}`, config);
    return await response.json();
  },

  async login(email, password) {
    const data = await this.request('/login', {
      method: 'POST',
      body: JSON.stringify({ email, password })
    });
    
    await AsyncStorage.setItem('token', data.token);
    return data;
  },

  async getPosts() {
    return await this.request('/posts');
  }
};
```

---

## 🔍 Tratamento de Erros

```javascript
async function fetchData() {
  try {
    const response = await fetch('http://localhost:8080/api/posts', {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Accept': 'application/json'
      }
    });

    if (!response.ok) {
      // Erro HTTP
      if (response.status === 401) {
        // Não autenticado - redirecionar para login
        localStorage.removeItem('token');
        window.location.href = '/login';
        return;
      }

      if (response.status === 422) {
        // Erros de validação
        const errors = await response.json();
        console.error('Validation errors:', errors.errors);
        return;
      }

      throw new Error(`HTTP error! status: ${response.status}`);
    }

    return await response.json();

  } catch (error) {
    console.error('Request failed:', error);
    throw error;
  }
}
```

### Estrutura de Resposta da API

**Sucesso:**

```json
{
  "id": 1,
  "title": "Post Title",
  "content": "Post content",
  "created_at": "2025-10-30T10:00:00.000000Z",
  "updated_at": "2025-10-30T10:00:00.000000Z"
}
```

**Erro de Validação (422):**

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "title": [
      "The title field is required."
    ],
    "content": [
      "The content field is required."
    ]
  }
}
```

**Erro de Autenticação (401):**

```json
{
  "message": "Unauthenticated."
}
```

---

## 🛠️ Testando a API

### Usando cURL

```bash
# Login
curl -X POST http://localhost:8080/api/login \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{"email":"user@example.com","password":"password"}'

# Request autenticado
curl -X GET http://localhost:8080/api/posts \
  -H "Accept: application/json" \
  -H "Authorization: Bearer SEU_TOKEN_AQUI"
```

### Usando Postman/Insomnia

1. **Criar requisição POST para `/api/login`**
   - Body (JSON):
     ```json
     {
       "email": "user@example.com",
       "password": "password"
     }
     ```
   - Headers:
     ```
     Content-Type: application/json
     Accept: application/json
     ```

2. **Copiar token da resposta**

3. **Criar requisição GET para `/api/posts`**
   - Headers:
     ```
     Accept: application/json
     Authorization: Bearer SEU_TOKEN_AQUI
     ```

---

## 🚀 Próximos Passos

1. **Configure seu framework frontend** (React, Vue, Angular, etc.)
2. **Instale dependências HTTP** (axios, fetch, etc.)
3. **Configure serviço de API** com a URL base
4. **Implemente autenticação** com Sanctum
5. **Crie seus componentes** consumindo a API
6. **Trate erros** adequadamente
7. **Configure ambiente de produção**

### Alternativa com Livewire

Se preferir não criar um frontend separado, considere usar **Laravel Livewire**:
- Componentes reativos escritos em PHP
- Sem necessidade de API REST
- Ideal para dashboards e CRUDs
- [Documentação Livewire](https://livewire.laravel.com)

---

## 📞 Suporte

- 📖 **Documentação Backend**: `../backend/README.md`
- 📖 **Documentação Principal**: `../README.md`
- 🔐 **Laravel Sanctum**: https://laravel.com/docs/12.x/sanctum
- 🔥 **Laravel Livewire**: https://livewire.laravel.com

---

**Bom desenvolvimento! 🚀**
