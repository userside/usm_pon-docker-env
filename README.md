# USM_PON Docker Environment

This environment provides the feature of running the usm_pon module inside the Docker container. This environment **does not contain** the module itself. You should download it additionally in your account and copy it inside the environment as described below.

1. Download last version of docker-environment to your server from https://github.com/userside/usm_pon-docker-env/releases
2. Unzip downloaded archive to folder `usm_pon-docker-env` and copy `usm_pon` folder from unzipped folder into **userside bundle** folder. You should get approximately the following bundle file structure:
  ```
  /docker
   └── userside
        ├── config
        ├── data
        ├── usm_pon
        │    ├── log
        │    ├── module
        │    │    └── usm_pon.conf
        │    ├── cpanfile
        │    └── Dockerfile
        ├── docker-compose.yml
        ├── init.sh
        ├── Makefile
        └── README.md
  ```
3. From `usm_pon-docker-env/docker-composer.yml` copy the whole service config `usm_pon:...` to service block into your docker-compose.yml. **Pay attention to the formatting!** The indents in the yaml file are very important.
  ```yaml
  services:
    postgres:
      ...
    
    fpm:
      ...
    
    ...(other services)...

    usm_pon:
      build:
        context: ${USERSIDE_BASE_DIR}/usm_pon
      environments:
        USERSIDE_API_KEY: 'api-secret-key'
      volumes:
        - ${USERSIDE_BASE_DIR}/usm_pon/module:/app
        - ${USERSIDE_BASE_DIR}/usm_pon/log:/var/log/usm_pon
        - /etc/localtime:/etc/localtime:ro
        - /etc/timezone:/etc/timezone:ro
      networks:
        - internal

  volumes:
    ...
  ```
4. Setup environments.
5. Download **usm_pon** module from https://my.userside.eu/, extract and copy **only one file** `usm_pon.pl` to the subdirectory `usm_pon/module` folder. It should turn out as follows:
  ```
  /docker
   └── userside
        ├── config
        ├── data
        ├── usm_pon
        │    ├── log
        │    ├── module
        │    │    ├── usm_pon.conf
        │    │    └── usm_pon.pl
        │    ├── cpanfile
        │    └── Dockerfile
  ```

6. Go to folder `/docker/userside` then build docker-image for service usm_pon. It can take a long time, but it only needs to be done once.
  ```bash
  sudo docker-compose build usm_pon
  ```
7. Make a test run:
  ```bash
  sudo docker-compose run --rm usm_pon
  ```

8. Add a line to your /etc/cron.d/userside similar to the others. For example:
  ```
  */30 * * * *    root    /usr/local/bin/docker-compose .... run --rm usm_pon > /dev/null
  ```

### Any questions?

All questions are accepted only through the ticket system of your account.