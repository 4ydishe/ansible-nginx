# Ansible
# Папку с файлами Git положить в одно место
1. # Создаем ключ (оставляем значения по умолчанию)
ssh-keygen
2. # Копируем ключ на серверную машину
ssh-copy-id 192.168.11.150 (оставив значения по умолчанию)
3. # Создаем папку ~/staging и копируем в неё все файлы для работы с ansible (конфигурация, файл инвентаризации, nginx.conf.j2, playbook)
sudo mkdir -p ~/staging
sudo cp /vagrant/ansible.cfg /vagrant/inventory /vagrant/nginx.conf.j2 /vagrant/playbook-nginx.yml /vagrant/readme.md ~/staging
4. # проверяем доступность машины
ansible -i ~/staging/inventory web -m ping
5. # получаем ответ
nginx | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
6. # Запускаем playbook
ansible-playbook -i /vagrant/inventory ~/staging/playbook-nginx.yml
###
vagrant@nginx:~$ ansible-playbook -i /vagrant/inventory ~/staging/playbook-nginx.yml

PLAY [Playbook for installing NGINX and uploading a test page on port 8080] ********************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [nginx]

TASK [Ensure NGINX is installed] ***************************************************************************************
changed: [nginx]

TASK [Ensure NGINX is started and enabled] *****************************************************************************
ok: [nginx]

TASK [Deploy custom NGINX config from template] ************************************************************************
changed: [nginx]

TASK [Create index.html for test page] *********************************************************************************
changed: [nginx]

TASK [Ensure the firewall allows HTTP traffic on port 8080] ************************************************************
changed: [nginx]

TASK [Ensure the firewall allows SSH traffic on port 22] ***************************************************************
changed: [nginx]

TASK [Restart NGINX to apply changes] **********************************************************************************
changed: [nginx]

PLAY RECAP *************************************************************************************************************
nginx                      : ok=8    changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

7. # Проверим страницу
curl http://192.168.11.151:8080
<html>
<head>
  <title>Welcome to NGINX on port 8080!</title>
</head>
<body>
  <h1>Success! The NGINX server is working on port 8080!</h1>
</body>
</html># ansible-nginx
