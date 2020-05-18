# 1 Pull latest image

```shell
docker pull mongo
```

# 2 Create container

``` shell
# Port mapping: -p 27017:27017
# DB files saved location: -v [location on PC]:/data/db
# Authorize needed: --auth
docker run --name mongodb -p 27017:27017 -v ~/WorkSpace/MongoDB:/data/db -d mongo:latest --auth
```

# 3 Create admin user

``` shell
# Get into the container (admin)
docker exec -it mongodb mongo admin
# Create admin user (in mongo command line)
> db.createUser({ user: 'admin', pwd: '123456', roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] });
# Check authorization
> db.auth("admin","123456");
# If admin user not authorized to do something, set the role to be root
> db.grantRolesToUser('admin', [{ role: 'root', db: 'admin' }]);
```

# 4 Enjoy

Recommend to connect via [MongoDB Compass](https://www.mongodb.com/products/compass).