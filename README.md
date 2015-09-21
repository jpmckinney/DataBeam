# DataBeam: Heroku

A fork of [GSA/DataBeam](https://github.com/GSA/DataBeam) to deploy to Heroku. Read its [README](https://github.com/GSA/DataBeam#readme). Note that CodeIgniter's `/index.php` and `/application` are copied to `web/`.

## Set up

* Create a new [GitHub application](https://github.com/settings/applications/new)
* Copy the Client ID and Client Secret
* Set the Homepage URL (e.g. `https://myapp.herokuapp.com/`)
* Set the Authorization callback URL (e.g. `https://myapp.herokuapp.com/auth/session/github`)

## Development

Install dependencies:

```
brew tap homebrew/php
brew install php55
brew install composer
composer install
```

Initialize the database:

```
mysql -u root -e 'CREATE DATABASE databeam'
mysql -u root databeam < sql/restdb.sql
```

Run a server (replace `REPLACE`):

```
GITHUB_CLIENT_ID=REPLACE GITHUB_CLIENT_SECRET=REPLACE ENV=development DATABASE_URL=mysql://root:@localhost:5000/databeam php -S 127.0.0.1:5000 -t web
```

Then, in another shell:

```
open http://127.0.0.1:5000
```

## Deployment

Replace `REPLACE`:

```
heroku create
heroku addons:create cleardb
heroku config:set ENV=production
heroku config:set GITHUB_CLIENT_ID=REPLACE
heroku config:set GITHUB_CLIENT_SECRET=REPLACE
heroku config:set ENCRYPTION_KEY=`base64 -b 16 /dev/random | head -1`
heroku config:set DATABASE_URL=`heroku config:get CLEARDB_DATABASE_URL`
git push heroku master
```

Then, initialize the database (replace `REPLACE`):

```
mysql -u REPLACE -p -h REPLACE REPLACE < sql/restdb.sql
```

Then, open the app:

```
heroku apps:open
```

## Issues

* File uploads probably won't work on Heroku
* A [package manager](https://devcenter.heroku.com/articles/getting-started-with-php#declare-app-dependencies
) would preferably be used for CodeIgniter libraries
* We may need to enable some [PHP extensions](https://devcenter.heroku.com/articles/php-support) on Heroku, e.g. `sqlite3` and/or `oauth`
