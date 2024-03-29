# Ansible modules 

There are lot of builtin modules available for ansible. Let us look into some of them.

```
ansible mygroup -m ping

```
You expect this work as expected.
```
1.1.1.23 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
1.1.1.24 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
1.1.1.25 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

You will be spending most of the time setting up inventory file right. Let us take the inventory file
```
[mygroup]
172.29.202.192 ansible_ssh_user=userA
172.29.202.229 ansible_ssh_user=userA
172.29.202.230 ansible_ssh_user=userA

```
You get output like
```
ansible mygroup -m command -a "hostname"
172.29.202.229 | CHANGED | rc=0 >>
kilar-centos04
172.29.202.230 | CHANGED | rc=0 >>
kilar-centos03
172.29.202.192 | CHANGED | rc=0 >>
kilar-centos01
```
It is not ideal to repeat code. Let us simplify the inventory file a bit.

```
[mygroup:vars]
ansible_connection=ssh
ansible_user=userA
[mygroup]
172.29.202.192 
172.29.202.229 
172.29.202.230 
```
We improved the inventory file.

Notice that we mentioned ssh user is userA repeated How do we improve inventory file? We don't need to repeat ssh_user.

```
[cloudbolt:vars]
ansible_connection=ssh
ansible_user=userA

[cloudbolt]
162.22.202.192
162.22.202.229
162.22.202.230
162.22.202.235
162.22.202.236
162.21.202.248
162.21.202.254
162.21.203.1
162.21.203.2
162.21.203.12
```
We improved inventory file, no repetation of data in the new format.
