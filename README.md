# A php71 facade class for getting secrets from Google Cloud Datastore

## Why
One of the advantages of running an application on app engine, is your app assumes the identity of the Google account it's running under. I don't need to auth (as much). I still need to get permission to use google apis with other accounts, and then store the credentials (refresh/access tokens) for my app to use.
Google Cloud doesn't provide a simple way to store secrets in the environment. Heroku, for example, allows you to save environment vars encrypted with your app key.  To get my secrets into GAE, I need to put them in my projects app.yaml. My project's app.yaml file is version controlled - not a good place for secrets.

## Requirements
- Google cloud account with access to Google Cloud Datastore
- php71
- Secrets to store https://github.com/Jamesway/gae-credentials-php71
-
## Installation

Install the google api client library
```composer require require google/apiclient:^2.0

# docker
docker run --rm -v ($pwd):/app Jamesway/php71-cli-dev composer require google/apiclient:^2.0
```

Add this service class to your project
```
wget https://github.com/Jamesway/docker-python36-django/archive/master.zip -O ./temp.zip \
&& unzip -j temp.zip \
&& rm temp.zip \
&& rm README.md
```

Adjust the namespace for your project
```
<?php

namespace [PROJECT_NAMESPACE]\App;

use Google\Cloud\Datastore\DatastoreClient;

class SecretStoreService
{
...
}
```

Add your secrets to Google Cloud Datastore


## Usage
```
use NAMESPACE\SecretStoreService;

...
# gmail api credentials
if (!get_env('GMAIL_API_CREDS')) {

  $creds = SecretStoreService::get('GMAIL_API_CREDS');
  putenv('GMAIL_API_CREDS=$creds');
}
...
```
