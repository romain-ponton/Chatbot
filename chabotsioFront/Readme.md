# Documentation Technique pour l'Application Chabotsio

## Introduction

Chabotsio est une application web de chatbot qui peut être exécutée localement sur votre machine. Elle utilise des modèles de langage fournis par Ollama pour générer des réponses. L'application est construite avec Express et Bun pour le backend, Vue.js pour le frontend, et MongoDB comme base de données.

## Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants installés sur votre machine :

- Node.js (Version LTS récente)
- Bun
- MongoDB (BDD)
- Ollama (pour les modèles de langage)

## Installation

### Frontend

```bash
git clone <url_depot> nom_dossier
cd nom_dossier
bun install
bun dev
```

### Backend

```bash
git clone <url_depot> nom_dossier
cd nom_dossier
mv .env.example .env
bun install
bun dev
```

### Semis

```bash
# Créer un utilisateur avec des privilèges admin
bun db:seed:admin

# Générer 100 utilisateurs fictifs
bun db:seed:users
```

## Contenu de l'Interface Utilisateur

- **Accueil** : Démarrer un nouveau chat ou accéder aux anciens chats via le tiroir.
- **Profil** : Modifier ses informations personnelles.

## Contenu de l'Interface Administrateur

- **Dashboard** : Listing des utilisateurs avec pagination et barre de recherche.

## Structure du Backend

```plaintext
├── README.md
├── bun.lockb
├── controllers
│   ├── adminController.ts
│   ├── authController.ts
│   ├── chatController.ts
│   ├── ollamaController.ts
│   └── userController.ts
├── database
│   ├── dbConnect.ts
│   └── seeders
│       ├── adminSeeder.ts
│       └── fakeUsersSeeder.ts
├── docker-compose.yml
├── http
│   ├── auth.http
│   ├── chat.http
│   └── ollama.http
├── index.ts
├── middlewares
│   ├── adminMiddleware.ts
│   ├── authMiddleware.ts
│   └── multer.ts
├── models
│   ├── chatModel.ts
│   └── userModel.ts
├── package.json
├── public
│   └── images
├── routes
│   ├── adminRoutes.ts
│   ├── authRoutes.ts
│   ├── chatRoutes.ts
│   ├── ollamaRoutes.ts
│   └── userRoutes.ts
├── startup.bat
├── storage
│   └── logs
│       ├── combined.log
│       └── error.log
├── tests
│   ├── auth
│   │   ├── login.test.ts
│   │   └── register.test.ts
│   └── ollama
│       └── ollama.test.ts
├── tsconfig.json
├── utils
│   └── logger.ts
└── validators
    └── authValidator.ts
```

## Points de Terminaison Backend

| **Path**                     | **Methods** | **Middlewares**                         |
|------------------------------|-------------|------------------------------------------|
| `/api/v1/ollama/show-models` | GET         | showModels                               |
| `/api/v1/auth/register`      | POST        | register                                 |
| `/api/v1/auth/login`         | POST        | login                                    |
| `/api/v1/auth/me`            | GET         | authMiddleware, me                       |
| `/api/v1/user/username/update` | PUT         | authMiddleware, updateUsername           |
| `/api/v1/user/password/update` | PUT         | authMiddleware, updatePassword           |
| `/api/v1/user/thumbnail/update` | PUT       | authMiddleware, multerMiddleware, updateProfileThumbnail |
| `/api/v1/chat`               | POST        | authMiddleware, chat                     |
| `/api/v1/chat/history`       | GET         | authMiddleware, chatHistory              |
| `/api/v1/chat/last`          | GET         | authMiddleware, getLastChat              |
| `/api/v1/chat/delete/:id`    | DELETE      | authMiddleware, deleteChat               |
| `/api/v1/chat/:id`           | GET         | authMiddleware, chatById                 |
| `/api/v1/admin/users`        | GET         | authMiddleware, adminMiddleware, getUsersList |
| `/api/v1/admin/users/:userId/delete` | DELETE  | authMiddleware, adminMiddleware, deleteUser |

## Diagramme MongoDB

![chatbot_simple_diagram](https://github.com/user-attachments/assets/39894776-b56c-408d-b67f-13f4cc04a9bb)


## Structure Frontend

```plaintext
├── README.md
├── bun.lockb
├── index.html
├── package.json
├── postcss.config.js
├── public
│   ├── drawer.png
│   └── vite.svg
├── src
│   ├── App.vue
│   ├── assets
│   │   └── vue.svg
│   ├── components
│   │   ├── Alert.vue
│   │   ├── Loading.vue
│   │   └── Navbar.vue
│   ├── main.ts
│   ├── pages
│   │   ├── Home.vue
│   │   ├── Login.vue
│   │   ├── Profile.vue
│   │   ├── Register.vue
│   │   └── admin
│   │       ├── Dashboard.vue
│   │       └── Login.vue
│   ├── router.ts
│   ├── style.css
│   ├── themes
│   │   └── index.ts
│   ├── types
│   │   └── index.ts
│   ├── utils
│   │   └── marked.ts
│   └── vite-env.d.ts
├── store
│   ├── authStore.ts
│   ├── modelStore.ts
│   ├── themeStore.ts
│   └── userStore.ts
├── tailwind.config.js
├── todo.md
├── tsconfig.json
├── tsconfig.node.json
└── vite.config.ts
```

## Points de Terminaison Frontend

| **Path**                     | **Component**  | **Meta**                     |
|------------------------------|----------------|------------------------------|
| `/`                          | Home           | requiresAuth: true           |
| `/profile`                   | Profile        | requiresAuth: true           |
| `/login`                     | Login          | requiresGuest: true          |
| `/register`                  | Register       | requiresGuest: true          |
| `/chat/:id`                  | Home           | requiresAuth: true, name: "chat" |
| `/admin/dashboard`           | Dashboard      | requiresAdmin: true          |
