# Docker support

## Development

Development docker image supports live reload

Environment variables must be located in the `.env.dev` file.
Port forwarding must be controlled manually in the `docker-compose.dev.yml` file.
Frontend environment variables must be located in `frontend/.env` file (see `frontend/.env.sample` for example configuration)

Run to get started:

```bash
docker-compose -f docker-compose.dev.yml up -d
```

To rebuild the image and run it:

```bash
docker-compose -f docker-compose.dev.yml up -d --build
```

### Attaching to the API container

To enter in the attach mode enter:
```bash
docker attach <container_name>
```

To exit the attach mode enter the following combination: `ctrl+p ctrl+q`

## Production

Copy `.env.sample` to `.env`

`NODE_ENV` is set to `production`

`devDependencies` are not downloaded

Run to get started:

```bash
docker-compose -f docker-compose.prod.yml up  -d
```

To rebuild the image and run it:

```bash
docker-compose -f docker-compose.prod.yml up -d --build
```

## MongoDB

MongoDB container data is stored in `mongodata` volume

Run this to delete volume and containers:

```bash
docker-compose -f docker-compose.{dev,prod}.yml down
```

If you want to remove dangling volume run:

```bash
docker volume ls | grep mongodata | awk '{print $2}' | xargs docker volume rm
```
