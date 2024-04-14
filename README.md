##### Treat your Ansible content like code

In 2013, my journey with Ansible began. At that time, I served as a Network Operations Engineer for an Internet Service Provider (ISP) that heavily embraced open source solutions across its operations. Within our Network Operations Center (NOC), we had distinct teams for Network Engineering, 

- Systems Engineering and 
- Network Engineering 

Although all engineers were expected to possess knowledge in both networks and systems, we were encouraged to specialize in one area to enhance the efficiency of the NOC team. I was a member of the Systems Engineering team, which was tasked with overseeing all hosted services, including SNMP, HTTP, SMTP, DNS, and the maintenance of KVM virtual servers hosting these services.

With the ISP experiencing a substantial growth in its customer base, we initiated discussions about implementing automation for all hosted services - SMTP (Postfix), HTTP (Apache), and DNS services.

#### Approach [adopted from Redhat best practices]

Version control your Ansible content
- Iterate
    - Start with a basic playbook and static inventory
    - Refactor and modularize later


- Create a style guide for consistency:
    - Tagging
    - Whitespace
    - Naming of Tasks, Plays, Variables, and Roles
    - Directory Layouts
    - Enforce the style  - example  [https://goo.gl/JfWBcW]

    ![alt text](/refimages/image.png)


Start with one Git repository - but when it grows,
use multiple!


#### Start with one Git repository - but when it grows, use multiple!

At the beginning: put everything in one Git repository

In the long term:

- One Git repository per role
- Dedicated repositories for completely separated teams / tasks

#### Give inventory nodes human-meaningful names rather than IPs or DNS hostnames.

![alt text](/refimages/names.png)


#### Group hosts for easier inventory selection and less conditional tasks -- the more the better.

![alt text](/refimages/groups.png)

#### Use dynamic sources where possible. Either as a single source of truth - or let Ansible unify multiple sources.

- Stay in sync automatically
- Reduce human error
- No lag when changes occur
- Let others manage the inventory

#### Proper variable names can make plays more readable and avoid variable name conflicts

![alt text](/refimages/variables.png)

#### Avoid collisions and confusion by adding the role name to a variable as a prefix.

apache_max_keepalive: 25
apache_port: 80
tomcat_port: 8080

##### Know where your variables are

- Find the appropriate place for your variables based on what, where and when they are set or modified
- Separate logic (tasks) from variables and reduce repetitive patterns
- Do not use every possibility to store variables - settle to a defined scheme and as few places as possible

#### Make your playbook readable

![alt text](/refimages/readableNo.png)

![alt text](/refimages/readableNo2.png)

![alt text](/refimages/readable.png)

#### Examples 

![alt text](/refimages/exhibitA.png)

![alt text](/refimages/exhibitB.png)

#### Blocks can help in organizing code, but also enable rollbacks or output data for critical changes.

![alt text](/refimages/blocks.png)

#### Ansible provides multiple switches for command line interaction and troubleshooting.

-vvvv
--step
--check
--diff
--start-at-task

#### Ansible has switches to show you what will be done

Use the power of included options:
--list-tasks
--list-tags
--list-hosts
--syntax-check

#### If there is a need to launch something without an inventory
- just do it!

- For single tasks - note the comma:
  ansible all -i neon.qxyz.de, -m service -a
  "name=redhat state=present"
- For playbooks - again, note the comma:
ansible-playbook -i neon.qxyz.de, site.yml


#### Don’t just start services -- use smoke tests

![alt text](/refimages/smoketests.png)