# Ansible Роль: nginx

Эта роль Ansible устанавливает и настраивает Nginx, выдает сертификаты Let's Encrypt для указанных доменов, выдает самоподписанные сертификаты для других доменов, настраивает автоматическое обновление сертификатов Let's Encrypt, копирует пользовательские конфигурации Nginx и автоматически активирует виртуальные хосты Nginx для указанных доменов.

## Переменные роли

Следующие переменные могут быть установлены для настройки поведения роли:

### Сертификаты для доменов

- `nginx_letsencrypt_cert`: Список доменов, для которых должны быть выданы сертификаты Let's Encrypt.
  ```yaml
  nginx_letsencrypt_cert:
    - example.com
    - another-example.com
- `nginx_selfsigned_cert`: Список доменов, для которых должны быть выданы самоподписанные сертификаты.
  ```yaml
  nginx_selfsigned_cert:
    - selfsigned-example.com
    - another-selfsigned.com
- `nginx_letsencrypt_cert_renewal`: Переменная типа boolean для включения автоматического обновления сертификатов Let's Encrypt.
  ```yaml
  nginx_letsencrypt_cert_renewal: true
- `nginx_configs`: Путь к каталогу/каталогам, содержащему пользовательские конфигурации Nginx, которые будут скопированы.
  ```yaml
  nginx_configs: 
    - /path/to/nginx/configs
    - /another/path/to/nginx/configs
- `nginx_vhosts`: Список доменов, для которых должны быть активированы виртуальные хосты Nginx.
  ```yaml
  nginx_vhosts:
  - vhost1.com
  - vhost2.com

При активации виртуальных хостов из списка nginx_vhosts происходит проверка на наличиче конфигурации, если подходящая конфигурация не была найдена то она будет скопирована из шаблона в соответствии с сретификатом который был выдан домену.

### Пример плейбука
```yaml
- name: Настройка Nginx и управление SSL сертификатами
  hosts: all
  become: yes
  vars:
    nginx_letsencrypt_cert:
      - example.com
      - another-example.com
    nginx_selfsigned_cert:
      - selfsigned-example.com
      - another-selfsigned.com
    nginx_letsencrypt_cert_renewal: true
    nginx_configs: /path/to/nginx/configs
    nginx_vhosts:
      - vhost1.com
      - vhost2.com
  roles:
    - nginx
