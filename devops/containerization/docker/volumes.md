# Volumes

Data volumes are used to persist data that outlives the container.

* A data volume is a directory or file in the docker host filesystem that is mounted directly to a container
  * Therefore: Operates on host speed

<div align="left">

<figure><img src="../../../.gitbook/assets/volume_create.png" alt="" width="358"><figcaption></figcaption></figure>

</div>

* Volumes are mounted when a container is created
  * If the containers image contains data at the specified mount point, that existing data is copied into the new volume upon volume initialization

<div align="left">

<figure><img src="../../../.gitbook/assets/Screenshot 2023-05-25 at 21.04.46.png" alt="" width="563"><figcaption></figcaption></figure>

</div>

### Mounting

There are 2 types of mounting:

* Volume mounting
  * Mounts a volume from the directory `/var/lib/docker/volumes`
    * If no volume is created while mounting, docker will automatically create one
* Bind mounting
  * Mounts any directory from the host e.g `/user`
  * This example mounts the folder
    * `/tmp/pan-parameterization into the container -> /usr/src/app/var`

<div align="center">

<figure><img src="../../../.gitbook/assets/volume_mounting.png" alt=""><figcaption></figcaption></figure>

</div>
