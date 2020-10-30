# Docker-compose

## Volumes

### What is it?

Volumes are the preferred mechanism for persisting data generated by and used by Docker containers. While [bind mounts](https://docs.docker.com/storage/bind-mounts/) are dependent on the directory structure and OS of the host machine, volumes are completely managed by Docker. 

Volumes are often a better choice than persisting data in a container’s writable layer, because a volume does not increase the size of the containers using it, and the volume’s contents exist outside the lifecycle of a given container.

### What problems does it solve?

* Data loss in case of container restart
* Storing \(non\)persistent application state

### How to mount a volume?

Depends on what you need, you can choose between 3 ways of storing your data:

![](../../../../../.gitbook/assets/image%20%284%29.png)

1. Bind mounts from filesystem
2. Create a volume in the Docker area
3. Create tmpfs mount inside memory if your container generates non-persistent state data

{% hint style="warning" %}
The best way is to always use volumes in the Docker area, because:  
1. **Bound mounts** from filesystem are not dependent on Docker so you may face several issues if you'll have to scale or migrate your application   
2. Storing application state on **tmpfs mounts** may lead to instability in application behaviour 
{% endhint %}

TODO: 

🔯 Restore, backup volume  
✡️ Examples 




