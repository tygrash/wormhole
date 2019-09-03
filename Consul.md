## Setup Consul as dynamic service discovery

#### Install consul on a VM
* `sudo apt-get install unzip`
* `cd /usr/local/bin`
* `sudo wget https://releases.hashicorp.com/consul/0.8.0/consul_0.8.0_linux_amd64.zip`
* `sudo unzip consul_0.8.0_linux_amd64.zip`
* `sudo rm -rf consul_0.8.0_linux_amd64.zip`
* `mkdir consul-ui`
* `cd consul-ui`
* `wget https://releases.hashicorp.com/consul/0.8.0/consul_0.8.0_web_ui.zip`
* `unzip consul_0.8.0_web_ui.zip`
* `rm -rf consul_0.8.0_web_ui.zip`
* `cd ~`
* `mkdir -p consul-config/server`
* `vim consul-config/server/config.json` and add following content in it:
```
	{
	    "bootstrap": true,
	    "server": true,
	    "log_level": "DEBUG",
	    "enable_syslog": true,
	    "datacenter": "server1",
	    "addresses" : {
	      "http": "0.0.0.0"
	    },
	    "bind_addr": "{machine's_IP}",
	    "node_name": "{machine's_IP}",
	    "data_dir": "/home/k/consuldata",
	    "ui_dir": "/home/k/consul-ui",
	    "acl_datacenter": "server1",
	    "acl_default_policy": "allow",
	    "encrypt": "5KKufILrf186BGlilFDNig=="  *********(get this by `consul keygen` command)*********
	}
```
* Start consul service in background by `consul agent -config-dir ~/consul-config/server -ui-dir /usr/local/bin/consul-ui/ -bootstrap true -client=0.0.0.0 > /dev/null 2>&1 &`
