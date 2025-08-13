1. Создайте в старой версии playbook файл requirements.yml и заполните его содержимым
2. При помощи ansible-galaxy скачайте себе эту роль
```
ansible-galaxy install -r requirements.yml
```
```
Starting galaxy role install process
- extracting clickhouse to /home/vv/.ansible/roles/clickhouse
- clickhouse (1.13) was installed successfully
```
8. Добавьте roles в requirements.yml в playbook
```yaml
- src: git@github.com:IooEWREYooI/vector-role.git
  scm: git
  version: "0.0.2"
  name: vector

- src: git@github.com:IooEWREYooI/lighthouse-role.git
  scm: git
  version: "0.0.1"
  name: lighthouse

```
```
Starting galaxy role install process
- extracting vector to /home/vv/.ansible/roles/vector
- vector (0.0.1) was installed successfully
- extracting lighthouse to /home/vv/.ansible/roles/lighthouse
- lighthouse (0.0.2) was installed successfully

```
9.  Переработайте playbook на использование roles
```yaml
---
- name: Install Clickhouse
  become: true
  hosts: clickhouse
  roles:
    - clickhouse

- name: Install others
  hosts: clickhouse
  become: true
  roles:
    - vector
    - lighthouse
```
11. В ответе дайте ссылки на оба репозитория с roles  
https://github.com/IooEWREYooI/vector-role  
https://github.com/IooEWREYooI/lighthouse-role 

Запуск плейбука:
```bash
ansible-playbook  main.yml 
```
Результат:  
![pic](/assets/img.png)
