#### Config
Data exists in /data/ folder in root. During installation I did:
- sudo mkdir -p /data/db
I then gave it the following:
- sudo chown -R `id -un` /data/db

#### Running
Start the mongo daemon with `mongod`
Start the shell in another terminal using `mongo` and use `quit()` to exit it
