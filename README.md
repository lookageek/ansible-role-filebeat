# Ansible Role: Filebeat

An Ansible Role that installs Filebeat on Debian/Ubuntu with systemd.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    elasticstack_deb_repo: "5.x"

The elasticstack debian repo version

    filebeat_version: "5.3.2"

Specific filebeat version to install

    filebeat_enabled_on_boot: yes

Set this to `no` if you don't want filebeat to run on system startup.

    filebeat_create_config: true

Whether to create the Filebeat configuration file and handle the copying of SSL key and cert for filebeat. If you prefer to create a configuration file yourself you can set this to `false`.

    filebeat_prospectors:
      - input_type: log
        paths:
          - "/var/log/*.log"
        json.message_key: event
        json.keys_under_root: "true"

Prospectors that will be listed in the `prospectors` section of the Filebeat configuration. Read through the [Filebeat Prospectors configuration guide](https://www.elastic.co/guide/en/beats/filebeat/current/configuration-filebeat-options.html) for more options.

    filebeat_output_elasticsearch_enabled: false
    filebeat_output_elasticsearch_hosts:
      - "localhost:9200"

Whether to enable Elasticsearch output, and which hosts to send output to.

    filebeat_output_logstash_enabled: true
    filebeat_output_logstash_hosts:
      - "localhost:5044"

Whether to enable Logstash output, and which hosts to send output to.

    filebeat_enable_logging: false
    filebeat_log_level: warning
    filebeat_log_dir: /var/log/filebeat
    filebeat_log_filename: filebeat.log
    filebeat_log_to_file: "true"
    filebeat_log_to_syslog: "false"

Filebeat logging.

    filebeat_ssl_dir: /etc/pki/logstash

The path where certificates and keyfiles will be stored.

    filebeat_ssl_certificate_file: ""
    filebeat_ssl_key_file: ""

Local paths to the SSL certificate and key files, which will be copied into the `filebeat_ssl_dir`.

For utmost security, you should use your own valid certificate and keyfile, and update the `filebeat_ssl_*` variables in your playbook to use your certificate.

To generate a self-signed certificate/key pair, you can use use the command:

    $ sudo openssl req -x509 -batch -nodes -days 3650 -newkey rsa:2048 -keyout filebeat.key -out filebeat.crt

Note that filebeat and logstash may not work correctly with self-signed certificates unless you also have the full chain of trust (including the Certificate Authority for your self-signed cert) added on your server. See: https://github.com/elastic/logstash/issues/4926#issuecomment-203936891

    filebeat_ssl_insecure: "false"

Set this to `"true"` to allow the use of self-signed certificates (when a CA isn't available).

## Dependencies

None.

## License

MIT / BSD

## Author Information

This role was created in 2017 by Jayanth Manklu cloning from [geerlingguy/ansible-role-filebeat](https://github.com/geerlingguy/ansible-role-filebeat).
