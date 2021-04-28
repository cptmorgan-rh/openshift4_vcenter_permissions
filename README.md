OpenShift 4 Minimum vCenter Permissions
===========================================

## Description
------------

**Note: This branch uses Ansible Modules. Please switch to the govc branch for the playbook that uses govc**

The ansible playbook in this repository will create and apply the required roles for the minimum permissions required to deploy OpenShift 4 on vCenter. This has been tested with the IPI and UPI methods.

The permissions are obtained from the Official OpenShift Installer Docs on GitHub.

[vCenter: Required Privileges & Permissions](https://github.com/openshift/installer/blob/master/docs/user/vsphere/privileges.md)

The user/service account that will be used by OpenShift **must** be created in vCenter prior to running this playbook.

If specifying a folder it should be created prior to running this playbook however the playbook can create the folder if desired.

## Quickstart

Start by editing the `group_vars/all.yml` file:

+ Set the vCenter variables
    1. IP/Host Name of vCenter
    2. vCenter Network
    3. Datastore name
    4. Datacenter name
    5. Cluster name
    6. Network Name
    7. username and passwords of vCenter Account
    8. Folder path - for example:
       * openshift4
       * Red Hat/Openshift/4

+ Set the OpenShift variables
    1. Specify Service Account that will be used by OpenShift in vCenter
    2. Specify if the folder has been previously created
       * If using a sub-folder the **entire** folder path must have been previously created.
       * e.g. 'Red Hat/OpenShift/' must exist if you specify 'Red Hat/OpenShift/4' as the folder path.

**Note**

Due to the two methods used to deploy OpenShift on vCenter you must decide the permissions required. If the openshift installer will be creating the folder then you must set the variable `openshift.installer_created_folder` to True. Doing so will apply the Datacenter level permissions required by the openshift installer to create the required folder. If you are creating the folder prior to running the installer than you must set the variable 'openshift.installer_created_folder' to False and the folder must be create prior to running the script or by having the script create the folder for you. This will set the required permissions on the folder specified to minimize the amount of permissions required by the service account.

## Infrastructure Prerequisites

1. vSphere ESXi and vCenter 6.7 or 7.0 installed.
2. A datacenter created with a vSphere host added to it and a datastore exists that has adequate capacity.
3. Ansible (preferably latest) on the machine where this repo is cloned.
    1. Before you install Ansible, install the `epel-release`, run `yum -y install epel-release`
4. PyVmomi installed on your local machine. 

### Expected Outcome

1. All roles required will be created.
2. All roles created will be applied to their specific entity in vCenter
3. VM Folder will be created if specified in the group vars.

AUTHOR
------
Morgan Peterman
