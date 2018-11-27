# Useful Snippets

Collection of snippets and use-cases from the Shipping Docker series that
I think will come in handy (WIP - for personal reference, will organize/clean-up
in the future)

---

### mysqldump

Use new/temp container to connect to existing database in network and dump a
table - writes output to pwd of host machine

```bash
$ docker run -it --rm \
  --network=sd_sd-net \
  mariadb:latest \
  mysqldump -h mysql -u root -proot my-app > my-app.sql
```
