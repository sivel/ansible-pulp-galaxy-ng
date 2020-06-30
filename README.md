# Playbook to create a working pulp/pulp_ansible/galaxy_ng container via ansible

This playbook uses https://hub.docker.com/r/pulp/pulp-galaxy-ng to create a Docker container
running `pulp`, `pulp_ansible`, and `galaxy_ng`.

## Use

```
$ ansible-playbook -v playbook.yml
```

Make sure to pay attention to the `debug` outputs at the end of the play. A pre-populated `ansible.cfg` will be created at `{{ playbook_dir }}/out/ansible.cfg`

### Pre-create namespaces in galaxy_ng

```
$ ansible-playbook -v -e '{namespaces: [ns1, ns2]}' playbook.yml
```

### Assumptions

1. Running `ansible-base` 2.10+ with the `community.general` collection installed, for access to the `docker` modules.
1. Running Docker, with env configurations to access the docker server
1. Python `docker` module installed and available to the playbook python (implicit localhost)
1. No other containers named `pulp-galaxy-ng`

## Notes

1. The `repository` and `distribution` are shared between `pulp_ansible` and `galaxy_ng`. As such, collections published to `galaxy_ng` will appear in `pulp_ansible` as well.
1. Due to `galaxy_ng` storing it's own information for an uploaded collection, it's best to upload to `galaxy_ng`, if you plan on interacting with both `galaxy_ng` and `pulp_ansible`
1. `galaxy_ng` requires pre-creating namespaces before collections can be uploaded, this can be done using the admin username and password in the UI by opening the URL root in a browser, or by using the extra vars example above.
1. This playbook nukes all distributions and repositories before configuring pulp.
