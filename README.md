# Ansible Role: Nginx Certbot

This Ansible role automates the setup of HTTPS encryption for Nginx web servers using Let's Encrypt certificates via Certbot.

## Requirements

- Ansible installed on the control node.
- Nginx installed and configured on the target server(s).
- Domain name(s) pointed to the server(s) where Nginx is running.

## Role Variables

Below are the variables used in this role, along with their descriptions:

- `nginx_certbot_domain_name`: The domain name for which the SSL certificate will be obtained.
- `nginx_certbot_certbot_email`: Email address to use for Let's Encrypt certificate registration.
- `nginx_certbot_deny_http`: Whether to deny HTTP access after enabling HTTPS. Default is `false`.
- `nginx_certbot_proxy_protocol`: Whether to enable support for the PROXY protocol. Default is `false`.
- `nginx_certbot_app`: Configuration for the application served by Nginx:
  - `name`: Name of the application.
  - `url`: URL where the application is hosted.
  - `http_template_name`: Name of the Nginx HTTP template.
  - `upstream_template_name`: Name of the Nginx upstream template.

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
        nginx_certbot_app:
          name: app
          url: "http://localhost:2000"
          http_template_name: root
          upstream_template_name: root_upstream
```

## License

This role is licensed under the MIT License.

## Author Information

This role was created by Chadek.

## Feedback and Contributions

Please feel free to open an issue or submit a pull request on [GitHub](https://github.com/chadek/ansible-role-nginx-certbot) if you have any feedback or would like to contribute.

