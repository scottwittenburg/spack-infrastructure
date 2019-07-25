
 - setup

```
cp vars vars2

# set gitlab and cdash passwords
# Make sure that they are each at least 8 characters long
#
# Also, if you want to mirror your own spack fork and/or
# a different branch, set UPSTREAM_REPO and/or UPSTREAM_REF
$EDITOR vars2

source vars2
```

```
docker-compose up -d mysql
watch 'docker-compose ps'
```

 - wait until all services report as "up" and "healthy", then ctrl-c the
   `watch`.

```
docker-compose -f docker-compose.yml -f docker-compose-alternate.yaml up cdash
docker-compose up -d cdash gitlab
watch 'docker-compose ps'
```

 - wait until all services report as "up" and "healthy", then ctrl-c the
   `watch`.

 - browse to `localhost:8081`. (CDash instance) If you get redirected to `viewProjects.php`, you can go straight to login page with `http://localhost:8081/login.php`.
 - login with name `root@docker.container` and cdash password
 - create a new project called "spack"

 - browse to `localhost:8080`. (Gitlab instance)
 - login with name `root` and gitlab password
 - browse to `localhost:8080/admin/runners`
 - Take note of the registration token under "Set up a shared Runner manually".

```
$EDITOR vars2 # set RUNNER_REGISTRATION_TOKEN
source vars2

docker-compose up -d rshell rdocker
```

 - Refresh the runners browser page.  A new runner should appear in the table
   below (sometimes, it takes a few minutes to appear).

 - browse to `localhost:8080/projects/new`
 - create a new project called "spack-ci".  Make sure that you initialize the
   repository with a README.

```
docker-compose up -d sync
```

 - browse to `localhost:8080/root/spack-ci/pipelines`

 - If a first pipeline is already present, it is likely one generated
   automatically by auto-devops.  It can be ignored.  After a few minutes, a new
   pipeline should be generated.  This is an instance of the spack
   package-building pipeline.

 - To shutdown:

```
docker-compose down
```

 - To delete persistent data

```
rm -rf volumes
```
