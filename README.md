# A php71 facade class for getting secrets from Google Cloud Datastore

## Why
One of the advantages of running an application on app engine, is your app assumes the identity of the Google account it's running under - you don't need to auth (as much). Account permissions are still necessary to use google apis with other accounts. Once you have permission and subsequent credentials (refresh/access tokens), you need to store them for your app to use.
Google Cloud doesn't provide a simple way to create environment variables. Heroku, for example, allows you to save environment vars (encrypted with your app key) from their cli or the app's web profile.  To get secrets into GAE, you need to put them in projects app.yaml which is probably and should be version controlled - not a good place for secrets.  

Google has a key management service, which provides encryption/decryption services, but Google Cloud Datastore encrypts at rest and in transit. Any encryption would sit on top of what GCD already provides.

## Requirements
- Google cloud account with access to Google Cloud Datastore
- php71
- Secrets to store https://github.com/Jamesway/gae-credentials-php71

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

Create a Google Cloud Datastore entity to store your secrets



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
