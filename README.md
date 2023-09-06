
# run command in the container

```shell

docker-compose build # just need to run one time
docker-composer up # start running container

docker-compose run --rm app sh -c "command"

# for example

docker-compose run --rm app sh -c "php artisan"

```



# first start

1. create a .env and update the variable
2. some commands:

```shell
php artisan migrate # create DB table 

php artisan key:generate # create a APP_KEY in the .env

```
