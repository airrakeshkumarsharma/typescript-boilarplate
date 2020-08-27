## Installation
#### 2. Install dependencies

```
$ npm i
```

#### 3. Update repository name in `.github/workflows/latest.yml`

```
      - name: Publish image to Github Packages
        uses: docker/build-push-action@v1
        with:
          username: $GITHUB_ACTOR
          password: ${{ secrets.GITHUB_TOKEN }}
          registry: docker.pkg.github.com
          repository: YOUR_GITHUB_USER/YOUR_REPO_NAME/YOUR_PACKAGE_NAME
          tags: latest
```

## Development

### Start dev server
Starting the dev server also starts MongoDB as a service in a docker container using the compose script at `docker-compose.dev.yml`.

```
$ npm run dev
```
Running the above commands results in 
* 🌏**API Server** running at `http://localhost:3000`
* ⚙️**Swagger UI** at `http://localhost:3000/dev/api-docs`
* 🛢️**MongoDB** running at `mongodb://localhost:27017`

## Packaging and Deployment
#### 1. Build and run without Docker

```
$ npm run build && npm run start
```

#### 2. Run with docker

```
$ docker build -t api-server .
$ docker run -t -i -p 3000:3000 api-server
```

#### 3. Run with docker-compose

```
$ docker-compose up
```


---

## Environment
To edit environment variables, create a file with name `.env` and copy the contents from `.env.default` to start with.

| Var Name  | Type  | Default | Description  |
|---|---|---|---|
| NODE_ENV  | string  | `development` |API runtime environment. eg: `staging`  |
|  PORT | number  | `3000` | Port to run the API server on |
|  MONGO_URL | string  | `mongodb://localhost:27017/books` | URL for MongoDB |

## Logging
The application uses [winston](https://github.com/winstonjs/winston) as the default logger. The configuration file is at `src/logger.ts`.
* All logs are saved in `./logs` directory and at `/logs` in the docker container.
* The `docker-compose` file has a volume attached to container to expose host directory to the container for writing logs.
* Console messages are prettified
* Each line in error log file is a stringified JSON.


### Directory Structure

```
+-- scripts
|   +-- dev.sh
+-- src
|   +-- controllers
|   |   +-- book
|   |   |   +-- add.ts
|   |   |   +-- all.ts
|   |   |   +-- index.ts
|   |   |   +-- search.ts
|   +-- errors
|   |   +-- index.ts
|   +-- middleware
|   |   +-- request-middleware.ts
|   +-- models
|   |   +-- Book.ts
|   +-- app.ts
|   +-- mongo-connection.ts
|   +-- routes.ts
|   +-- server.ts
+-- .env
+-- .env.default
+-- .eslintrc.json
+-- .gitignore
+-- .travis.yml
+-- docker-compose.dev.yml
+-- docker-compose.yml
+-- Dockerfile
+-- jest.config.js
+-- nodemon.json
+-- openapi.json
+-- package-lock.json
+-- package.json
+-- README.md
+-- tsconfig.json
```
