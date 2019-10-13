## MongoBB

Inspired by [this guy](https://thedatafrog.com/mongodb-remote-raspberry-pi/)

1 - Install mongodb.

```
sudo apt install mongodb
```

2 - Put the server to run.

```
sudo systemctl enable mongodb
```

3 - Test it.

```
mongo
```

```
> use test
switched to db test
> db.col.insert({a:1})
> db.col.insert({a:2})
> db.col.find()
{ "_id" : ObjectId("5ce6ad86dace0cba25fa3458"), "a" : 1 }
{ "_id" : ObjectId("5ce6ad8bdace0cba25fa3459"), "a" : 2 }
```

## Openning MongoDB to the network

1 - Install *lsof* to watch the doors of the pi.

```
sudo apt install lsof
```

Then do

```
sudo lsof -i -P -n | grep LISTEN
```

You might see the 2 doors for the mongodb open only for local connections (127.0.0.1) at the ports 27017 and 27017. Check if the ssh is also open in your pi for external connections.

To avoid using iptables, install *ufw* to manage the firewall and open the doors. Be careful as the data base is not secured!

```
sudo apt install ufw
```

Open the doors! (including the ssh here)

```
sudo ufw allow ssh
sudo ufw allow 27017
sudo ufw allow 28017
```

Enable *ufw* to apply the rules and run. Use status command to check if it is running.

```
sudo ufw enable
sudo ufw status
```

Now fix the connection in the mongodb configuration file.

```
sudo nano /etc/mongodb.conf 
```

Comment the following line:

```
# bind_ip = 127.0.0.1
```

Reinstall the bd server.

```
sudo systemctl restart mongodb
```

To test it, go to your browser (in another computer) and check the following page:

```
http://192.168.0.108:28017/
```

If you want to test the remote access use the terminal:

```
mongo 192.168.0.108
```