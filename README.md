# Koha Item Recalls plugin

This Koha plugin that adds the ability to place recalls on items.

# Introduction

Koha’s Plugin System (available in Koha 3.12+) allows for you to add additional tools and reports to [Koha](http://koha-community.org) that are specific to your library. Plugins are installed by uploading KPZ ( Koha Plugin Zip ) packages. A KPZ file is just a zip file containing the perl files, template files, and any other files necessary to make the plugin work. Learn more about the Koha Plugin System in the [Koha 3.22 Manual](http://manual.koha-community.org/3.22/en/pluginsystem.html) or watch [Kyle’s tutorial video](http://bywatersolutions.com/2013/01/23/koha-plugin-system-coming-soon/).

# Downloading

From the [release page](https://github.com/bywatersolutions/koha-plugin-item-recalls/releases) you can download the relevant *.kpz file

# Installing

Koha's Plugin System allows for you to add additional tools and reports to Koha that are specific to your library. Plugins are installed by uploading KPZ ( Koha Plugin Zip ) packages. A KPZ file is just a zip file containing the perl files, template files, and any other files necessary to make the plugin work.

The plugin system needs to be turned on by a system administrator.

To set up the Koha plugin system you must first make some changes to your install.

* Change `<enable_plugins>0<enable_plugins>` to `<enable_plugins>1</enable_plugins>` in your koha-conf.xml file
* Confirm that the path to `<pluginsdir>` exists, is correct, and is writable by the web server
* Restart your webserver
* Restart memcached if you are using it

Once set up is complete you will need to alter your UseKohaPlugins system preference. On the Tools page you will see the Tools Plugins and on the Reports page you will see the Reports Plugins.

# Setup

* Install the plugin
* Ensure each HOLD notice ends with the following code:

```
--
ID: <<reserves.reserve_id>>.
--
```

* Add the following to each staff and opac section of your Apache config:

```apache
Alias /plugin "/var/lib/koha/kohadev/plugins"
# The stanza below is needed for Apache 2.4+
<Directory /var/lib/koha/kohadev/plugins>
      Options Indexes FollowSymLinks ExecCGI
      AddHandler cgi-script .pl
      AllowOverride None
      Require all granted
</Directory>
```

* Set up the nightly cronjob
* Tie the regular cronjob to the cronjob for process_message_queue.pl so it always run before it


#Staff Client Process

When you install the plug in, it does three things:
Creates a table called Plug_In_recalls - has added Reserve ID and the item number
Creates two new notices: RECALL_Plugin and RECALL_pickup
Goes through the Holds notices and adds the Reserve ID.


For Recalls to happen: 
Item must be checked out
Item must be on hold
For a patron to place a recall on the hold- they need to be the first person on hold and the hold must be an item level hold.
Must have a rule allowing item recall

