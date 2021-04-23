# Configuring Katello using Ansible Automation to Synchronize and Serve RHEL Repositories #

## What is this demo about? ##

The [content_rhel role](https://github.com/theforeman/foreman-ansible-modules/tree/develop/roles/content_rhel) is a soon to be released role in the [foreman collection](https://galaxy.ansible.com/theforeman/foreman) on Ansible Galaxy. This role configures [Katello](https://theforeman.org/plugins/katello/), one of several upstream components behind [Red Hat Satellite](https://www.redhat.com/en/technologies/management/satellite), to synchronize [RHEL](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) repositories and serve them to registered clients. The full steps include uploading a subscription manifest, enabling and optionally syncing RHEL7 and RHEL8 repositories, creating a sync plan to schedule future syncs automatically, and creating an activation key to use when registering RHEL clients.

For more details, See the [live recording](https://youtu.be/k0spcDCIYYU?t=176) from the Foreman Community Demo #93 series. The content_rhel demo starts around 2:56 and lasts around ten minutes.

## What do I need to run the demo? ##

1. A Red Hat Login with the ability to create a [Subscription Allocation](https://access.redhat.com/management/subscription_allocations). Craete a Subscription Allocation for the demo and assign at least one subscription providing RHEL repositories to it. Download the Subscription Manifest (manifest.zip file) and copy it to the location where you will run the demo.

N.B. The `content_rhel` role configures Katello to provide RHEL content, and therefore requires a Red Hat Subscription Manifest providing RHEL content. We would like to expand the collection with roles providing other operating systems as well. Such a role might drop the manifest import step, add custom_credentials if any are needed for the OS, and use custom repositories instead of Red Hat repository sets.

[Pull Requests](https://github.com/theforeman/foreman-ansible-modules/pulls) are welcomed and there is a currently in-progress [guide for role developers](https://github.com/theforeman/foreman-ansible-modules/pull/1186/files).

2. A live Katello instance to configure. You can install one following the [install documentation](https://theforeman.org/plugins/katello/3.18/installation/index.html) or using [Forklift](https://github.com/theforeman/forklift). You will need to create[1] a new organization such as "Demo Organization" where the configuration from the `content_rhel` role will be isolated. Still, it's best to not use your production Satellite Server for this demo if you care at all about the load it will create from syncing repositories! :)

[1] Via WebUI: organizations menu (near top left) --> manage organizations --> new organization --> set the organization name and use all defaults for other values.

3. As of this writing the `content_rhel` role is not yet included in a released version of the [foreman collection](https://galaxy.ansible.com/theforeman/foreman) on [Ansible Galaxy](https://galaxy.ansible.com/); therefore this demo includes all of the necessary steps to create a Python virtual environment where we will install the latest development branch of the collection in a way that is completely isolated from the rest of your system.

## How do I run the demo? ##

1. Clone the demo project from GitHub to your machine:

```shell
$ git clone https://github.com/wbclark/content_rhel_demo.git
$ cd content_rhel_demo
```

2. Create and Activate a Python Virtual Environment for the demo projects:

```shell
$ python3 -m venv env
$ source env/bin/activate
```

2.a. You can confirm that you are using the virtual environment's Python from this point, by checking:

```shell
$ which python
$ python --version
```

3. Install Ansible and apypie (library for communicating with Katello API) to the virtual environment:

```shell
$ pip install ansible apypie
```

3.a. You can confirm that you are using the virtual environment's Ansible from this point, by checking:

```shell
$ which ansible
$ ansible --version
```

4. Install the development branch of the Foreman collection to the virtual environment's Ansible:

```shell
$ ansible-galaxy collection install git+https://github.com/theforeman/foreman-ansible-modules.git
```

5. Copy the server variables template and edit your server variables file to point to your live Katello instance:

```shell
$ cp vars/server.yml/{.example,}
$ vim vars/server.yml
```

6. Inspect the demo playbook. Optionally, edit role variables to set desired behavior.

```shell
$ vim content_rhel_demo.yml
```

For the complete list of supported role variables, refer to the `content_rhel` role [README.md](https://github.com/theforeman/foreman-ansible-modules/blob/develop/roles/content_rhel/README.md)

7. If you so desire, open the Katello WebUI to the Monitor --> Tasks page to directly observe progress, or directly observe system logs via a terminal session.

```shell
$ foreman-tail                             # tail all logs, including candlepin and pulp logs
$ tail -f /var/log/foreman/production.log  # quieter alternative, production.log only
```

8. Run the demo playbook.

```shell
$ ansible-playbook content_rhel_demo.yml
```

9. When the playbook is complete, find the applied confiugrations on the Katello WebUI. With the Organization context set to "Demo Organization". Some hints: check Content --> Sync Status to see whether repositories synced or are syncing; check Content --> Subscriptions to see the uploaded subscription manifest; check Content --> Sync Plans to check the sync plan that the role created; check Content --> Activation Keys to see the activation key that the role created; check Monitor --> Tasks to see status of any tasks that ran or are running as part of the role.

## How do I clean up when finished? ##

1. When you are finished with the demo, it's time to clean up. First delete the "Demo Organization" on Katello if you will no longer use it (you can delete the organization from the same Manage Organizations page on the Katello WebUI where you created it). Once the organization is deleted and the subscription manifest is freed up, delete the corresponding Subscription Allocation by logging into the Red Hat Customer Portal, navigating to Subscription Allocations, and deleting the Subscription Allocation to release its subscriptions back to the available pool.

2. Finally deactivate the Python virtual environment and confirm that you are once again using your system's Python and Ansible:

```shell
$ deactivate
$ which python
$ python --version
$ which ansible
$ ansible --version
```

## Where can I contribute, discuss, provide feedback, or find more information? ##

Happy Automating and thanks for checking out the demo! Please let us know what you think.

Please direct issues related to this demo to the issues tab on [this repository](https://github.com/wbclark/content_rhel_demo).

For Foreman Ansible Collection feedback, including bug reports, feature requests, and pull requests, please visit the [FAM Repository](https://github.com/theforeman/foreman-ansible-modules) on GitHub.

For other feedback and discussion, please use the [community forum](https://community.theforeman.org/).

Find other Foreman community events and demos on the [community calendar](https://community.theforeman.org/calendar).

For previous live demos and other recorded community events, visit the [Foreman Community Channel](https://www.youtube.com/channel/UCCo7AZ1oG6TbG0-dwjRqCmw) on YouTube.
