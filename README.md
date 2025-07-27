# AlfaNotes
by Marcus Zou

## Intro
This is a technotes WebApp utilizing the best open-sourced blogging platform of ghost.org;

## Run up the Tenchnotes WebApp
Some pre-jobs:
```shell
mkdir alfanotes && cd alfanotes
## make 2 folders for 2 volumes engaged later
mkdir db ghost
## Create a docker-compose.yaml file
nano docker-compose.yaml
```
The contents of the docker-compose.yaml shall be like below:
```textfile
services:
  ghost:
    image: ghost:5-alpine
    restart: always
    ports:
      - 8632:2368
    environment:
      # see https://ghost.org/docs/config/#configuration-options
      database__client: mysql
      database__connection__host: db
      database__connection__user: root
      database__connection__password: Kanada2024!pass
      database__connection__database: technotes
      # this url value is just an example, and is likely wrong for your environment!
      url: http://localhost:8632
      # contrary to the default mentioned in the linked documentation, this image defaults to NODE_ENV=production
      # (so development mode needs to be explicitly specified if desired)
      # NODE_ENV: development
    volumes:
      - ghost:/var/lib/ghost/content

  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: Kanada2024!pass
    volumes:
      - db:/var/lib/mysql

volumes:
  ghost:
  db:
```

or using sqlite3 database as below
```textfile
services:
  ghost:
    image: ghost:5-alpine
    container_name: notes-ghost
    restart: always
    ports:
      - 8632:2368
    environment:
      database__client: sqlite3
      database__connection__filename: /var/lib/ghost/content/data/ghost.db
      url: http://notes.alfazen.org
    volumes:
      - ghost:/var/lib/ghost/content

volumes:
  ghost:
```
then running up the docker:
```shell
## docker-compose up -d  ## (if you are a root user)
sudo docker-compose up -d
```
finally open a browser,
and type: http://localhost:8632 for the website;
or type: http://localhost:8632/ghost for the admin page.

## Pullover the webapp
As simple as below:
```shell
# Take down the 2 containers
docker step $(docker ps -ah)
docker rm $(docker ps -ah)
docker ps  ## there is no such 2 containers any more

# Remove the images
docker image rm $(docker images -ah)
docker images  ## there is no such 2 images any more
```

## Outro
You may go to https://ghost.org/ for more details.

## License
MIT
