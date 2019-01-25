# Development template - Docker/WordPress

Creates a development environment for an existing WordPress site using Docker.

The `docker/docker-compose.yml` file is designed to run an existing production WordPress instance locally. It creates 2 containers - 

* `wordpress` - for the website files.
* `db` - for the associated MySQL database.

The `wordpress` container has 2 volumes - 

1. The `site/wp-content` folder - Copy this from the production instance. It's used for all customisations outside of the core WordPress code. This includes the themes, plugins, uploads etc.

2. `docker/setup` - 
    * `chown-wp-content.sh` - Makes the `wp-content` folder the same owner/group as the rest of the site. This has to be run manually within the container currently.
    
    * `disable-plugins.sh` - You can disable certain plugins that are not needed for development, e.g. security or SEO plugins. The plugin folder name(s) is inserted directly in to the bash script. This has to be run manually withint the container currently. 

**Note:** The `wp-config.php` file gets auto generated for this local WordPress instance.

**Note:** To get in to the `wordpress` container shell to run the bash scripts, you can do the following:

Navigate to `/docker` within project folder and run:

`$ docker-compose up`

Once the containers are running, get the name of the WordPress container:

`$ docker ps`

This will list the WordPress container, copy the name, then run:

`$ docker exec -it {paste_name_here} bash` 

The `db` container has 3 volumes - 

1. `docker/database` - A folder for all of the MySQL files, this gets automatically generated by docker and can remain untouched for average usage.

2. `docker/mysqldumps/backup.sql.gz` - This is a snapshot taken from the production instance via mysqldump, place the file in this location and it will get imported on the first run of the container automatically.

3. `docker/migrate/migrate.sql` - A domain migration script, this updates the domain name stored in the database, this also gets run automatically when the containers are spun up for the first time. You need to add your domain name in to the SQL queries currently.
