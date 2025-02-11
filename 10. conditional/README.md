```


    __   ___   ____   ___    ____  ______  ____  ___   ____    ____  _          ______   ____  _____ __  _  _____    
   /  ] /   \ |    \ |   \  |    ||      ||    |/   \ |    \  /    || |        |      | /    |/ ___/|  |/ ]/ ___/    
  /  / |     ||  _  ||    \  |  | |      | |  ||     ||  _  ||  o  || |        |      ||  o  (   \_ |  ' /(   \_     
 /  /  |  O  ||  |  ||  D  | |  | |_|  |_| |  ||  O  ||  |  ||     || |___     |_|  |_||     |\__  ||    \ \__  |    
/   \_ |     ||  |  ||     | |  |   |  |   |  ||     ||  |  ||  _  ||     |      |  |  |  _  |/  \ ||     \/  \ |    
\     ||     ||  |  ||     | |  |   |  |   |  ||     ||  |  ||  |  ||     |      |  |  |  |  |\    ||  .  |\    |    
 \____| \___/ |__|__||_____||____|  |__|  |____|\___/ |__|__||__|__||_____|      |__|  |__|__| \___||__|\_| \___|    
                                                                                                                     


```

the when statement is used to run a tasks conditionally. it takes as a value the condition to test. if the condition is met, the task runs. if the condition is not met, the task is skipped.

One of the simples condition that can be tested is wether a boolean is true or false. The when statement in the follwoing example causes the task run only if run_my_task is true.

```YAML
---
- name: Simple Boolean Task demo
  hosts: all
  vars:
    run_my_task: true

  tasks:
    - name: httpd package is installed
      ansible.builtin.dnf:
        name: httpd
    when: run_my_task
```

Below you can find some Example Conditionals

Operation  | Example
------------- | -------------
Equal (value is a string) | ansible_facts['machine'] == "x86_64"
Equal (value is nummeric)  | max_memory == 512
Less then | min_memory < 128
Greater then | min_memory > 256
Less then or equal to | min_memory <= 256
Greater then or equal to | min_memory >= 512
Not equal to | min_memory != 512
Variable exists | min_memory is defined
Variable does not exists | min_memory is not defined
Boolean variable is true | memory_available
Boolean variable is false | not memory_available
First variables value is present as a value in second variable list | ansible_facts['distribution'] insupported_distros
String contains | "''RedHat' in ansible_facts['distribution']"
String not contains | "''RedHat' not in ansible_facts['distribution']"
# Testing Multiple Conditions
One when statement can be used to evaluate multiple conditionals.To do so, conditinals can be combined with either the **and** or **or** keywords, and grouped with parantheses.

The following snippets show some more examples of how to express multiple conditions

If a conditional atatement should be met when either condition is ture, then use the or statement. For example the follwinf condition is met if the machine is running either Red Hat Enterprise Linux or Fedore
```YAML
when: ansible_facts['distribution'] == "RedHat" or ansible_facts['distribution'] == "Fedora" 
```

With the and operation, both conditions have to be true for the entire conditional statement to be meet. For example, the follwoing condition is met if the remote host is a Red Hat Enterprise Linux 9.0 host and the kernel installed is the specified version.\
```YAML
when: ansible_facts['distribution_version'] == "9.0" and ansible_facts['kernel'] == "5.14.0-70.13.1.el9_0.x86_64"
```

The when keyword also supports using a list to describe a list of conditions. When a list is provided to the when keyword, all the conditionals are combined using the and operation. the example below demostrate another way to combine multiple conditional statements using the and operator.
```YAML
when:
  - ansible_facts['distribution_version'] == "9.0"
  - ansible_facts['kernel'] == "5.14.0-70.13.1.el9_0.x86_64"
```

You can express more complex conditional statements by grouping conditions with parentheses.This ensures that they are correctly interpreted.

For example, the following conditional statement is met if the machine is running either Red Hat Enterprise Linux 9 or Fedora 34. This example uses the greater-than character (>) so that the long conditional can be split over multiple lines in the playbook, to make it easier to read.
```YAML
when: >
  ( ansible_facts['distribution'] == "RedHat" and
  ansible_facts['distribution_major_version'] == "9" )
  or
  ( ansible_facts['distribution'] == "Fedora" and
  ansible_facts['distribution_major_version'] == "34" )

```