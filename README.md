# Introduction
PowerDNS is an incredible project, and although it's easy to install and configure, I wanted to lower the bar for others to try it out for themselves. So, I created this nascent project   

PowerDNS is an incredible project, and although it's typically easy to install and configure, I want to lower the bar even further for others to try it out for themselves. So I will start maintaining this nascent project called `podman-powerdns`.

# Details
Each of the containers will leverage small and well-maintained images. They will either be based on UBI images, or as with the case of PowerDNS-Admin, they will be maintained by upstream maintainers.

# Introduction
To get started, use the following instructions. This project is just starting off, so I will make improvements soon. I'm having a really busy week, and I just wanted to throw this up as quickly as possible. All priveledges and SELinux requirements will be improved very soon. This is just a starting place for now.

There are three primary folders that you can get started with: `mysql`, `pdns-server`, and `pdns-recursor`. PowerDNS-Admin is a project maintained upstream, and the project can be found on GitHub [HERE](https://github.com/PowerDNS-Admin/PowerDNS-Admin). You can use these base `Dockerfile` configs to build your own containers, if you wish.

Instructions on how to edit usernames, passwords, database names, and API keys will be provided below:

1. Create the following directories on your Red Hat based system:
```
sudo mkdir -p {/opt/pdns/mysql,/opt/pdns/pdns,/opt/pdns/pdns-recursor,/opt/pdns/pdns-admin/data}
```

2. Next, create the `pdns` Podman pod with the following command:
```
sudo podman pod create -p 3306:3306 -p 5300:5300 -p 5300:5300/udp -p 53:53 -p 53:53/udp pdns
``` 
_Note: I will be improving this soon..._

3. Now, change the permission of `/opt/pdns/mysql` to user `27:27` and create the `pdns-mysql` container within the `pdns` Podman pod:
```
sudo chown -Rv 27:27 /opt/pdns/mysql
sudo podman run -d --pod pdns --name pdns-mysql -v /opt/pdns/mysql:/var/lib/mysql/data:Z quay.io/bjozsa-redhat/pdns-mysql-80:latest
```

4. Once that container has fully started, populate the db schema for PowerDNS with the following commands
```
sudo podman exec -it pdns-mysql sh -c 'mysql -u pdnsadmin --password=pdnsadmin -p pdns < /tmp/pdns-schema.sql'
```

5. You can test that the db was populated correctly with the following commands:
```
sudo podman exec -it pdns-mysql sh -c 'mysqlshow -u pdnsadmin --password=pdnsadmin  -p pdns;'
```

6. Remove the schema initialization file provided, and make sure that it's removed:
```
sudo podman exec -it -u root pdns-mysql sh -c 'rm -rf /tmp/pdns-schema.sql'
sudo podman exec -it -u root pdns-mysql sh -c 'ls -asl /tmp/'
```

7. Now run the PowerDNS recursor container:
```
sudo podman run -d --pod pdns --name pdns-recursor -v /opt/pdns/pdns-recursor:/etc/pdns-recursor:Z quay.io/bjozsa-redhat/pdns-recursor:latest
```

8. Run the PowerDNS server container:
```
sudo podman run -d --pod pdns --name pdns-server -v /opt/pdns/pdns:/etc/pdns:Z quay.io/bjozsa-redhat/pdns-server:latest
```

9. Lastly, run the PowerDNS-Admin webUI management container:
```
sudo podman run -d \
  -e SECRET_KEY='supersecretapikey' \
  --name=pdns-admin \
  -v pda-data:/opt/powerdns-admin/data \
  --network=host docker.io/ngoduykhanh/powerdns-admin:latest
```

10. Now you can log into the PowerDNS-Admin website using port standard port 80 (http).
_NOTE: This will obviously be configurable soon_

I will provide more later. I would recommend waiting until I get some rootless containers going, but for now this should just work (at least that's the hope anyway).
