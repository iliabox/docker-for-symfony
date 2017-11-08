# Docker stack for Symfony 3

## Installation

1. Create a `.env` from the `.env.dist` file and adapt it according to the needs of the application

    ```sh
    $ cp .env.dist .env && vim .env
    ```

2. Build/run containers in detached mode

    ```sh
    $ docker-compose build
    $ docker-compose up -d
    ```

3. Update your system's hosts file


4. Prepare the Symfony application
    1. Update Symfony parameters (*app/config/parameters.yml*)

    2. Composer install & create database

        ```sh
        $ docker-compose exec php bash
        $ composer install
        $ sf doctrine:database:create
        $ sf doctrine:schema:update --force
        $ sf server:start 0.0.0.0:8000
        ```
