Introduction

Chabotsio est une application web de chatbot qui peut être exécutée localement sur votre machine. Elle utilise des modèles de langage fournis par Ollama pour générer des réponses. L'application est construite avec Express et Bun pour le backend, Vue.js pour le frontend et MongoDB comme base de données.

Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants installés sur votre machine :

Node.js (Version LTS récente)
Chignon
MongoDB (BDD)
Ollama (pour les modèles de langage)
Installation frontale
    git clone <url_depot> nom_dossier
    cd nom_dossier
    bun install
    bun dev
Installation Ollama
Liste des modèles Ollama
    # Télécharge un modèle de langage depuis le référentiel d'Ollama et l'enregistre localement sur la machine
    ollama pull <nom_du_model>

    # Liste les modèles disponible
    ollama list

    # Supprimer un modèle
    ollama rm <nom_du_model>
Installation du backend

    git clone <url_depot> nom_dossier
    cd nom_dossier
    mv .env.example .env
    bun install
    bun dev
MONGODB AS DOCKER SERVICE (optionnelle)
    services:
  mongodb:
    image: mongodb/mongodb-community-server:7.0.11-ubi8
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - ./data:/data/db
    environment:
      MONGO_INITDB_DATABASE: chatbot
      
    restart: always
    # Run the service
    docker-compose up
    # Stop the service
    docker-compose down
Semis
    # Create a user with admin privileges 
    bun db:seed:admin
    
    # Generate a 100 fake users 
    bun db:seed:users
Contenu de l'interface utilisateur
Accueil : démarrer un nouveau chat ou accéder aux anciens chat, grâce au tiroir
Profil : modifier ses informations personnelles
Contenu de l'interface administrateur
Dashboard : listing des utilisateurs, pagination et barre de recherche
Structure du backend
    # ignore data if using mongo as Docker service
    tree -I "node_modules" -I "data"
    
├── README.md
├── bun.lockb
├── controllers
│   ├── adminController.ts
│   ├── authController.ts
│   ├── chatContoller.ts
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
│   ├── chatroutes.ts
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
    └── authvalidator.ts
Points de terminaison backend
[
  {
    path: "/api/v1/ollama/show-models",
    methods: [ "GET" ],
    middlewares: [ "showModels" ],
  }, {
    path: "/api/v1/auth/register",
    methods: [ "POST" ],
    middlewares: [ "register" ],
  }, {
    path: "/api/v1/auth/login",
    methods: [ "POST" ],
    middlewares: [ "login" ],
  }, {
    path: "/api/v1/auth/me",
    methods: [ "GET" ],
    middlewares: [ "authMiddleware", "me" ],
  }, {
    path: "/api/v1/user/username/update",
    methods: [ "PUT" ],
    middlewares: [ "authMiddleware", "updateUsername" ],
  }, {
    path: "/api/v1/user/password/update",
    methods: [ "PUT" ],
    middlewares: [ "authMiddleware", "updatePassword" ],
  }, {
    path: "/api/v1/user/thumbnail/update",
    methods: [ "PUT" ],
    middlewares: [ "authMiddleware", "multerMiddleware", "updateProfileThumbnail" ],
  },
  {
    path: "/api/v1/chat",
    methods: [ "POST" ],
    middlewares: [ "authMiddleware", "chat" ],
  }, {
    path: "/api/v1/chat/history",
    methods: [ "GET" ],
    middlewares: [ "authMiddleware", "chatHistory" ],
  }, {
    path: "/api/v1/chat/last",
    methods: [ "GET" ],
    middlewares: [ "authMiddleware", "getLastChat" ],
  }, {
    path: "/api/v1/chat/delete/:id",
    methods: [ "DELETE" ],
    middlewares: [ "authMiddleware", "deleteChat" ],
  }, {
    path: "/api/v1/chat/:id",
    methods: [ "GET" ],
    middlewares: [ "authMiddleware", "chatById" ],
  }, {
    path: "/api/v1/admin/users",
    methods: [ "GET" ],
    middlewares: [ "authMiddleware", "adminMiddleware", "getUsersList" ],
  }, {
    path: "/api/v1/admin/users/:userId/delete",
    methods: [ "DELETE" ],
    middlewares: [ "authMiddleware", "adminMiddleware", "deleteUser" ],
  }
]
Diagramme MongoDB
texte alternatif

Structure Frontend
tree -I "node_modules"

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
Points de terminaison frontaux
[
    {path : '/', component : Home, meta : {requiresAuth : true} },
    {path : '/profile', component : Profile, meta : {requiresAuth : true} },
    {path : '/login', component : Login,  meta : {requiresGuest : true} },
    {path : '/register', component : Register,  meta : {requiresGuest : true} },
    {path : '/chat/:id', component : Home,  meta : {requiresAuth : true}, name : "chat" },
    {path : '/admin/dashboard', component : Dashboard,  meta : {requiresAdmin : true} },
]
