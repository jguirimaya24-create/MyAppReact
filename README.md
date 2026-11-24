# TP4 : Docker et Docker Compose

1. J'ai cloné le projet MyAppReact (dossiers Client et Server).

2. J'ai créé un Dockerfile pour le serveur (Express) : image node:lts-alpine, installation des dépendances avec npm install, port 9000, démarrage avec npm start.

3. J'ai créé un Dockerfile pour le client (React/Vite) : image node:lts-alpine, npm install, npm run build, installation de serve, port 3000. Comme le projet utilise Vite, le build est dans le dossier dist.

4. J'ai ajouté un fichier .dockerignore dans chaque dossier pour ne pas copier node_modules dans les images.

5. J'ai construit les images :
```
docker build -t mern-server ./Server
docker build -t mern-client ./Client
```

1. J'ai créé le réseau et lancé les 3 conteneurs avec Docker seul :
```
docker network create mern-network
docker run -d --name mongodb --network mern-network mongo
docker run -d --name server --network mern-network -p 9000:9000 -e MONGO_URI=mongodb://mongodb:27017/mydb mern-server
docker run -d --name client --network mern-network -p 3000:3000 mern-client
```

1. J'ai supprimé ces conteneurs et le réseau, puis j'ai créé le fichier docker-compose.yml avec 3 services (mongodb, server, client) et le réseau mern-network en bridge.

2. J'ai lancé l'application avec Docker Compose :
```
docker compose up --build
```

## Résultat

- Client : http://localhost:3000
- API : http://localhost:9000/api/health

## Remarque

Le serveur fourni n'utilise pas encore MongoDB dans son code. La variable MONGO_URI est définie et le conteneur mongodb tourne sur le même réseau, prêt pour plus tard.