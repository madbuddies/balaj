Go to Searchbar search Windows feature on or off 
Click on windows subsystem for Linux 
Click on ok 
Click on Restart now 

>wsl --list --online
>wsl --install --distribution Ubuntu-24.04
>apt update && apt install ansible -y
>mkdir ~/ansible-localhost && cd ~/ansible-localhost
>nano hosts.ini

[local]
localhost ansible_connection=local

>ansible -i hosts.ini all -m ping
>nano ansible.yml

- name: Install and run nginx on localhost
  hosts: local
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start nginx
      service:
        name: nginx
        state: started
        enabled: yes

>ansible-playbook -i hosts.ini ansible.yml --ask-become-pass
Check browser:
	http://localhost
>nano index.html

<!DOCTYPE html>
<html>
<head>
    <title>My Custom Nginx Page</title>
</head>
<body>
    <h1>🚀 Hello from Ansible!</h1>
    <p>This is a custom web page deployed using Ansible!</p>
</body>
</html>

>nano ansible.yml
(remove the existing code)
- name: Install and run nginx on localhost
  hosts: local
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Copy custom index.html
      copy:
        src: index.html
        dest: /var/www/html/index.html
        owner: www-data
        group: www-data
        mode: '0644'

    - name: Start nginx service
      service:
        name: nginx
        state: started
        enabled: yes

>ansible-playbook -i hosts.ini ansible.yml --ask-become-pass
Open Your Browser:  http://localhost
