# Bintray

1. Create a free account at bintray.com

## Obtain API Credentials

 Click "API Key" to obtain a key at https://bintray.com/profile/edit

Then:

```
mkdir -p ~/.bintray/
touch ~/.bintray/.credentials
# add the following lines
realm = Bintray API Realm
host = api.bintray.com
user = <insert-your-username-here>
password = <insert-your-key-here>
```

## Prepare for publication
Go to `https://bintray.com/<username>/maven`, so in my case to [bintray.com/oschrenk/maven](https://bintray.com/oschrenk/maven).

Click on the green button "Add new package" or visit [bintray.com/oschrenk/maven/new/package?pkgPath=](https://bintray.com/oschrenk/maven/new/package?pkgPath=).

Fill out the name, license, and point to the Github repository.

Click on Create package

## Prepare Scala project

### Add plugin

```
echo 'addSbtPlugin("org.foundweekends" % "sbt-bintray" % "0.5.6")' > project/bintray.sbt
```

You can check for the latest version at [sbt-bintray](https://github.com/sbt/sbt-bintray)

### Add license

I find this very odd, but it is not enough to specify a license on bintray, you also have to do it in your `build.sbt` file (and of course add the license file to your repo).

Add one of these license to your `build.sbt`

```
licenses += ("MIT", url("http://opensource.org/licenses/MIT"))

licenses += ("ISC", url("http://opensource.org/licenses/ISC"))
```

or convince sbt-bintray that is is fine

```
bintrayOmitLicense := true
```

## Limitations for OSS projects

### No Snapshot files

When

```
sbt publish
```

you might get

```
[error] java.lang.RuntimeException: error uploading to https://api.bintray.com/maven/oschrenk/maven/maven/dev/oschrenk/scampart_2.13/0.1.0-SNAPSHOT/scampart_2.13-0.1.0-SNAPSHOT.pom: {"message":"Snapshot files cannot be uploaded to OSS repositories. For more information, please see https://www.jfrog.com/confluence/display/RTF/Deploying+Snapshots+to+oss.jfrog.org"}
```

## Prepare a release

1. Fix the version to a non-snapshot number
2. Publish
3. Fix version number in README
4. Commit
4. Bump to to next snapshot version
5. Commit
6. Push

