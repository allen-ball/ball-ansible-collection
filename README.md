ball-ansible-collection
=======================

A collection of miscellaneous [Ansible] roles.

| Role                                         | Description               |
|----------------------------------------------|---------------------------|
| [roles/dump-variables](roles/dump-variables) | Dumps [Ansible] variables |

Add to `requirements.yml`:

```yml
collections:
  - name: https://github.com/allen-ball/ball-ansible-collection.git
    type: git
    version: trunk
```


[Ansible]: https://www.ansible.com/
