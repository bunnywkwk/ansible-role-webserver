# ansible-role-webserver

Example Ansible role: installs nginx, writes an index page and opens the port in firewalld. It is also the template for new roles.

This repo holds role code only. CI comes from the central pipeline in [ansible-pipeline](https://github.com/bunnywkwk/ansible-pipeline): Jenkins builds any branch that contains `meta/main.yml`.
