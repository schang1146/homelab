# Create Domain For App

Create DNS Records

| Name                   | Type | Value                     | TTL |
| ---------------------- | ---- | ------------------------- | --- |
| {{ application_name }} | AAAA | AAAA:BBBB:CCCC::DDDD:EEEE | 60  |
| {{ application_name }} | A    | 123.456.78.901            | 60  |

Create Traefik labels in `docker-compose.yml` file

```yml
services:
  "{{ application_name }}":
    ...
    labels:
      traefik.enable: true
      traefik.http.routers.{{ application_name }}.tls: true
      traefik.http.routers.{{ application_name }}.tls.certresolver: letsencrypt_resolver
      traefik.http.routers.{{ application_name }}.tls.domains: {{ application_domain_name }}
      traefik.http.routers.{{ application_name }}.entrypoints: websecure
      traefik.http.routers.{{ application_name }}.rule: Host(`{{ application_domain_name }}`)
      traefik.http.routers.{{ application_name }}.service: {{ application_name }}-svc
      traefik.http.services.{{ application_name }}-svc.loadbalancer.server.url: http://{{ server_ip }}:{{ application_port }}
```

Add entry in `dynamic_conf.yml` (`dynamic_conf_template.j2`) for Traefik

```yml
http:
  services:
    ...
    {{ application_name }}-svc:
      loadBalancer:
        servers:
        - url: "http://{{ tailscale_ipv4 }}:{{ application_port }}"
```
