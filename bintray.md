# Bintray

1. Create a free account at bintray.com
2. Cick API Key to obtain a key at https://bintray.com/profile/edit
3. Add a new maven package; you should see something like this:

```
mkdir -p ~/.bintray/
touch ~/.bintray/.credentials
# add the following lines
realm = Bintray API Realm
host = api.bintray.com
user = <insert-your-username-here>
password = <insert-your-key-here>
```
