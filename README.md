Helpers for accessing [OpenStack](http://api.openstack.org/)
with [cURL](http://curl.haxx.se/)

License: MIT.
Author: [Martin Vidner](https://github.com/mvidner/) @ [SUSE](http://suse.com).

Make a file defining the endpoint and credentials.
I suggest the naming scheme openrc-_$CLOUD_-_$USER_

    mkdir -p ~/.openstack
    F=~/.openstack/openrc-mycloud-demo
    : > $F
    chmod 600 $F
    cat >> $F <<'EOF'
      SERVICE_HOST=mycloud.example.com
      export OS_AUTH_URL="https://${SERVICE_HOST}:5000/v2.0" # or http:
      export OS_TENANT_NAME=demo
      export OS_USERNAME=demo
      export OS_PASSWORD=openstack
    EOF
    .  $F

This will authenticate and set up `$OS_TOKEN` and `$OS_*_URL`:

    $ eval `./ks-auth | ./ks-parse`

See:

    $ curl -H "X-Auth-Token: $OS_TOKEN" $OS_OBJECT_STORE_URL
    MyContainer
    $ curl -H "X-Auth-Token: $OS_TOKEN" $OS_OBJECT_STORE_URL/MyContainer
    MyObject1
    MyObject2

    $ env | grep OS_.\*_URL | sort
    OS_AUTH_URL=http://mycloud.example.com:5000/v2.0
    OS_COMPUTE_URL=http://10.0.0.2:8774/v2/8ab8dbaed7be429e96a4b23ccc30064c
    OS_EC2_URL=http://10.0.0.2:8773/services/Cloud
    OS_IDENTITY_URL=http://10.0.0.2:5000/v2.0
    OS_IMAGE_URL=http://10.0.0.2:9292/v1
    OS_OBJECT_STORE_URL=http://10.0.0.2:8080/v1/AUTH_8ab8dbaed7be429e96a4b23ccc30064c
    OS_S3_URL=http://10.0.0.2:8080
    OS_VOLUME_URL=http://10.0.0.2:8776/v1/8ab8dbaed7be429e96a4b23ccc30064c

Requirements:

* cURL
* Python 2 or Python 3

FIXME ks-parse does not escape malicious input yet.
