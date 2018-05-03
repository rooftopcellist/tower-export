# Tower CLI Import/Export Feature Usage

## Introduction

This feature allows AWX users to export resources from a running tower node, and import them into another 
tower node.  This tool is especially useful for users who want to upgrade from an old version of AWX when 
the upgrade path is not seamless as a result of database changes, etc.  

This allows you to export your job templates and other objects (not including credential secrets) to a JSON 
file, which you can then re-import to a freshly installed AWX version of your choosing.  

Currently, this tool does not support export/import of the following:
* Logs/history
* Credential passwords
* LDAP/AWX config

### Install & Configure Tower-CLI

In terminal, pip install tower-cli (if you do not have pip already, install [here](https://pip.pypa.io/en/stable/installing/)):
```
$ pip install ansible-tower-cli
```

The AWX host URL, user, and password must be set for the AWX instance to be exported:
```
$ tower-cli config host <tower.example.com>
$ tower-cli config username <user>
$ tower-cli config password <pass>
```

For more information on installing tower-cli look [here](http://tower-cli.readthedocs.io/en/latest/quickstart.html).


### Export Resources

Export all objects

```$ tower-cli receive --all > assets.json```



### Teardown Old AWX

Clean up remnants of the old AWX install:

```docker rm -f $(ps -aq)     # remove all old awx containers```

```make clean-ui              # clean up ui artifacts```


### Install New AWX version

If you are installing AWX as a dev container, pull down the latest code or version you want from GitHub, build
the image locally, then start the container

```
git clone git@github.com:ansible/awx.git        # clone AWX repository
make docker-compose-build                       # build AWX image
make docker-compose                             # run container
```
For other install methods, refer to the [Install.md](https://github.com/ansible/awx/blob/devel/INSTALL.md). 


### Import Resources


Import from a JSON file named assets.json

```
$ tower-cli send assets.json
```

--------------------------------------------------------------------------------

## Additional Info

If you have two running AWX/Tower instances, it is possible to copy all assets from one instance to another

```$ tower-cli receive --tower-host tower1.example.com --all | tower-cli send --tower-host tower2.example.com```



#### More Granular Exports:

Export all credentials

```$ tower-cli receive --credential all > credentials.json```
> Note: This exports the credentials with blank strings for passwords and secrets

Export a credential named "My Credential"

```$ tower-cli receive --credential "My Credential"```

#### More Granular Imports:


You could import anything except an organization defined in a JSON file named assets.json

```$ tower-cli send --prevent organization assets.json```



