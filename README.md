1. Create a virtual environment: `python3 -m venv env`
2. Activate the virtual environment: `source env/bin/activate`
3. Install dependencies: `pip install ansible apypie`
4. Install FAM collection: `ansible-galaxy collection install git+https://github.com/theforeman/foreman-ansible-modules.git`
5. Copy and Edit vars/server.yml.example: `cp vars/server.yml{.example,}` and `vim vars/server.yml`
