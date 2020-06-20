Taiga Dockerized environment
============================

Complete environment to run self-hosted Taiga.io project in an elegant
way.

**Features:**
- Optional SSL support in the container
- Optional events support (allows live updates in the application)
- BUILDS ANY TAIGA VERSION EASILY!
- In comparison to other docker images, this one runs production
  environment...
- Very flexible, a lot of environment variables to configure
- Uses docker-compose to simplify setup, can be used also standalone or
  in Kubernetes/Swarm
- Uses standardized JINJA2 to generate configuration files
- Allows to change a lot of parameters without rebuilding the container
- With Makefile all tasks are automated enough to provide a fully working environment without need to adjust anything

*Notice: This is not a official Taiga.io project and is not affiliated
with Taiga Agile, LLC © or any other company, it's a completly grassroot
project*

*Based on docker image built originally by Benjamin Hutchins <ben@hutchins.co> and released on GPL license*

*Built docker images are licensed under MIT*

#### What is Taiga?

Taiga is a project management platform for startups and agile developers & designers who want a simple, beautiful tool that makes work truly enjoyable.

> [taiga.io](https://taiga.io)

#### Running the container

The container is placed at `quay.io/riotkit/taiga`. There is no `latest` tag, we do not like it, it's an unstable, dangerous idea to use latest.
Instead of `latest` we tag each Taiga stable release with the release number ex. `quay.io/riotkit/taiga:4.2.12`

See the list of available tags there: https://quay.io/repository/riotkit/taiga?tab=tags and check out the configuration reference.

![versioning](./docs/versioning.png)

```bash
# NOTICE 1: Check "Configuration reference" section of this README for list of available environment variables
# NOTICE 2: You need a working PostgreSQL server
# NOTICE 3: Use volumes to keep your uploads, else they will disappear on container recreation

sudo docker run \
    -v $(pwd)/media:/usr/src/taiga-back/media \
    -p 80:80 \
    -e TAIGA_DB_HOST=some-db-host \
    -e TAIGA_DB_USER=some-user \
    -e TAIGA_DB_PASSWORD=some-password \
    quay.io/riotkit/taiga:4.2.12
```

#### Quick start with Docker-Compose

```
# get the sources
git clone https://github.com/riotkit-org/docker-taiga.git
cd docker-taiga

# prepare the environment
virtualenv .venv
source .venv/bin/activate
pip install -r ./requirements.txt

# optionally if you do not have docker-compose
# pip install docker-compose

# edit configuration
cp .env.dist .env
edit .env

# start it!
rkd :start

# tadam...
```

##### First log-in to the application, default username and password

To log-in first time you should go to the [login page](http://localhost/login?next=%252Fdiscover) and enter "admin" as username, and "123123" as password.
You can also log-in to the Django administration panel at http://localhost/admin/ (please remember about the slash at the end)

#### Configuring LDAP

LDAP support can optionally be enabled by setting the `TAIGA_LDAP` environment variable to `true`. See the Dockerfile for a list of the environment variables used for configuring LDAP and their descriptions.

#### Configuration reference

List of all environment variables that could be used.

```yaml
# Enables Celery support
- CELERY_ENABLED # (default: false)

# Serializer type supported by Taiga at Celery
- CELERY_SERIALIZER_TYPE # (default: pickle)


- DEBIAN_FRONTEND # (default: noninteractive)


- DEBUG # (default: false)

# The maximum number of pending connections
- GUNICORN_BACKLOG # (default: 2048)

# Workers silent for more than this many seconds are killed and restarted
- GUNICORN_TIMEOUT # (default: 60)

# The number of worker processes for handling requests
- GUNICORN_WORKERS # (default: 4)

# The maximum number of simultaneous clients
- GUNICORN_WORKER_CONNECTIONS # (default: 1000)

# The maximum number of requests a worker will process before restarting
- GUNICORN_WORKER_MAX_REQUESTS # (default: 3000)

# The number of worker threads for handling requests
- GUNICORN_WORKER_THREADS # (default: 1)

# The granularity of Error log outputs
- GUNICRON_LOG_LEVEL # (default: info)

# Set to "c" for English, pl-PL.UTF-8 for Polish etc.
- LANG # (default: en_US.UTF-8)

# Set to "c" for English, pl-PL.UTF-8 for Polish etc.
- LC_TYPE # (default: en_US.UTF-8)

# The DN to bind to the LDAP server with. If left blank the client will attempt to bind anonymously.
- LDAP_BIND_DN # (default: "")

# The password for the bind DN.
- LDAP_BIND_PASSWORD # (default: "")

# The LDAP attribute that will be used for the account's Email address.
- LDAP_EMAIL_ATTRIBUTE # (default: "mail")

# The fallback authentication method to use if LDAP fails. This will allows users to login with either an LDAP account or a local account. Set to a blank string to prevent logging in with anything other than LDAP.
- LDAP_FALLBACK # (default: "normal")

# The LDAP attribute that will be used for the account's full name.
- LDAP_FULL_NAME_ATTRIBUTE # (default: "cn")

# The port to connect to the LDAP server on.
- LDAP_PORT # (default: 0)

# Whether or not to save the LDAP password in the local database. If `LDAP_FALLBACK` is set to `normal` this will allow users that have logged in with LDAP before to login even if the LDAP server is unavailable.
- LDAP_SAVE_LOGIN_PASSWORD # (default: "true")

# The root of the LDAP structure in which to search for user accounts.
- LDAP_SEARCH_BASE # (default: "")

# Additional filter added to the user account query.
- LDAP_SEARCH_FILTER_ADDITIONAL # (default: "")

# The LDAP server URL.
- LDAP_SERVER # (default: "")

# Set to `true` to enable StartTLS when connecting to the server.
- LDAP_START_TLS # (default: "false")

# The LDAP attribute that will be used for the account's Taiga username.
- LDAP_USERNAME_ATTRIBUTE # (default: "uid")

# Interval (in seconds) for a background task that sends mails
- MAIL_NOTIFICATIONS_SEND_EVERY # (default: 120)


- RKD_PATH # (default: "/opt/rkd/.rkd")

# Queue connection string
- TAIGA_BROKER_URL # (default: amqp://guest:guest@rabbit:5672)

# PostgreSQL database hostname
- TAIGA_DB_HOST # (default: postgres)

# PostgreSQL database name
- TAIGA_DB_NAME # (default: taiga)

# PostgreSQL database user password
- TAIGA_DB_PASSWORD # (default: )

# PostgreSQL database user login
- TAIGA_DB_USER # (default: taiga)

# Default locale ex. en, pl
- TAIGA_DEFAULT_LOCALE # (default: en)

# SMTP "From" header value
- TAIGA_EMAIL_FROM # (default: taiga@localhost)

# SMTP server host
- TAIGA_EMAIL_HOST # (default: smtp)

# SMTP user password
- TAIGA_EMAIL_PASS # (default: some-password)

# SMTP server port
- TAIGA_EMAIL_PORT # (default: 25)

# SMTP user login
- TAIGA_EMAIL_USER # (default: taiga@riotkit.org)

# Use TLS encryption when sending mails
- TAIGA_EMAIL_USE_TLS # (default: true)

# Enable mail server
- TAIGA_ENABLE_EMAIL # (default: False)

# Enable or disable events?
- TAIGA_ENABLE_EVENTS # (default: false)

# Enable or not the webhooks
- TAIGA_ENABLE_WEBHOOKS # (default: False)

# Hostname for events server
- TAIGA_EVENTS_HOST # (default: events)

# Default container group id
- TAIGA_GID # (default: 1000)

# Hostname of your instance (domain ex. riotkit.org or subdomain - board.riotkit.org)
- TAIGA_HOSTNAME # (default: localhost)

# Asana importer app ID
- TAIGA_IMPORTER_ASANA_APP_ID # (default: "")

# Asana importer app secret
- TAIGA_IMPORTER_ASANA_APP_SECRET # (default: "")

# Override callback URL for Asana importer. Will be automatically set based on Taiga URL if left blank.
- TAIGA_IMPORTER_ASANA_CALLBACK_URL # (default: "")

# Enable the Asana project importer
- TAIGA_IMPORTER_ASANA_ENABLED # (default: "false")

# GitHub importer client ID
- TAIGA_IMPORTER_GITHUB_CLIENT_ID # (default: "")

# GitHub importer client secret
- TAIGA_IMPORTER_GITHUB_CLIENT_SECRET # (default: "")

# Enable the GitHub project importer
- TAIGA_IMPORTER_GITHUB_ENABLED # (default: "false")

# JIRA importer cert
- TAIGA_IMPORTER_JIRA_CERT # (default: "")

# JIRA importer consumer key
- TAIGA_IMPORTER_JIRA_CONSUMER_KEY # (default: "")

# Enable the JIRA project importer
- TAIGA_IMPORTER_JIRA_ENABLED # (default: "false")

# JIRA importer public cert
- TAIGA_IMPORTER_JIRA_PUB_CERT # (default: "")

# Trello importer api key
- TAIGA_IMPORTER_TRELLO_API_KEY # (default: "")

# Enable the Trello project importer
- TAIGA_IMPORTER_TRELLO_ENABLED # (default: "false")

# Trello importer secret key
- TAIGA_IMPORTER_TRELLO_SECRET_KEY # (default: "")

# Set to `true` to enable the LDAP authentication.
- TAIGA_LDAP # (default: false)

# List of plugins to enable eg. "slack, other, other" or just "slack"
- TAIGA_PLUGINS # (default: "")

# Redis url
- TAIGA_REDIS_URL # (default: "redis://redis:6379/0")

# Protocol http or https your instance will listen on
- TAIGA_SCHEME # (default: http)

# !!! Secret key, please change it with your own
- TAIGA_SECRET_KEY # (default: REPLACE-ME-j1598u1J^U*(y251u98u51u5981urf98u2o5uvoiiuzhlit3))

# Default container user id
- TAIGA_UID # (default: 1000)

# Timezone
- TZ # (default: "Europe/Warsaw")


```

Building images
---------------

```
# to build a 4.2.5 version of backend and frontend
rkd :build:snapshot --backend-version=4.2.5 --frontend-version=4.2.5-stable
```

Troubleshooting
---------------

1. Not able to log-in or edit user settings

If you have trouble logging in or editing user settings it may be related to a failure sending emails. This will be accompanied by a `[CRITICAL] WORKER TIMEOUT` error in the logs. Try validating your email configuration or setting `TAIGA_ENABLE_EMAIL` to `false` to see if that fixes the issue.

2. Using existing database

Set `TAIGA_DB_HOST` to point to your PostgreSQL hostname.
See also: https://github.com/riotkit-org/docker-taiga/issues/24

3. Debugging, getting logs

In container there is a supervisor configured. Use `supervisorctl` to access services.

Examples:
```bash
supervisorctl status send_app_notifications
supervisorctl tail -f send_app_notifications stderr
supervisorctl tail -f backend stderr
supervisorctl tail -f nginx stderr
```

4. Installing under subpath, not subdomain eg. domain.org/taiga

Sorry, you cannot. The application itself does not support it. See https://github.com/riotkit-org/docker-taiga/issues/32

Developing the container
------------------------

- The container is built on quay.io
- When you start working on it locally, please before commiting do the `rkd :generate-readme` (pull requests should be rejected, when something was added only to README.md, not to README.j2, and when environment variables were added to Dockerfile but README file was not generated)
- Use `rkd` for building, pushing, etc.

Releasing
---------

On Travis CI the build is triggered each month, then all recent versions of Taiga are built. Already existing docker tags are not overwritten.
The build is also triggered on-commit. Snapshot version is made from master branch, and is always a latest master, can be unstable.

To release a bugfix version and REBUILD EXISTING TAGS just add "@force-rebuild" in commit message, recent 3 tags will be rebuilt (not all in registry).

Versioning
----------

![versioning](./docs/versioning.png)

The container has it's own versioning.  There are a few rules:

1. Releasing TEST builds requires tagging `master` branch with name containing "RC" eg. `1.0-RC3`

After tagging 1.0-RC3, recent 3 versions of Taiga will be built with RC eg. quay.io/riotkit/taiga:4.2.12-D1.0-RC3
**No main tag would be replaced, so quay.io/riotkit/taiga:4.2.12 stays untouched**

2. Development versions of the container

On each commit a SNAPSHOT version is produced. Snapshot means a not numbered version of docker container, replaces on each new commit.

Example of produced images on push: taiga:5.0.0-SNAPSHOT, taiga-5.0.1-SNAPSHOT

2. Releasing STABLE images requires tagging `master` with just version number eg. `1.0`

When `master` branch was tagged and pushed with eg. `1.0`, then 2 recent versions of Taiga would be rebuilt.
The main versions such as **quay.io/riotkit/taiga:4.2.12** would be rebuilt, and new tags such as **quay.io/riotkit/taiga:4.2.12-D1.0** would be produced.

If you want to use only unchanged, stable releases, without getting hotfixes you should use a TAIGA VERSION + BUILD NUMBER, WITHOUT RC. Example: **quay.io/riotkit/taiga:4.2.12-D1.0**

Copyleft
--------

Created by **RiotKit Collective**, a libertarian, grassroot, non-profit organization providing technical support for the non-profit Anarchist movement.

Check out those initiatives:
- International Workers Association (https://iwa-ait.org)
- Federacja Anarchistyczna (http://federacja-anarchistyczna.pl)
- Związek Syndykalistów Polski (https://zsp.net.pl) (Polish section of IWA-AIT)
- Komitet Obrony Praw Lokatorów (https://lokatorzy.info.pl)
- Solidarity Federation (https://solfed.org.uk)
- Priama Akcia (https://priamaakcia.sk)