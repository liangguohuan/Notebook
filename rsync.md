## Warn

`source` folder must end with `/` , otherwise `source` folder will sync as `target` subdir



In this post I cover the basics of `rsync`, in preparation for a subsequent post that will cover backups and it's use in conjunction with cronjobs to automatize the backup process. From the copying and synchronization of local files and folders, to it's use for transfer information among computers. Its [use as a daemon when SSH is unavailable](http://www.jveweb.net/en/archives/2011/01/running-rsync-as-a-daemon.html)was moved to it's own section.

**Topics**
[The basics of rsync](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_01)
[Copying local files and folders](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_02)
[Dealing with whitespace and rare characters](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_03)
[Update the contents of a folder](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_04)
[Synchronizing two folders with rsync](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_05)
[Compressing the files while transferring them](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_06)
[Transferring files between two remote systems](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_07)
[Excluding files and directories](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_08)
[Running rsync as a daemon](http://www.jveweb.net/en/archives/2011/01/running-rsync-as-a-daemon.html) (moved to it's own section)
[Some additional rsync parameters](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_10)
[Footnotes](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_11)

### The basics of rsync

`rsync` is a very versatile copying and backup tool that is included by default in almost every Linux distribution. It can be used as an advanced copying tool, allowing us to copy files both locally and remotely. It can also be used as a backup tool. It supports the creation of incremental backups.

`rsync` counts with a famous delta-transfer algorithm that allows us to transfer new files as well as recent changes to existent files, while ignoring unchanged files. Additionally to this, the behavior of `rsync` can be throughly customized, helping us to automatize backups, it can also be run as a daemon to turn the computer into a host and allow `rsync` clients connect to it.

Besides the copying of local files and folders, `rsync` allow us to copy over SSH (Secure Shell), RSH (Remote Shell) and it can be run as a daemon in a computer and allow other computers to connect to it, when `rsync` is run as a daemon it listens to the port TCP 873.

When we use `rsync` as a daemon or when we use RSH, the data that is send between computers travels unencrypted, so, if you are transferring files between two computers in the same local network, this is useful, but this shouldn't be used to transfer files over insecure networks, such as the Internet. For this purpose SSH is the way to go.

This is the main reason why I favor the use of SSH for my transfers, besides, since SSH is secure, many servers have the SSH daemon available. But the use of `rsync` as a daemon for transfers over fast connections, as is usually the case in a local network, is useful. I don't have the RSH daemon running in my computers so you may find me a bit biased about SSH in the examples. The examples covering the transfer of files between two computers use SSH as the medium of transport, but in a separate post I cover the use of rsync as a daemon.

### Copying local files and folders

To copy the contents of one local folder into another, replacing the files in the destination folder, we use:

```
rsync -rtv source_folder/ destination_folder/
```

In the source_folder notice that I added a slash at the end, doing this prevents a new folder from being created, if we don't add the slash, a new folder named as the source folder will be created in the destination folder. So, if you want to copy the contents of a folder called Pictures into an existent folder which is also called Pictures but in a different location, you need to add the trailing slash, otherwise, a folder called Pictures is created inside the Pictures folder that we specify as destination.

```
rsync -rtv source/ destination/
```
![A graphical representation of the results of rsync with a trailing slash in the source folder.](http://www.jveweb.net/en/media/trailingslash.png)

```
rsync -rtv source destination/
```
![A graphical representation of the results of rsync without a trailing slash in the source folder.](http://www.jveweb.net/en/media/notrailingslash.png)

The parameter `-r` means recursive, this is, it will copy the contents of the source folder, as well as the contents of every folder inside it.

The parameter `-t` makes rsync preserve the modification times of the files that it copies from the source folder.

The parameter `-v` means verbose, this parameter will print information about the execution of the command, such as the files that are successfully transferred, so we can use this as a way to keep track of the progress of rsync.

This are the parameters that I frequently use, as I am usually backing up personal files and this doesn't contain things such as symlinks, but another very useful parameter to use rsync with is the parameter -a.

```
rsync -av source/ destination/
```

The parameter `-a` also makes the copy recursive and preserve the modification times, but additionally it copies the symlinks that it encounters as symlinks, preserve the permissions, preserve the owner and group information, and preserve device and special files. This is useful if you are copying the entire home folder of a user, or if you are copying system folders somewhere else.

### Dealing with whitespace and rare characters

We can escape spaces and rare characters just as in bash, by the use of \ before any whitespace and rare character. Additionally, we can use single quotes to enclose the string:

```
rsync -rtv so\{ur\ ce/ dest\ ina\{tion/
rsync -rtv 'so{ur ce/' 'dest ina{tion/'
```

### Update the contents of a folder

In order to save bandwidth and time, we can avoid copying the files that we already have in the destination folder that have not been modified in the source folder. To do this, we can add the parameter `-u` to `rsync`, this will synchronize the destination folder with the source folder, this is where the delta-transfer algorithm enters. To synchronize two folders like this we use:

```
rsync -rtvu source_folder/ destination_folder/
```

By default, `rsync` will take into consideration the date of modification of the file and the size of the file to decide whether the file or part of it needs to be transferred or if the file can be left alone, but we can instead use a hash to decide whether the file is different or not. To do this we need to use the `-c` parameter, which will perform a checksum in the files to be transferred. This will skip any file where the checksum coincides.

```
rsync -rtvuc source_folder/ destination_folder/
```

### Synchronizing two folders with rsync

To keep two folders in synchrony, not only do we need to add the new files in the source folder to the destination folder, as in the past topics, we also need to remove the files that are deleted in the source folder from the destination folder. `rsync` allow us to do this with the parameter `--delete`, this used in conjunction with the previously explained parameter `-u` that updates modified files allow us to keep two directories in synchrony while saving bandwidth.

```
rsync -rtvu --delete source_folder/ destination_folder/
```

The deletion process can take place in different moments of the transfer by adding some additional parameters:

- rsync can look for missing files and delete them before it does the transfer process, this is the default behavior and can be set with `--delete-before`
- rsync can look for missing files after the transfer is completed, with the parameter `--delete-after`
- rsync can delete the files done during the transfer, when a file is found to be missing, it is deleted at that moment, we enable this behavior with `--delete-during`
- rsync can do the transfer and find the missing files during this process, but instead of delete the files during this process, waits until it is finished and delete the files it found afterwards, this can be accomplished with `--delete-delay`

e.g.:

```
rsync -rtvu --delete-delay source_folder/ destination_folder/
```

### Compressing the files while transferring them

To save some bandwidth, and usually it can save some time as well, we can compress the information being transfer, to accomplish this we need to add the parameter `-z` to rsync.

```
rsync -rtvz source_folder/ destination_folder/
```

Note, however, that if we are transferring a large number of small files over a fast connection, rsync may be slower with the parameter -z than without it, as it will take longer to compress every file before transfer it than just transferring over the files. Use this parameter if you have a a connection with limited speed between two computers, or if you need to save bandwidth.

### Transferring files between two remote systems

`rsync` can copy files and synchronize a local folder with a remote folder in a system running the SSH daemon, the RSH daemon, or the rsync daemon. The examples here use SSH for the file transfers, but the same principles apply if you want to do this with `rsync` as a daemon in the host computer, read [Running rsync as a daemon when ssh is not available](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_09) further below for more information about this.

To transfer files between the local computer and a remote computer, we need to specify the address of the remote system, it may be a domain name, an IP address, or a the name of a server that we have already saved in our SSH config file (information about how to do this can be found in [Defining SSH servers](http://www.jveweb.net/en/archives/2010/08/defining-ssh-servers.html)), followed by a colon, and the folder we want to use for the transfer. Note that rsync can not transfer files between two remote systems, only a local folder or a remote folder can be used in conjunction with a local folder. To do this we use:

Local folder to remote folder, using a domain, an IP address and a server defined in the SSH configuration file:
```
rsync -rtvz source_folder/ user@domain:/path/to/destination_folder/
rsync -rtvz source_folder/ user@xxx.xxx.xxx.xxx:/path/to/destination_folder/
rsync -rtvz source_folder/ server_name:/path/to/destination_folder/
```

Remote folder to local folder, using a domain, an IP address and a server defined in the SSH configuration file:
```
rsync -rtvz user@domain:/path/to/source_folder/ destination_folder/
rsync -rtvz user@xxx.xxx.xxx.xxx:/path/to/source_folder/ destination_folder/
rsync -rtvz server_name:/path/to/source_folder/ destination_folder/
```

### Excluding files and directories

There are many cases in which we need to exclude certain files and directories from rsync, a common case is when we synchronize a local project with a remote repository or even with the live site, in this case we may want to exclude some development directories and some hidden files from being transfered over to the live site. Excluding files can be done with `--exclude` followed by the directory or the file that we want to exclude. The source folder or the destination folder can be a local folder or a remote folder as explained in the previous section.

```
rsync -rtv --exclude 'directory' source_folder/ destination_folder/
rsync -rtv --exclude 'file.txt' source_folder/ destination_folder/
rsync -rtv --exclude 'path/to/directory' source_folder/ destination_folder/
rsync -rtv --exclude 'path/to/file.txt' source_folder/ destination_folder/
```

The paths are relative to the folder from which we are calling rsync unless it starts with a slash, in which case the path would be absolute.

Another way to do this is to create a file with the list of both files and directories to exclude from rsync, as well as patterns (all files that would match the pattern would be excluded, *.txt would exclude any file with that extension), one per line, and call this file with `--exclude-from` followed by the file that we want to use for the exclusion of files. First, we create and edit this file in our favorite text editor, in this example I use gedit, but you may use kate, Vim, nano, or any other text editor:

```
touch excluded.txt
gedit excluded.txt
```

In this file we can add the following:

```
directory
relative/path/to/directory
file.txt
relative/path/to/file.txt
/home/juan/directory
/home/juan/file.txt
*.swp
```

And then we call rsync:

```
rsync -rvz --exclude-from 'excluded.txt' source_folder/ destination_folder/
```

In addition to delete files that have been removed from the source folder, as explained in [Synchronizing two folders with rsync](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html#jveweb_en_014_05), `rsync` can delete files that are excluded from the transfer, we do this with the parameter `--delete-excluded`, e.g.:

```
rsync -rtv --exclude-from 'excluded.txt' --delete-excluded source/ destination/
```

This command would make `rsync` recursive, preserve the modification times from the source folder, increase verbosity, exclude all the files that match the patterns in the file `excluded.txt`, and delete all of this files that match the patternif they exist in the destination folder.

### Running rsync as a daemon when ssh is not available

This was moved to it's own section, [Running rsync as a daemon](http://www.jveweb.net/en/archives/2011/01/running-rsync-as-a-daemon.html).

### Some additional rsync parameters

`-t` Preserves the modification times of the files that are being transferred.
`-q` Suppress any non-error message, this is the contrary to -v which increases the verbosity.
`-d` Transfer a directory without recursing, this is, only the files that are in the folder are transferred.
`-l` Copy the symlinks as symlinks.
`-L` Copy the file that a symlink is pointing to whenever it finds a symlink.
`-W` Copy whole files. When we are using the delta-transfer algorithm we only copy the part of the archive that was updated, sometimes this is not desired.
`--progress` Shows the progress of the files that are being transferred.
`-h` Shows the information that rsync provides us in a human readable format, the amounts are given in K's, M's, G's and so on.

### Footnotes

The amount of options that `rsync` provide us is immense, we can define exactly which files we want to transfer, what specific files we want to compress, what files we want to delete in the destination folder if this files exists, and we can deal with system files as well, for more information we can use `man rsync` and `man rsyncd.conf`

I leave the information concerning backups out of this post, as this will be covered, together with the automation of the backups, in an incoming post.

It is possible to run rsync on Windows with the use of [cygwin](http://www.cygwin.com/), however I don't have a Windows box available at the moment (nor do I plan to aquire one in the foreseeable future), so even thought I have done it I can't post about this. If you run `rsync` as a service in Windows tho, you need to add the line "`strict mode = false`" in `rsyncd.conf` under the modules area, this will prevent `rsync` from checking the permissions in the secrets file and thus failing because they are not properly set (as they don't work the same as in Linux).

This post may be updated if there is something to correct or to add a little more information if I see it necessary.

Categories: [Commands](http://www.jveweb.net/en/archives/commands/index.html), [FOSS](http://www.jveweb.net/en/archives/foss/index.html), [Linux](http://www.jveweb.net/en/archives/linux/index.html), [rsync](http://www.jveweb.net/en/archives/rsync/index.html)
