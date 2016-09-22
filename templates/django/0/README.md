# Django

Uses the accent/rancher-django docker image to host a site in rancher.

The above image has gunicorn installed as we dont want to use pythons dev server in staging/production environments.

## Usage

Ensure repo is cloned to to the host

Ensure repo has a rancher.sh file located in the same directory as manage.py

Example:

```
#!/usr/bin/env bash

echo "waiting for db..."
while ! nc -w 1 -z $RDS_HOSTNAME $RDS_PORT 2>/dev/null;
do
  echo -n .
  sleep 1
done
echo "db ready"

pip install -r requirements.txt
python manage.py migrate
python manage.py collectstatic --noinput
gunicorn app.wsgi:application -w 2 -b :8000
```
