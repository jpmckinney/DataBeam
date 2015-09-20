# DataBeam: Heroku

A fork of [GSA/DataBeam](https://github.com/GSA/DataBeam) to deploy to Heroku. Read its [README](https://github.com/GSA/DataBeam#readme).

## Development

```
brew tap homebrew/php
brew install php55
brew install composer
composer install
```

## Deployment

Replace `REPLACE`:

```
heroku create
heroku config:set GITHUB_CLIENT_ID=REPLACE
heroku config:set GITHUB_CLIENT_SECRET=REPLACE
git push heroku master
```
