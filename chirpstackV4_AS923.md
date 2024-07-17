2B7E151628AED2A6ABF7158809CF4F3E

-------------------------------------------

sudo apt-get update

sudo apt install \
    mosquitto \
    mosquitto-clients \
    redis-server \
    redis-tools \
    postgresql

sudo -u postgres psql

-- create role for authentication
create role chirpstack with login password 'chirpstack';

-- create database
create database chirpstack with owner chirpstack;

-- change to chirpstack database
\c chirpstack

-- create pg_trgm extension
create extension pg_trgm;

-- exit psql
\q

sudo apt install apt-transport-https dirmngr

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1CE2AFD36DBCCA00

sudo echo "deb https://artifacts.chirpstack.io/packages/4.x/deb stable main" | sudo tee /etc/apt/sources.list.d/chirpstack.list

sudo apt update

sudo apt install chirpstack-gateway-bridge

The configuration file is located at /etc/chirpstack-gateway-bridge/chirpstack-gateway-bridge.toml.
 Please update the [integration.mqtt] section to match the region prefix for the region that applies 
to this ChirpStack Gateway Bridge instance.
Example for EU868:

[integration.mqtt]
event_topic_template="eu868/gateway/{{ .GatewayID }}/event/{{ .EventType }}"
command_topic_template="eu868/gateway/{{ .GatewayID }}/command/#"


----------------------------
# start chirpstack-gateway-bridge
sudo systemctl start chirpstack-gateway-bridge

# start chirpstack-gateway-bridge on boot

sudo systemctl enable chirpstack-gateway-bridge

----------------------------------------------------------------
Install ChirpStack

sudo apt install chirpstack

The configuration files are located at /etc/chirpstack/chirpstack.toml
You will find one global configuration file called chirpstack.toml 
and various region configuration files.

# start chirpstack
sudo systemctl start chirpstack

# start chirpstack on boot
sudo systemctl enable chirpstack

sudo journalctl -f -n 100 -u chirpstack

-----------------------------------------------------
tra ip 
ip address
port
netstat -tuln
