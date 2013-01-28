# CakePHP #

I downloaded the [1.3.0 RC4][cake.1.3RC4] as the [1.2.7][cake.1.2.7] release threw some deprecated warning in my face.

In unpacked the release into my `htdocs` directory and started my apache and pointed my browser to [localhost][localhost]. If the apache2 is properly configured it should already output some cakePHP information.

I got some errors:

	/tmp/cache/ is not writable

The solution is to make the `cache` directory writable. I made the `tmp` directory writable for everyone, by adding the permissions using Finder's information menu.

	Please change the value of 'Security.salt'

Edit `app/config/core.php` file and change the value of the string where it says:

	Configure::write('Security.salt', 'DYhG93b0qyJfIxfs2guVoUubWwvniR2G0FgaC9mi');

I kept the length but put in something else.

	Please change the value of 'Security.cipher'

Again goto `app/config/core.php` file and change the value of the string where it says:

	Configure::write('Security.cipherSeed', '76859309657453542496749683645');

My database wasn't properly configured

	Your database configuration file is NOT present.

Rename the file `database.php.default` to `database.php` and set the parameters in it according to your setup. I had some big troubles getting my local development setup to work. I got access via my [phpMyAdmin][phpmyadmin] installation but cakePHP kept on complaining.

In the end I used:

	var $default = array(
	    'driver' => 'mysql',
	    'persistent' => false,
	    'host' => '127.0.0.1',
	    'login' => 'cake_blog_user',
	    'password' => 'cake_blog_password',
	    'database' => 'cake_blog',
	    'prefix' => '',
	);

When I used `localhost` as my host I couldn't connect even If I added the socket at `/tmp/mysql.sock` as the port (which works for my [Sequel Pro][sequelpro])

I also had to create a virtual host in `/opt/local/apache2/conf/httpd-vhosts.conf` (keep in mind to activate the extra conf file in `httpd.conf` by uncommenting `#Include conf/extra/httpd-vhosts.conf`) that pointed to the cake php webroot, as css and images weren't loaded properly:

	<VirtualHost *:80>
	    ServerAdmin webmaster@dummy-host.example.com
	    DocumentRoot "/opt/local/apache2/htdocs/cake_blog/app/webroot"
	    ServerName karmariders
	    ErrorLog "logs/cake_blog-error_log"
	    CustomLog "logs/cake_blog-access_log" common
	</VirtualHost>

And the according entry in `/etc/hosts`

	# My local aliases
	127.0.0.1	cake_blog

[localhost]: http://localhost
[cake.1.3RC4]: http://github.com/cakephp/cakephp1x/zipball/1.3.0-RC4
[cake.1.2.7]: http://github.com/cakephp/cakephp1x/zipball/1.2.7

[sequelpro]: http://www.sequelpro.com/
[phpmyadmin]: http://www.phpmyadmin.net/home_page/index.php
