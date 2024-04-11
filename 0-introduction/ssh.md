# SSH Secure Shell

SSH stands for secure shell; the server runs on port 22, available on most *nix environments and network devices.
OpenSSH is the defacto software 

* You need a username and password to log in to the remote host.
* Like https, SSH encrypts data on the wire + gives assurance you are talking to the correct server.
* Ssh configuration blocks login to root user. 
* ssh client-side configuration + ssh server-side configuration
* Generate public/private keys and copy public keys to the remote hosts to log in without a password.
* Protecting private keys and not entering a password is outside of the discussion.
* Openssh try to login to same user if you don't provide user along with IP.* 
* On the client side /home/username/.ssh/config is client-side configuration /etc/ssh/sshd_config is server-side configuration.

```
ssh remoteuser@remoteip
scp file remoteuser@remoteip:
```
This is test.
