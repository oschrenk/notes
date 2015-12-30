# Udoo Neo on OS X

- Download the image
- Prepare the MicroSD card (it took 6 hours)

Install [serial driver](http://www.udoo.org/docs-neo/driversandtools/Mac%20USB%20Drivers/EnergiaFTDIDrivers2.2.18.pkg) and [network driver](http://nyus.joshuawise.com/HoRNDIS-rel8pre1.pkg). The first one forces you to restart, after the second one you have to restart.

After the second restart there should be a new network interface available to you system. I assigned it an IP `192.168.7.1` to be exact. Then I was ready.

```
ssh udooer@192.168.7.2
# default password is `udooer`
```
##

Visit http://192.168.7.2/firstconfig


## Install Ruby

```
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev

cd
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc

git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Instal
rbenv install -v 2.2.4
rbenv global 2.2.4
```

## Reading Sensors

Enable gyroscope

    echo 1 > /sys/class/misc/FreescaleGyroscope/enable

Read gyroscope data

    cat /sys/class/misc/FreescaleGyroscope/data

Enable accelerometer

    echo 1 > /sys/class/misc/FreescaleAccelerometer/enable

Read accelerometer data

    cat /sys/class/misc/FreescaleAccelerometer/data

Enable temperature sensor

    sudo sh -c 'echo lm75 0x48 >/sys/class/i2c-dev/i2c-1/device/new_device'

Read temperature sensor

    cat /sys/class/i2c-dev/i2c-1/device/1-0048/temp1_input

