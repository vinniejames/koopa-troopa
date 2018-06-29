# Shell Commands Cheatsheet 🐢

[Explain Shell :)](https://www.explainshell.com)

[Just enough](http://alexpetralia.com/posts/2017/6/26/learning-linux-bash-to-get-things-done)

### Windows Power Shell
Mimic Unix commands in Power Shell
http://superuser.com/questions/502374/equivalent-of-linux-touch-to-create-an-empty-file-with-powershell
Make `touch` work

`function touch {set-content -Path ($args[0]) -Value ($null)}`

OR

`New-Item -ItemType file example.txt`

Access envvars `$Env:NAME`

Set persistant envvars `setx NAME value`

Set session envvars `set NAME value`


### Logging stdout and stderr

[`python script.py 2>&1 | tee output.log`](https://stackoverflow.com/a/418899/1580610)

[`script output.log; python script.py`](https://explainshell.com/explain?cmd=script+output.log%3B+python+script.py)

### Static Site Backup

`wget -P /path/to/destination/directory/ -mpck --user-agent="" -e robots=off --wait 1 -E https://www.example.com/`

### AWS

Download S3 bucket to local `aws s3 sync s3://bucketName localFolder/path`

Upload sync local to S3 `aws s3 sync localFolder/path s3://bucketName`

Upload sync with delete `aws s3 sync localFolder/path s3://bucketName --delete`

### Git cherry pick

eg next-master commits need to be saved for later

`git checkout master -b on-hold-changes-branch`

`git cherry-pick olderCOmmitHash^..newerCommitHash` inclusive branches (repeat for another range of commits)

`git checkout next-master`

`git revert olderCOmmitHash^..newerCommitHash`

`git push origin next-master` will open a commit message dialouge for each revert

### VIM save w/o sudo

`:w !sudo tee %`

## Docker

### Build image in current Dockerfile dir

`docker build -t docker-image-name .`

### Run container, expose and publish local:8080 and map to container:80

`docker run -p 8080:80 container_name`

### Run container with mapped local src folder

`docker run -v /Path/to/dev/folder:/var/www/html -p 8080:80 --sig-proxy=false named-image`

### Enter bash shell for container
`docker exec -it container_name /bin/bash`


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

OR
disable expansion by quoting the here-doc delimiter

```
cat > /path/to/new/file.ext << 'HERE'
$keep
$dollar
$signs
HERE
```

### Find
List files by creation:

`find . -type f -newermt 2016-09-07 | xargs ls -la`

find text files that were last modified 60 days ago:

`find /home/you -iname "*.txt" -mtime -60 -print`

http://www.cyberciti.biz/faq/howto-finding-files-by-date/

Recursively delete all files of a name or specific extension in the current directory

`find . -name "*.bak" -type f`

to see exactly which files you will remove

Then add `-delete` flag

`find . -name "*.bak" -type f -delete`


### Search
GREP
search all files and folders recursivley, \ escapes “.”

`grep -r "3\.1\.2" .`

Find all instaces of a string, then >> print out to a file

`grep -rin "string to find" * >> results.txt`

Show number of files a string occurs in recursively

`find . -type f | xargs grep -l 'string to search for' | wc -l`

Find and replace string in files recursively in directory:

`find folder/name/to/search -type f  -exec LC_ALL=C sed -i '' -e 's/RegExStringToFind/RegExsStringToReplace/g' {} \;`

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

Add localhost TLS

[Create self-signed OpenSSL certificate](https://www.njimedia.com/local-development-with-ssl-tls-p2/), and mount to VM directory, eg `$local/cert >> $vagrant/var/www/cert`

Enable SSL in Apache config `/etc/apache2/sites-enabled/default-ssl.conf`

```
#   SSL Engine Switch:
#   Enable/Disable SSL for this virtual host.
SSLEngine on

#   A self-signed (snakeoil) certificate can be created by installing
#   the ssl-cert package. See
#   /usr/share/doc/apache2/README.Debian.gz for more info.
#   If both key and certificate are stored in the same file, only the
#   SSLCertificateFile directive is needed.
#SSLCertificateFile     /etc/ssl/certs/ssl-cert-snakeoil.pem
#SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
SSLCertificateFile      /var/www/cert/domain_dev.crt
SSLCertificateKeyFile   /var/www/cert/domain_dev.key

```

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

### copy folder
`cp -rf /source/path/ /destination/path/`

### Quick Empty trash
`rm -rf ~/.Trash/*`

### Secure Erase Empty Space

`diskutil secureErase freespace (level 0-4) /Volumes/(Drive Name)`

(level 0-4) is a number indicating the number of passes to write to the free space

Levels:

  - 0 – Single-pass zero-fill erase.
  - 1 – Single-pass random-fill erase.
  - 2 – US DoD 7-pass secure erase.
  - 3 – Gutmann algorithm 35-pass secure erase.
  - 4 – US DoE algorithm 3-pass secure erase.

*Warning!* It’s critically important that you include the **`freespace`** portion of that command. If you don’t, diskutil will happily start securely erasing the entire disk

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


