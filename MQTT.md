sudo mosquitto_sub -t "+/gateway/#" -v
nano /etc/mosquitto/mosquitto.conf 
nano /etc/mosquitto/conf.d/listeners.conf
  per_listener_settings true


 listener 1883 0.0.0.0
 allow_anonymous true

systemctl restart mosquitto

-check mqtt explore application

sudo apt-get install golang-cfssl

nano ca-csr.json
{
  "CN": "ChirpStack CA",
  "key": {
    "algo": "rsa",
    "size": 4096
  }
}

sudo ca-config.json
{
  "signing": {
    "default": {
      "expiry": "8760h"
    },
    "profiles": {
      "server": {
        "expiry": "8760h",
        "usages": [
          "signing",
          "key encipherment",
          "server auth"
        ]
      }
    }
  }
}

cfssl gencert -initca ca-csr.json | cfssljson -bare ca

wait..

nano mqtt-server.json
{
    "CN": "example.com",
    "hosts": [
        "example.com"
    ],
    "key": {
        "algo": "rsa",
        "size": 4096
    }
}


cfssl gencert -ca ca.pem -ca-key ca-key.pem -config ca-config.json -profile server mqtt-server.json | cfssljson -bare mqtt-server

mkdir -p /etc/chirpstack/certs
cp ca.pem /etc/chirpstack/certs
cp ca-key.pem /etc/chirpstack/certs

chmod 655 /etc/chirpstack/certs
chmod 655 /etc/chirpstack/certs/ca.pem
chmod 655 /etc/chirpstack/certs/ca-key.pem
chmod 755 /etc/chirpstack/certs
nano /etc/chirpstack/chirpstack.toml
mkdir -p /etc/mosquitto/certs
ca.pem /etc/mosquitto/certs
cp mqtt-server.pem /etc/mosquitto/certs
cp mqtt-server-key.pem /etc/mosquitto/certs
cp ca.pem /etc/mosquitto/certs
cp mqtt-server.pem /etc/mosquitto/certs
cp mqtt-server-key.pem /etc/mosquitto/certs
chmod 655 /etc/mosquitto/certs
chmod 655 /etc/mosquitto/certs/ca.pem
chmod 655 /etc/mosquitto/certs/mqtt-server.pem
chmod 655 /etc/mosquitto/certs/mqtt-server-key.pem
chmod 755 /etc/mosquitto/certs

nano /etc/mosquitto/acl
pattern readwrite +/gateway/%u/#
pattern readwrite application/%u/#

nano /etc/mosquitto/conf.d/listeners.conf
per_listener_settings true

listener 1883 localhost
allow_anonymous true

listener 8883 0.0.0.0
cafile /etc/mosquitto/certs/ca.pem
certfile /etc/mosquitto/certs/mqtt-server.pem
keyfile /etc/mosquitto/certs/mqtt-server-key.pem
allow_anonymous false
require_certificate true
use_identity_as_username true


 systemctl restart mosquitto
systemctl restart chirpstack
systemctl restart chirpstack-gateway-bridge
