# SSHing into Linux VM

In order to SSH into a Linux VM on Google Cloud, you can use the very convenient:

```
gcloud compute ssh --zone "asia-northeast1-b" "gvm-debian-x86"  --project "your-project-name"         
```

But if you want to use SSH keys like normal, you'll need to configure the VM configuration. Instructions are here:

https://cloud.google.com/compute/docs/connect/add-ssh-keys

```
gcloud compute os-login ssh-keys add --key-file=${HOME}/.ssh/google_compute_engine.pub

# Metadata way
echo -n "liquidx:" > keyfile
cat ${HOME}/.ssh/google_compute_engine.pub >> keyfile
gcloud compute project-info add-metadata --metadata-from-file=ssh-keys=./keyfile
```

I'm not sure if the VM actually uses OS Login or the metadata. I set both up and it seemed to work.

Get the External IP address from the console page and do : `ssh liquidx@1.2.3.4`

Bonus is that if you want to make sure it uses the right key for sign in, change the `~/.ssh/config` with:

```
Host 1.2.3.4
  Identity ~/.ssh/google_compute_engine
  User liquidx
```
