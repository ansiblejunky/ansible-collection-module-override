# Override a module within an Ansible Collection

This repository shows how to override a single module that exists inside an Ansible Collection. You might want to do this if you have some adjustments or fixes to apply to a specific module.

The repository is structured as a "playbook" repository however you can implement this within an Ansible Role in a similar fashion. This would be the optimal way of doing it as it's reusable.

## Steps

Below are the simple steps to use your customized python module.

1. Create `library` folder within the repository
2. Copy the original module from the collection and place it inside the `library` folder
3. Rename the module inside the `library` folder using a prefix based on your company name. For example `abc_archive.py`. This is critical as it will have Ansible use your module instead.
4. Adjust all your Ansible code/content to reference the new module name instead of the old collection module name. Do not use namespaces at all. Simply reference the module name in its simple form. See [test.yml](./test.yml) as an example.
5. Run your Ansible code using `-vvvv` and the debug statements for the task will prove that Ansible is now using your customized module.

```shell
TASK [Compress directory /path/to/foo/ into /path/to/foo.tgz] ********************************************************************************
...
Using module file my_archive.py
...
```

## Example

This repository provides a simple example where we override the `archive.py` module that comes with the Ansible Community General collection. We want to use our own version of `archive`! So the python file was copied to the `library` folder and renamed to `my_archive.py`. Additionally, the code in the Playbook `test.yml` references the new module name.

To run this test, do the following:

```shell
# Clone repository to local
git clone <repo>

# Download collection
ansible-galaxy collection install -r collections/requirements.yml -p collections/ -f

# Run playbook
ansible-playbook test.yml -vvvv
```

Notice the module that is used for the `archive` task.
