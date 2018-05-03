# Tower CLI Import/Export Feature Usage

## Introduction

This feature allows AWX users to export resources from a running tower node, and import them into another 
tower node.  This tool is especially useful for users who want to upgrade from an old version of AWX when 
the upgrade path is not seamless as a result of database changes, etc.  

### Install & Configure Tower-CLI

Install with pip:
```
$ pip install ansible-tower-cli
```

The AWX host URL, user, and password must be set for the AWX instance to be exported:
```
$ tower-cli config host tower.example.com
$ tower-cli config username user
$ tower-cli config username pass
```

For more information on installing tower-cli look [here](http://tower-cli.readthedocs.io/en/latest/quickstart.html).


###




