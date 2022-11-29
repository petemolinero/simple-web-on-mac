# Simple Web Development Environment Using Docker Compose

## Overview
The Docker development environment consists of four parts:
 - Nginx
 - PHP
 - MySQL
 - Mutagen

[Mutagen](https://mutagen.io/) is a tool that compensates for the slow loading
of volumes on MacOS.

The folder structure should be:

```
┌ database
│──── current
│──── fresh
│ docker
│ nginx
│──── certs
└ app
```

### General Operation
 - Start with `docker-compose up -d` (use `--build` for the first run, or
   whenever you change configurations)
 - Stop with `docker-compose down` or `docker-compose down --remove-orphans`

## Installation

### Website files
Put all of your website files inside the `web` directory inside the project
root.

Depending on your application, you may need to update `nginx/default.conf` to
serve a different folder within the `app` directory.

### Database

If you'd like to do a fresh import into the database, put all SQL files
(filename doesn't matter) into the `/database/fresh` folder. The files in that
folder will only be executed if the `/database/current` folder is empty.

The `/database/fresh` folder can contain an `sql` file that your database will
be populated with. The current database files are stored in `/database/current`.
If you'd like to wipe the database and reload from the `fresh` directory, simply
delete the files in the current directory.

If you've been working in your `current` database and want to create a new fresh
version from your current version, you can run the following command:

`docker exec mkt_mysql_1 mysqldump -u example_user --password=password example_db > backup.sql`

Of course, make sure that you replace `mkt_mysql_1` with whatever your mysql
container name is, which you can find out by running `docker stats`.

### Hosts
Add `127.0.0.1    mkt3.local` to your `/etc/hosts` file.

### SSL Certificates

#### Generate certificates
1. `cd` into certs
2. Run `mkcert -install`
3. Run the following command: `mkcert mkt3.local "*.mkt3.local" mkt3.local
localhost 127.0.0.1 ::1`

#### mkcert Documentation
[github.com/FiloSottile/mkcert](https://github.com/FiloSottile/mkcert)

## Performance
Macs have some serious performance issues with Docker. For the most part, they
can be remedied with Mutagen.
