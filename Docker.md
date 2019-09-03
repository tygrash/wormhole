## Install and Setup Docker on each Ubuntu VMs:
* `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`
* `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`
* `sudo apt-get update`
* `apt-cache policy docker-ce`
* `sudo apt-get install -y docker-ce`
* `sudo usermod -a -G docker $USER`
* [setup consul](Consul.md)
* `sudo vi /etc/default/docker` and add replace `DOCKER_OPTS` env variable by following line:
    ```
    DOCKER_OPTS="-H tcp://0.0.0.0:2376 -H unix:///var/run/docker.sock --cluster-advertise {machine's_IP}:2376 --cluster-store consul://{consul_VM_IP}:8500"
    ```
* `sudo vi /lib/systemd/system/docker.service` and add following content to load docker service with above changes:
    ```
        [Unit]
    	Description=Docker Application Container Engine
    	Documentation=https://docs.docker.com
    	BindsTo=containerd.service
    	After=network-online.target firewalld.service containerd.service
    	Wants=network-online.target
    	Requires=docker.socket
    
    	[Service]
    	Type=notify
    	# the default is not to use systemd for cgroups because the delegate issues still
    	# exists and systemd currently does not support the cgroup feature set required
    	# for containers run by docker
    	EnvironmentFile=/etc/default/docker
    	ExecStart=/usr/bin/dockerd $DOCKER_OPTS -H fd:// --containerd=/run/containerd/containerd.sock
    	ExecReload=/bin/kill -s HUP $MAINPID
    	TimeoutSec=0
    	RestartSec=2
    	Restart=always
    
    	# Note that StartLimit* options were moved from "Service" to "Unit" in systemd 229.
    	# Both the old, and new location are accepted by systemd 229 and up, so using the old location
    	# to make them work for either version of systemd.
    	StartLimitBurst=3
    
    	# Note that StartLimitInterval was renamed to StartLimitIntervalSec in systemd 230.
    	# Both the old, and new name are accepted by systemd 230 and up, so using the old name to make
    	# this option work for either version of systemd.
    
    	StartLimitInterval=60s
    
    	# Having non-zero Limit*s causes performance problems due to accounting overhead
    	# in the kernel. We recommend using cgroups to do container-local accounting.
    	LimitNOFILE=infinity
    	LimitNPROC=infinity
    	LimitCORE=infinity
    
    	# Comment TasksMax if your systemd version does not supports it.
    	# Only systemd 226 and above support this option.
    	TasksMax=infinity
    
    	# set delegate yes so that systemd does not reset the cgroups of docker containers
    	Delegate=yes
    
    	# kill only the docker process, not all processes in the cgroup
    	KillMode=process
    
    	[Install]
    	WantedBy=multi-user.target
    ```
    
* `sudo systemctl daemon-reload`
* `sudo service docker restart`

#### Create docker overlay network in one of the docker host
* `docker network create -d overlay alluxio_net`

This will connect all docker hosts via a private LAN kind of network and since every docker is connected to consul, one container can recognize another just by containerID

#### Setup alluxio-docker pre-requisites
* `docker volume create ufs`