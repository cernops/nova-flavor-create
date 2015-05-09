CERN Flavor Create
==================

What is it?
-----------
It's a small tool that creates flavors in a cell environment.
It creates a flavor with the same row ID in all available cells.

The flavors in the children cells are always created as public.
In order to delete, disable or manage the the flavor with spefic tenants use 
nova api in the parent cell. 


How to use it?
--------------
To see the available options run:

$python cern-add-flavor -h

usage: cern-add-flavor [-h] [--ephemeral EPHEMERAL] [--swap SWAP]
                       [--rxtx_factor RXTX_FACTOR] [--is_public IS_PUBLIC]
                       [--config CONFIG]
                       name id ram disk vcpus

positional arguments:
  name                  Name of the new flavor
  id                    UniqueID (integer or UUID) for the new flavor. If
                        specifying 'auto', a UUID will be generated as id
  ram                   Memory size in MB
  disk                  Disk size in GB
  vcpus                 Number of vcpus

optional arguments:
  -h, --help            show this help message and exit
  --ephemeral EPHEMERAL
                        Ephemeral space size in GB (default 0)
  --swap SWAP           Swap space size in MB (default 0)
  --rxtx_factor RXTX_FACTOR
                        RX/TX factor (default 1)
  --is_public IS_PUBLIC
                        Make flavor accessible to the public (default False)
  --config CONFIG       Configuration file


Configuration File
------------------

The configuration file should contain the DBs locations for all cells and
should be explicitly defined with the option --config
Example for an infrastructure with a parent cell and one child.

[default]
available_cells=cell_parent,cell_child

[cell_parent]
cell_type=parent
db_type=mysql
db_location=parent_db.cern.ch
db_port=3306
user=nova_user
password=db_parent_password
database=nova_parent

[cell_child]
cell_type=child
db_type=mysql
db_location=child_db.cern.ch
db_port=3306
user=nova_user
password=db_child_password
database=nova_child


Example
-------

To create a flavor: 
./cern-add-flavor --config my_cells_dbs.conf my_flavor auto 2048 20 2



Nova versions supported
-----------------------
We use it in Icehouse and Juno.


Bugs and Disclaimer
-------------------
Bugs? Oh, almost certainly.

This tool was written to be used in the CERN Cloud Infrastructure and
it has been tested only in our environment.

Since it updates nova DB use it with extremely caution.
