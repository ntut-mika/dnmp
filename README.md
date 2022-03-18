# DNMP
LNMP in Docker

## DNMP Feature
- ubuntu：20.04
- mysql：latest stable
    - default account：root
    - default password：無
- nginx：latest stable
- reids：latest stable
- phpmyadmin： 5.1.3
- php
    - 5.6
    - 7.0
    - 7.1
    - 7.2
    - 7.3
    - 7.4
    - 8.0
    - 8.1

## Local enviroment require
- install any version php-cli
- install docker

## Tutorial
- first time usage you should make image following command
    ```bash
    ./buildImage.sh
    ```
- congiguration ./volumes/settings/config.json
    ```json
    {
        # php-cli-version in container
        "php-cli-version": "7.4",

        # composer version in container (1 or 2)
        "composer-version": "2", 

        # local folder map to container folder
        "folders": [
            {
                # local folder path
                "source": "~/Desktop/project",

                # container folder path, this forder should be empty or not exist
                "dist": "/var/www/project"
            }
        ],

        # site settings
        "sites": [
            {
                # site domain
                "domain": "phpmyadmin.test",

                # this project entroy point directory
                "entry-point": "/builds/phpmyadmin/5.1.3",
                
                # which nginx template should use in this site, the template file in volumes/settings/templates/nginx
                "template": "default.conf",

                # which php-fpm version should use in this site
                "php-fpm-version": "7.4",

                # add domain to your /etc/hosts ?
                "auto_host": true,

                # auto create or renew signed ssl ?
                "auto_ssl": true
            }
        ],
        
        # local port map to container port
        "ports": {
            # local port:container port
            "80": "80",
            "443": "443",
            "3306": "3306",
            "6379": "6379"
        }
    }
    ```
    - start and attach to the container
        ```bash
        sudo ./start.sh
        ```

## How to use formal certificate
Considering that the site may require use of a formal certificate, following step to setup

- step1: start container
- step2: configuration volumes/settings/config.json, let auto_ssl of your site be false
- step3: put your sll.key and ssl.crt inside volumns/settings/ssl/{domain} folder
- step4: restart container

## How to use nginx template
Considering each projects may require different nginx settings, you can create a special template for a specific project to avoid the need to fine-tune the site settings every time when container restarted

- step1: inside volumes/settings/templates/nginx create nginx config
- step2: configuration volumes/settings/config.json, let your site template be template file name in volumes/settings/templates/nginx
- step3: restart container

## Configuration php.ini
- step1: inside volumes/settings/templates/php folder have cli and fpm settings file, you can configuration it
- step2: restart container
