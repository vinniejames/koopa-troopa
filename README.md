# Shell Commands Cheatsheet 🐢

### Check for cache hits

Check cache status: `curl -svo /dev/null https://url.to/test`
Purge doc from Fastly: `curl -X PURGE https://url.to/purge/`

### Verify Checksum

SHA-256: `shasum -a 256 ~/Downloads/verifyMe.zip`

MD5: `md5 ~/Downloads/verifyMe.zip`

### Multi-line write to file

```
cat > /path/to/new/file.ext << EOF
// one
// two
// three
EOF
```

### Find
List files by creation:

`find . -type f -newermt 2016-09-07 | xargs ls -la`

find text files that were last modified 60 days ago:

`find /home/you -iname "*.txt" -mtime -60 -print`

http://www.cyberciti.biz/faq/howto-finding-files-by-date/

### Search
GREP
search all files and folders recursivley, \ escapes “.”

`grep -r "3\.1\.2" .`

Find all instaces of a string, then >> print out to a file

`grep -rin "string to find" * >> results.txt`

Show number of files a string occurs in recursively

`find . -type f | xargs grep -l 'string to search for' | wc -l`

Find and replace string in files recursively in directory:

`find folder/name/to/search -type f  -exec sed -i '' -e 's/RegExStringToFind/RegExsStringToReplace/g' {} \;`

### Cron Jobs
To view:
`crontab -l`
To edit:
`crontab -e`
Example:
```
MAILTO=your-email@gmail.com
*/30	*	*	*	*	/usr/local/bin/wget -O - -q -t 1 http://example.com/
0	10,16	*	*	*	/usr/local/bin/wget -O - -q -t 1 http://example/update
```

### Aliases for shell .profile

`alias sshExample='ssh username@ftpAddressOrIp'`

`alias storm='open /Applications/WebStorm.app'`

### Vagrant
SSH into vagrant box:
`vagrant ssh`

Stop or start vagrant, from `Vagrantfile` dir

`vagrant up`

`vagrant halt`

Rebuild vagrant from config file

`vagrant provision`

DATABASE DUMP via VAGRANT

`vagrant ssh`

```
mysql> show databases;
+----------------------+
| Database             |
+----------------------+
| information_schema   |
| mysql                |
| performance_schema   |
| wordpress_default    |
| wordpress_develop    |
| wordpress_trunk      |
| wordpress_unit_tests |
+----------------------+
7 rows in set (0.01 sec)
```

! exit mysql back to vagrant shell
vagrant@vvv:/vagrant$ `mysqldump wordpress_develop > backupFileName.sql`

### SCP
copy from remote to local:

`scp username@serveraddress:~/foo.txt localfolder/foo-local.txt`

copy from local to remote

`scp bar.txt username@serveraddress:~/barzzz.txt`

copy from remote to current local dir

`scp -r username@serveraddress:/foldertocopy .`

Copy file into running vagrant vm:

`scp -P 2222 vagrant@127.0.0.1:/home/vagrant/wp_dump.sql .`

`pw: vagrant`

### List folder sizes
`du -sh *`

### Quick Empty trash
`rm -rf ~/.Trash/*`

### Reset file permissions for WordPress or Webapp
directories
`find . -type d -exec chmod 755 {} \;`
files
`find . -type f -exec chmod 644 {} \;`

wp permissions

`sudo find . -type f -exec chmod 644 {} +`

`sudo find . -type d -exec chmod 755 {} +`

`sudo chmod 600 wp-config.php`

# Recursively find and replace in files 
e.g.

`find . -name "*.txt" -print0 | xargs -0 sed -i '' -e 's/fooOldString/barNewString/g'`

`find . -name "*.php" -print0 | xargs -0 sed -i '' -e 's/src="img/src="<?php echo get_template_directory_uri(); ?>\/img/g'`

`find . -name "*.php" -print0 | xargs -0 sed -i '' -e 's/this.src='\/img\/png/this.src='<?php echo get_template_directory_uri(); ?>\/img\/png/g'`

regex e.g. `s/this.src='\/img\/png/this.src='<?php echo get_template_directory_uri(); ?>\/img\/png/g`

Remove `class="lead"`

`find . -name "*.php" -print0 | xargs -0 sed -i '' -e 's/class="lead/class="/g'`

### Switch versions of Node
https://gist.github.com/kugaevsky/68a7fa894551da9c310a
http://apple.stackexchange.com/questions/171530/how-do-i-downgrade-node-or-install-a-specific-previous-version-using-homebrew
e.g.

`brew install node010` followed by 

`brew link --overwrite node010`

to install the 0.10 version of Node.JS.
  	 	
You might also need to `brew unlink node`
before you `brew install node010`

### Windows Power Shell
Mimic Unix commands in Power Shell
http://superuser.com/questions/502374/equivalent-of-linux-touch-to-create-an-empty-file-with-powershell
Make `touch` work

`function touch {set-content -Path ($args[0]) -Value ($null)}`

OR

`New-Item -ItemType file example.txt`
