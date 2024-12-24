# Ansible Role: Nginx Certbot

This Ansible role automates the setup of Nginx web servers using Certbot to deploy HTTPS encryption using Let's Encrypt certificates.

## Requirements

- Ansible installed on the control node.
- Domain name(s) pointed to the server(s) where Nginx is running.

## Role Variables

Below are the variables used in this role, along with their descriptions:

- `nginx_certbot_domain_name`: The domain name for which the SSL certificate will be obtained.
- `nginx_certbot_certbot_email`: Email address to use for Let's Encrypt certificate registration.
- `nginx_certbot_deny_http`: Whether to deny HTTP access after enabling HTTPS. Default is `false`.
- `nginx_certbot_proxy_protocol`: Whether to enable support for the nginx PROXY protocol. Default is `false`.
- `nginx_certbot_ha`: If defined configure a load balancing for between 2 peers in `lb_peers` list:
  - `lb_peers`: List of ipv4 for load balancers
- `nginx_certbot_app`: Configuration for the application served by Nginx:
  - `name`: Name of the application.
  - `url`: URL where the application is hosted.
  - `http_template_name`: Name of the Nginx HTTP template. Some examples are available in templates dir
  - `upstream_template_name`: Name of the Nginx upstream template. Some examples are available in templates dir
- `nginx_certbot_custom_http_block`: If `http_template_name` is set to `custom`, it will use the content of this variable as template for http block.
- `nginx_certbot_custom_upstream_block`: If `upstream_template_name` is set to `custom_upstream`, it will use the content of this variable as template for upstream block.


These variables can be customized in your playbook to match your specific environment and configuration requirements.

## Dependencies

None.

## Example Playbook

```yaml
- hosts: web_servers
  roles:
    - role: chadek.nginx_certbot
      vars:
        nginx_certbot_domain_name: app.example.com
        nginx_certbot_certbot_email: some.mail@example.com
        nginx_certbot_deny_http: false
        nginx_certbot_proxy_protocol: false
        nginx_certbot_ha: {
          lb_peers: [
            some.ip.v.4,
            some.ip.v.4
          ]
        }        
        nginx_certbot_app:
          name: app
          url: "http://localhost:2000"
          http_template_name: custom
          upstream_template_name: custom_upstream
        nginx_certbot_custom_upstream_block: |
          upstream custom-default {
            zone custom-default 64k;
            server 127.0.0.1:8080;
            keepalive 2;
          }          
        nginx_certbot_custom_http_block: |
          root /var/www/someapp;
          index index.php index.html index.htm;

          client_max_body_size 128M;
          location / {
                try_files $uri /index.php$is_args$args;
          }
          location ~ \.php$ {
              include snippets/fastcgi-php.conf;
              fastcgi_pass       unix:/var/run/php/php8.2-fpm.sock;
              fastcgi_param      SCRIPT_FILENAME $document_root$fastcgi_script_name;
          }          
```

## License

This role is licensed under the MIT License.

## Author Information

This role was created by Chadek.

## Feedback and Contributions

Please feel free to open an issue or submit a pull request on [GitHub](https://github.com/chadek/ansible-role-nginx-certbot) if you have any feedback or would like to contribute.

