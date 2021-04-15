NOTE: The content_rhel role is not released, but will be in an upcoming release of the FAM collection. For the purposes of this demo, we will install the development branch.
To isolate this installation of the development branch from the rest of the system, we'll install it in a virtual environment.

1. Install Katello from the documentation or using Forklift. Give it an initial organization like "Demo Organization"
2. Clone this project: `git clone https://github.com/wbclark/content_rhel_demo.git` and `cd content_rhel_demo`
3. Create a virtual environment: `python3 -m venv env`
4. Activate the virtual environment: `source env/bin/activate`
5. Install dependencies: `pip install ansible apypie`
6. Install FAM collection: `ansible-galaxy collection install git+https://github.com/theforeman/foreman-ansible-modules.git`
7. Copy and Edit Server Variables from template: `cp vars/server.yml{.example,}` and `vim vars/server.yml`
8. Edit vars in demo playbook to set desired behavior: `vim content_rhel_demo.yml`
9. Open Katello WebUI or observe Katello logs in terminal: `foreman-tail`
10. Run the demo playbook: `ansible-playbook content_rhel_demo.yml`
11. When done, deactivate the virtual environment: `deactivate`
