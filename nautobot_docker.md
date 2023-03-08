### Nautobot Plugin Development Commands 

* invoke build debug 
* invoke shell-plus -> IPAddress IPAddress._meta.label
* sudo deluser "" 
* fuser -n tcp -k <portnumber>
* nautobot-server  runserver 0.0.0.0:8000 --insecure 
* sudo -iu nautobot
* docker:

```

	$ sudo systemctl status docker 
	$ sudo systemctl enable--now docker 
	$ docker volume ls
	$ docker volume rm <name>
	$ docker ps 
	$ docker ps -a
	$ docker system prune -->yes
	$ sudo service redis-server stop 
	$ sudo service postgresql stop
	$ docker volume rm <name>
	$ sudo lsof -i -P -n | grep 6379
	$ sudo ss -lptn 'sport = :<port>'
	$ sudo service docker stop 

```

* Postgres port: 5432 
* Redis Port : 6379
* docker-compose exec db psql -U nautobot -d postgres -c "CREATE DATABASE  nautobot;"
* rm -rf~/.cache/pypoetry/virtualenvs/nautobot-plugin-ixia-<tab>

* poetry shell --> activate venv

* pip3 uninstall -y nautobot

* export $(cat creds.env | xargs)

## lock inconsistent solution 
* poetry lock --no-update
* poetry install 
* cd ~/.cache/pypoetry/cache/
* rm -rf artifacts/ cache/  poetry cache list, poetry config experimental.new-installer false 
* poetry lock --check
* poetry add nautobot=1.4.5
* sudo rm -r migrations/
* sudo -u postgres psql

