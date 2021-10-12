# USM_CHECKER Docker Environment

This environment provides the feature of running the usm_checker module inside the Docker container. This environment **does not contain** the module itself. You should download it additionally in your account and copy it inside the environment as described below.

1. Download last version of docker-environment to your server from https://github.com/userside/usm_checker-docker-env/releases
2. Unzip downloaded archive to folder `usm_checker-docker-env` and copy `usm_checker` from unzipped folder into **userside bundle** folder. You should get approximately the following bundle file structure:
  ```
  /docker
   └── userside
        ├── config
        ├── data
        ├── usm_checker
        │    ├── log
        │    ├── module
        │    └── Dockerfile
        ├── docker-compose.yml
        ├── init.sh
        ├── Makefile
        └── README.md
  ```
3. From `usm_checker-docker-env/docker-composer.yml` copy the whole service config `usm_checker:...` to service block into your docker-compose.yml. **Pay attention to the formatting!** The indents in the yaml file are very important.
  ```
  services:
    postgres:
      ...
    
    fpm:
      ...
    
    ...(other services)...

    usm_checker:
      build:
        context: ${USERSIDE_BASE_DIR}/usm_checker
      volumes:
        - ${USERSIDE_BASE_DIR}/usm_checker/module:/app
        - ${USERSIDE_BASE_DIR}/usm_checker/log:/var/log/userside/usm_checker
      networks:
        - internal

  volumes:
    ...
  ```
4. Download **usm_checker** module from https://my.userside.eu/ and extract the module files to the subdirectory `usm_checker/module` folder. It should turn out as follows:
  ```
  /docker
   └── userside
        ├── config
        ├── data
        ├── usm_checker
        │    ├── log
        │    ├── module
        │    │    ├── multiping
        │    │    ├── settings.yml-example
        │    │    └── usm_checker.py
        │    └── Dockerfile
  ```
5. Rename `settings.yml-example` to `settings.yml` and setup parameters into `api:` block.
6. **Do not change** the `log:` block!
7. Configure `checks:` according to the [documentation](https://wiki.userside.eu/Usm_checker#.D0.A1.D1.82.D1.80.D0.B0.D1.82.D0.B5.D0.B3.D0.B8.D0.B8_.D0.BF.D1.80.D0.BE.D0.B2.D0.B5.D1.80.D0.BE.D0.BA_.D0.B4.D0.BE.D1.81.D1.82.D1.83.D0.BF.D0.BD.D0.BE.D1.81.D1.82.D0.B8).
8. Build docker-image for service usm_checker
  ```
  sudo docker-compose build usm_checker
  ```
9. Make a test run:
  ```
  sudo docker-compose run --rm usm_checker
  ```

10. Add a line to your /etc/cron.d/userside similar to the others. For example:
  ```
  */2 * * * *    root    /usr/local/bin/docker-compose .... run --rm usm_checker > /dev/null
  ```

### Any questions?

All questions are accepted only through the ticket system of your account.