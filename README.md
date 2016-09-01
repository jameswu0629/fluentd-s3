# fluentd-s3

### Install Fluentd (Amazon Linux)
```
$ curl -L https://toolbelt.treasuredata.com/sh/install-redhat-td-agent2.sh | sh
```

### Install s3 plug-in
```
$ /opt/td-agent/embedded/bin/fluent-gem install fluent-plugin-s3
```

### Customize your configuration
Ref: https://github.com/fluent/fluent-plugin-s3 or your can use the example as following:
* **format**: supported formats are "apache_error", "apache2", "syslog", "json", "tsv", "ltsv", "csv", "nginx" and "none".
* **store_as**: you can choose save file on S3 as gzip/text/json
```
<source>
  @type tail
  format apache2
  path /var/log/httpd/access_log
  pos_file /var/log/td-agent/httpd.access_log.pos
  tag s3.apache.access
</source>

<match s3.*.*>
  @type s3
  aws_key_id ABCD
  aws_sec_key ABCD
  s3_region cn-north-1
  s3_bucket wuzc-uuzu
  s3_object_key_format %{path}%{time_slice}_%{index}.%{file_extension}
  path logs/
  buffer_path /var/log/td-agent/s3

  time_slice_format %Y%m%d-%H
  time_slice_wait 1m
  utc
  format json
  store_as json
</match>
```
