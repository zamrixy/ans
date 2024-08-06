# ans

Беседа № 8: 

Вот пример Ansible Playbook, который подключается к хостам, получает имя устройства и IP-адрес, а затем записывает результат в единый файл на управляющем хосте (Ansible-хосте):

YAML

- hosts: all
  tasks:
    - name: Gather facts
      ansible.builtin.setup:

    - name: Write device info to file
      ansible.builtin.lineinfile:
        path: /path/to/device_info.txt
        line: "Hostname: {{ ansible_hostname }}, IP Address: {{ ansible_host }}"
        create: yes
        state: present
      delegate_to: localhost
      run_once: true
Давайте разберем этот Playbook по шагам:

1. hosts: all - указывает, что данный Playbook будет выполнен на всех хостах, определенных в инвентаре Ansible.

2. tasks: - начало секции задач.

3. - name: Gather facts - задача, которая собирает факты об управляемых хостах, включая имя устройства и IP-адрес.

4. ansible.builtin.setup: - модуль, который используется для сбора фактов об управляемых хостах.

5. - name: Write device info to file - задача, которая записывает информацию об устройстве в файл.

6. ansible.builtin.lineinfile: - модуль, который используется для добавления или изменения строк в файле.

7. path: /path/to/device_info.txt - путь к файлу на управляющем хосте (Ansible-хосте), в который будет записана информация об устройствах.

8. line: "Hostname: {{ ansible_hostname }}, IP Address: {{ ansible_host }}" - строка, которая будет записана в файл. Здесь используются факты ansible_hostname и ansible_host, которые были получены на предыдущем шаге.

9. create: yes - создает файл, если он не существует.
10. state: present - гарантирует, что строка будет добавлена в файл.

11. delegate_to: localhost - указывает, что эта задача должна быть выполнена на управляющем хосте (Ansible-хосте), а не на управляемых хостах.

12. run_once: true - гарантирует, что эта задача будет выполнена только один раз, независимо от количества управляемых хостов.

Для запуска этого Playbook выполните следующую команду:

shell

ansible-playbook -i inventory.yml playbook.yml
Где:
- inventory.yml - файл с инвентарем хостов, к которым нужно подключиться.
- playbook.yml - файл с описанным выше Playbook.

После выполнения Playbook, в файле /path/to/device_info.txt на управляющем хосте (Ansible-хосте) будет записана информация обо всех управляемых хостах в формате "Hostname: <hostname>, IP Address: <ip_address>".
