## tl;dr
This simple repo helps you join multiple worker nodes to your master node.

### Prerequisites

#### Worker

1. All worker nodes have been added to sudo list.
2. Worker Nodes must be ubuntu machine, although we have not tested with other OSes.
3. Python installed.


#### Master

1. Kubernetes cluster already initalized using kubeadm and healthy.
2. kubeconfig file is present with current users context.
3. All machines are in same network.
4. Master can ssh into each machine with/without password.

### Steps -- On master node
1. git clone <repo>/join-worker
2. cd repo
3. uv sync
4. source venv/bin/activate
5. Open ```hosts.ini``` file. and replce master and worker nodes with correct IPs, host names, and passwords.
6. Run the following command to initilize the playbook

```
cd ./ansible
ansible-playbook -i hosts.ini join-worker.yml -K  -v
```

This will prompt for password of master node machine.