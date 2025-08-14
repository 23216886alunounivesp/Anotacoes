

# **Relatório Técnico Detalhado: PWA "Perfect Posture Pilates"**

## **1. Visão Geral e Objetivo do Projeto**

O "Perfect Posture Pilates" é um **Aplicativo Web Progressivo (PWA)** projetado para servir como a plataforma digital de um estúdio de Pilates. O objetivo principal é fornecer uma interface pública informativa e uma área administrativa restrita (dashboard) para que os instrutores gerenciem aspectos operacionais do estúdio, como aulas, alunos e conteúdo do site. A arquitetura segue o padrão **Jamstack**, com um frontend desacoplado e funcionalidades de backend servidas por APIs serverless.

## **2. Tecnologias, Frameworks e Serviços Utilizados**

### **Frontend (Interface do Usuário)**

- **Framework:** **React (v19)** - Uma biblioteca JavaScript para construir interfaces de usuário reativas e baseadas em componentes.
- **Linguagens:** HTML5, CSS3, JavaScript (ES6+), JSX.
- **Roteamento:** **React Router (v6)** - Para criar uma experiência de navegação de página única (SPA), gerenciando rotas como `/`, `/login`, `/principal`, `/dashboard`, etc.
- **Build Tool:** **Vite (v6)** - Uma ferramenta de build moderna que oferece um servidor de desenvolvimento extremamente rápido com Hot Module Replacement (HMR) e um processo de build otimizado para produção.

### **Backend (Lógica do Servidor e Dados)**

- **Plataforma de Backend:** **Supabase** - Uma alternativa open-source ao Firebase, construída sobre tecnologias robustas. É utilizada para:
  - **Banco de Dados:** **PostgreSQL** - Um banco de dados relacional poderoso para armazenar dados de instrutores, alunos, aulas e conteúdo editável.
  - **Autenticação:** **Supabase Auth** - Gerencia o login de usuários (instrutores e, futuramente, alunos), com suporte a provedores OAuth como **Google (Gmail)** e login tradicional com email/senha.
- **Computação Serverless:** **Netlify Functions** - Funções Node.js executadas sob demanda na infraestrutura da Netlify. Elas servem como a camada de API (backend) segura entre o frontend (React) e o banco de dados (Supabase), processando todas as operações CRUD (Create, Read, Update, Delete).

### **Infraestrutura e DevOps**

- **IDE (Ambiente de Desenvolvimento):** **Visual Studio Code (VS Code)** rodando em um ambiente **WSL 2 (Subsistema Windows para Linux)** com a distribuição **Ubuntu**.
- **Controle de Versão:** **Git** - Utilizado para rastrear todas as alterações no código-fonte.
- **Hospedagem de Repositório:** **GitHub** - Plataforma para hospedar o repositório Git e colaborar.
- **Plataforma de Nuvem (Hospedagem e Deploy):** **Netlify** - Utilizada para:
  - **Hospedagem do Frontend:** Servir os arquivos estáticos (HTML, CSS, JS) gerados pelo build do Vite através de uma CDN global.
  - **Deploy Contínuo (CI/CD):** Conectada ao repositório GitHub, o Netlify automaticamente faz o build e o deploy do site a cada `git push` na branch `main`.
  - **Hospedagem das Funções Serverless:** Executar as Netlify Functions que compõem o backend.

### **PWA (Progressive Web App)**

- **Ferramentas:** **Vite PWA Plugin (`vite-plugin-pwa`)** - Um plugin para o Vite que automatiza a geração do Service Worker.
- **Biblioteca do Service Worker:** **Workbox** - Utilizada pelo plugin para gerar um Service Worker robusto que gerencia o cache de arquivos (precaching), estratégias de cache para requisições de rede (runtime caching) e funcionalidades offline.

## **3. Estado Atual do Projeto (Funcionalidades Implementadas)**

-   **Estrutura de Roteamento:**
    -   **Páginas Públicas:** Foram criadas e são funcionais, incluindo a Landing Page, Principal, Nosso Espaço (com mapa), Instrutores e Aulas (com conteúdo estático/buscado).
    -   **Página de Login:** Uma página dedicada `/login` está funcional.
    -   **Dashboard (Estrutura):** Uma rota protegida `/dashboard` foi criada, com um layout de barra lateral e uma área de conteúdo principal (`<Outlet />`).
-   **Autenticação e Autorização:**
    -   O fluxo de login com **Google via Supabase Auth** está totalmente funcional.
    -   Um **Contexto de Autenticação (`AuthContext`)** global gerencia o estado do usuário logado.
    -   O sistema diferencia usuários **Instrutores** de usuários comuns através de uma consulta à tabela `instrutores` no Supabase.
    -   O acesso à rota `/dashboard` é protegido por um componente **`PrivateRoute`**, que permite a entrada apenas de usuários autenticados e autorizados como instrutores.
    -   A funcionalidade de **Logout** está implementada e funcionando.
-   **Banco de Dados (Supabase):**
    -   O schema do banco de dados está definido e implementado, com tabelas para `instrutores`, `alunos`, `aulas`, `conteudo_paginas` e `contact_info`.
    -   **Row Level Security (RLS)** foi habilitado e políticas iniciais foram criadas para proteger o acesso aos dados.
-   **Dashboard do Instrutor (Funcionalidades Iniciais):**
    -   A funcionalidade de **Listar Aulas** está implementada, com o componente React `AulasCRUD` buscando dados da função Netlify `getAulas`.
    -   A funcionalidade de **Criar Aulas** está implementada, com o formulário no `AulasCRUD` enviando dados para a função Netlify `createAula`, que por sua vez os insere no Supabase.
    -   A funcionalidade de **Editar e Excluir Aulas** está iniciada na UI, mas a lógica de backend (`updateAula`, `deleteAula`) ainda precisa ser implementada.
    -   A funcionalidade de **Gerenciar Contato** foi iniciada com a criação da tabela, funções Netlify (`getContactInfo`, `updateContactInfo`) e o componente React `GerenciarContato`.

---

## **4. Análise de Conformidade com os Requisitos do Projeto Integrador**

Abaixo está uma lista de verificação dos itens da ementa e como o projeto os atende até o momento:

-   **[✅] Resolução de problemas:** O projeto foi desenvolvido de forma iterativa, superando desafios significativos de configuração de rede (WSL), depuração de erros de build e autenticação em ambiente de produção.
-   **[✅] Levantamento de requisitos:** Os requisitos foram definidos e refinados, como a criação de um dashboard para gerenciar conteúdo, aulas e instrutores.
-   **[✅] Desenvolvimento web com framework:** O projeto utiliza **React**, um dos frameworks/bibliotecas mais populares do mercado.
-   **[✅] HTML, CSS:** A base de toda a interface do usuário é construída com HTML semântico e CSS (incluindo Flexbox para layout e Media Queries para responsividade).
-   **[✅] linguagem de script:** O projeto é extensivamente desenvolvido em **JavaScript (com JSX)**, tanto no frontend (React) quanto no backend (Netlify Functions).
-   **[✅] Banco de Dados:** Utiliza **PostgreSQL** através do serviço **Supabase**, com schema relacional, chaves estrangeiras e políticas de segurança.
-   **[✅] Controle de Versão:** Todo o código-fonte é gerenciado com **Git** e hospedado no **GitHub**, com um histórico claro de commits.
-   **[✅] Nuvem:** O projeto faz uso extensivo de serviços de nuvem:
    -   **Netlify:** Para hospedagem, deploy contínuo (CI/CD) e computação serverless.
    -   **Supabase:** Para banco de dados como serviço (DBaaS) e autenticação.
    -   **Google Cloud:** Como provedor de identidade para o login OAuth.
-   **[✅] API:** O projeto define e consome sua própria API RESTful através das **Netlify Functions**, que servem como a camada de comunicação entre o frontend e o banco de dados.
-   **[✅] Acessibilidade:** Pontos básicos foram considerados (HTML semântico, `alt` text em imagens), mas uma auditoria e melhorias mais profundas podem ser feitas.
-   **[🔄] Testes:** O projeto foi testado manualmente de forma extensiva em ambiente de desenvolvimento e produção. **Testes automatizados** (unitários, de integração) ainda não foram implementados, mas a estrutura modular do projeto facilita sua adição futura.
-   **[❌] Análise de dados (Opcional):** Esta funcionalidade opcional ainda não foi abordada.

