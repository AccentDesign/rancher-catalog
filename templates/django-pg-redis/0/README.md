# Django

Uses the accent/rancher-django docker image to host a site in rancher.

The above image has gunicorn installed as we dont want to use pythons dev server in staging/production environments.

## Usage

Ensure repo is cloned to to the host

Ensure repo has a rancher.sh file located in the same directory as manage.py

Example:

```
#!/bin/bash

export PGPASSWORD=$RDS_PASSWORD

while ! psql --host=$RDS_HOSTNAME --port=$RDS_PORT --username=$RDS_USERNAME > /dev/null 2>&1; do
    echo 'Waiting for connection with db...'
    sleep 1;
done;
echo 'Connected to db...';

pip install -r requirements.txt
python manage.py collectstatic --noinput
python manage.py migrate
gunicorn app.wsgi:application -w 2 -b :8000
```
