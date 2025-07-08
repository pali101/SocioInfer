# SocioInfer

**socioinfer** is a containerized microservice system that infers a Twitter user's interests by analyzing their profile and social graph. It combines on-demand data syncing, graph-based storage (Neo4j), and NLP-driven interest extraction.

## Architecture

- **`twitter-interest-inference`** (FastAPI + Python)  
  - Extracts interests from user and followings' bios using NLP.  
  - Queries **Neo4j** directly for graph data.  
  - Triggers syncing via `NetworkSync`.

- **`NetworkSync`** (Express + TypeScript)  
  - Syncs a user's Twitter followings from the Twitter API into Neo4j.  
  - Also supports and exposes APIs for mutual followings lookup and user graph storage.

- **`Neo4j`** (Graph Database)  
  - Stores users and their following relationships as a graph.

## Getting Started

### Clone the repository (with submodules)
```bash
git clone --recurse-submodules https://github.com/pali101/SocioInfer.git
cd SocioInfer
```

### Configure environment variables
Each service requires its own `.env` file.

 For twitter interest inference:
  ```bash
  cp ./twitter-interest-inference/.env.example ./twitter-interest-inference/.env
  ```
  For network sync:
  ```bash
  cp ./NetworkSync/.env.example ./NetworkSync/.env
  ```

### Run the services

Both services are containerized and orchestrated via Docker Compose. Start everything using Docker Compose:

```bash
docker-compose up --build
```
This will launch all services, networked together and ready for API requests.

## Structure

```
socioinfer/
├── NetworkSync/                   # Submodule: Twitter data sync
├── twitter-interest-inference/    # Submodule: interest inference API
├── docker-compose.yml
└── README.md
```

## Tech Stack

- **Backend**: FastAPI (Python), Express (TypeScript)
- **Database**: Neo4j
- **Containerization**: Docker, Docker Compose
- **NLP**: Sentence Transformers

## API Endpoints

### Twitter Interest Inference (Port 8000)

- `GET /interests/{username}`: Returns inferred interests (optionally scored and model-selectable)

### Network Sync (Port 4000)

- `POST /api/sync`: Syncs a user's Twitter followings into Neo4j
- `POST /api/user/store`: Stores a user in the graph
- `GET /api/mutual?user1=&user2=`: Checks mutual followings between two users