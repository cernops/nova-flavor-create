CERN Flavor Create
==================

What is it?
-----------
It's a small tool that creates flavors in a cell environment. <br />
It creates a flavor with the same row ID in all available cells. <br />

The flavors in the children cells are always created as public. <br />
In order to delete, disable or manage the the flavor with spefic tenants use
nova api in the parent cell. 


How to use it?
--------------
To see the available options run:

$python cern-flavor-create -h

usage: cern-flavor-create [-h] [--ephemeral EPHEMERAL] [--swap SWAP] 
                          [--rxtx_factor RXTX_FACTOR] [--is_public IS_PUBLIC] 
                          [--config CONFIG] 
                          name id ram disk vcpus

positional arguments:

name - Name of the new flavor <br />
id - UniqueID (integer or UUID) for the new flavor. If specifying 'auto', a UUID will be generated as id <br />
ram - Memory size in MB <br />
disk - Disk size in GB <br />
vcpus - Number of vcpus <br />

optional arguments:

-h, --help - show this help message and exit <br />
--ephemeral EPHEMERAL - Ephemeral space size in GB (default 0) <br />
--swap SWAP - Swap space size in MB (default 0) <br />
--rxtx_factor RXTX_FACTOR - RX/TX factor (default 1) <br />
--is_public IS_PUBLIC - Make flavor accessible to the public (default False) <br />
--config CONFIG - Configuration file <br />


Configuration File
------------------

The configuration file should contain the DBs locations for all cells and
should be explicitly defined with the option --config <br />
Example for an infrastructure with a parent cell and one child.

[default] <br />
available_cells=cell_parent,cell_child <br />
<br />
[cell_parent] <br />
cell_type=parent <br />
db_type=mysql <br />
db_location=parent_db.cern.ch <br />
db_port=3306 <br />
user=nova_user <br />
password=db_parent_password <br />
database=nova_parent <br />
<br />
[cell_child] <br />
cell_type=child <br />
db_type=mysql <br />
db_location=child_db.cern.ch <br />
db_port=3306 <br />
user=nova_user <br />
password=db_child_password <br />
database=nova_child <br />


Example
-------

To create a flavor: <br />
./cern-flavor-create --config cern-flavor-create.conf my_flavor auto 2048 20 2



Nova versions supported
-----------------------
We use it in Icehouse and Juno.


Bugs and Disclaimer
-------------------
Bugs? Oh, almost certainly.

This tool was written to be used in the CERN Cloud Infrastructure and
it has been tested only in our environment.

Since it updates nova DB use it with extremely caution.
