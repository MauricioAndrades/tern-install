#Installing Tern.js with Sublime Text and Atom

##What is it?

tern is an intelligent code analysis engine designed to be used inside text
editors. Tern's main features are:

	-Auto-completion on JS variables and properties.
	-Function argument hints.
	-Querying the type of an expression.
	-Definitions(with full Doc Support).
	-Automatic refactoring.

[Tern.js Documentation](http://ternjs.net/doc/manual.html)

##Pre Install Steps:
####Fixing npm permissions on `/usr/local` & Configuring Global Settings:
```shell

# cd into npm's installation folder:

cd `npm config get prefix` && echo "$(PWD)";

# find any files that do not belong to you:
# returns: [permissions]-[user]-[group]-[filename]
# 0:0 user is equal to root:wheel

find . -type f ! -user `whoami` -exec gstat -c "%a-%u-%g %N" '{}' \+ ;

```
####Optional: Make some backups just in case.
> Probably a good idea to backup your files before you go and change the
> permissions on a very critical folder. Either back up the actual files, or
> backup the permissions so you can look back and see what they were.

```shell
	
# option 1: backup using rsync --archive
# back up all files in `/usr/local` to '/tmp/local'

sudo rsync -a '/usr/local' '/tmp/local';

# option 2: make a tar-ball
# cd where you want the archive to be:

cd /path/to/where/you/want/the/backup/to/be;

# make a tar-ball archive of all your files.
# gtar -cpvzf 'filename.tar.gz' 'path-to-backup'

sudo gtar -cpvzf 'local.tar.gz' '/tmp/local'

# option 3: just backup the file permissions into a text file
# make sure you are where you think you are.
cd /usr/local;

# dump the permissions into a text file for later references:
sudo gfind '/usr/local' -exec gstat -c "%a-%u-%g %N" '{}' \+ >files.txt;

```

####Take ownership of all your files inside /usr/local:
```shell

# chown the /usr/local folder for proper operation of npm.

sudo chown -R `whoami` '/usr/local';

```
####Take a look at what you have installed globally so far:
```shell

# list global packages	
npm list -g --depth=0;
	
# list outdated packages
npm outdated -g --depth=0;
	
# view global config
npm config ls -l;
	
# update global packages npm
npm update -g;

```

> Install it __locally__ if you're going to __require()__ it.
> 
> Install it __globally__ if you're going to run it on the __command line__.


##Tern Installation:

####cd into ST3 Packages and clone a repo:
```shell
	
# Go to the location where package control install your packages:
cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/;
	
# Clone tern_for_sublime using git:
git clone git://github.com/ternjs/tern_for_sublime.git
	
```

####cd into the git-repo you just made and npm install tern's dependencies:
```shell

#
cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/tern_for_sublime/;


npm install;
	

npm install acorn --save

```


##Tern Sublime Settings:
####Make a Pref file in your ST3 User Prefs:
```shell

# ST3 settings that live inside packages are non-writable on purpuse so you can
# restore to a fresh state without having to reinstall.
# Any setting in the User space supercedes any other setting in ST3.
touch ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/Tern.sublime-settings;

```


####Get the Absolute PATH for Tern's bin/
```shell

# cd into tern_for_sublime's bin:
$ cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/tern_for_sublime/node_modules/tern/bin;
# copy the current path to your clipboard:
$ cpath;

```


####Make and Copy these Settings into your ST3 User Prefs Folder.
```json
{
    "tern_argument_hints": false,
    "tern_output_style": "tooltip",
    "tern_argument_completion": false,
    "tern_command": ["node", "[absolute path to]/Packages/tern_for_sublime/node_modules/tern/bin/tern"]
}
```


###Create the Settings File:
```json
	
```

##Using Tern:

####Working with Project Folders:
When you are working on a project, you should be

####.tern-project Files
Tern uses `.tern-project` files to manage importing 3rd party plugins and
managing settings on a per project basis. You should place a
`.tern-project` file in the root folder of the project you are working on. When
ST3 loads a project folder it will create an express server
instance that tern can load on. Loading `projects` is a good habit to get in.
ST3 symbol scraping, completions etc depend on it.

###Plugins:

###Useful snippet for .tern-project Files
```js

<snippet>
  <content><![CDATA[
{
    "ecmaVersion": 6,
    "libs": [
        "browser",
        "jquery"
    ],
    "loadEagerly": [
        "/js/**"
    ],
    "dontLoad": [
    "$TM_FILEPATH/node_modules/**"
    ],
    "plugins": {
        "angular": {},
        "complete_strings": {},
        "node-express": {},
        "node": {},
        "lint": {},
        "requirejs": {},
        "modules": {},
        "es_modules": {},
        "doc_comment": {
            "fullDocs": true
        }
    }
}
]]></content>
  <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
  <tabTrigger>tern-project</tabTrigger>
  <!-- Optional: Set a scope to limit where the snippet will trigger -->
  <!-- <scope>source.python</scope> -->
</snippet>

```

####default tern key commands:
If you want to to re-assing what keycommands trigger the differnt tern
capabilites you need to copy the default keycommands into your User's `Default
(OSX).sublime-keymap`

		Menu: Sublime-Text

###QuickTip:
add this key-command to your `Default (OSX).sublime-keymap` to open up or make
`settings` in `/Packages/User/`: i.e. If you are viewing an HTML file -> It will
make a settings file in your user space for all HTML files. Equivalent to
`Menu: Sublime Text -> Preferences -> Syntax Specific -> User`.

#####In SublimeText go to Menu:
`Menu: Sublime Text -> Preferences -> KeyBindings User`

add:

```json
{ "keys": ["super+alt+,"], "command": "open_file_settings"},
```
