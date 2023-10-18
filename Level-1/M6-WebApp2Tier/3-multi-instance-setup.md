# WordPress application will achieve high availability by developing a multi-instance infrastructure.

### Creating a multi-instance architecture in AWS for Wordpress application with
    - High availability
    - Security
    - Monitoring

### For installation of WordPress, PHP, and MySQL replication clusters, refer to my repository.

### Now, I am going to create a Instance Group for the load balancer. It is used to redirect requests to one or more backend registered targets.


![image](https://github.com/rutwiksquareops/RTD/assets/146915430/54d92ea7-9ae2-4175-a4aa-5203326f6838)



### Now, go to the load balancer and create an ALB for the WordPress application.

![image](https://github.com/rutwiksquareops/RTD/assets/146915430/ac612eba-6802-4d83-ba9b-a328dc570bc4)


#### Target group health status should be Success
#### Open the DNS link provided by ALB, upload your image, and check that your image is visible on the browser after consecutive refreshes.

Your image is stored in local WordPress storage; now we have to attach EFS to all WordPress applications.

#### Create an FileStore and select the same VPC where your instance is running.

![image](https://github.com/rutwiksquareops/RTD/assets/146915430/e5ac26df-7d9e-476a-ac56-1b4be46f9cf5)


### SSH to your vm and run following command:
    Install NFS:
    #sudo apt-get -y update &&
    #sudo apt-get install nfs-common
    
    Make a local directory to map to the Filestore file share:
    #sudo mkdir -p mount-point-directory
    where mount-point-directory is the directory to create, for example /mnt/filedir.

    #sudo vim /etc/fstab

    10.0.0.2:/vol1 /mnt nfs defaults,_netdev 0 0

    #sudo mount -a

    ip-address:/file-share mount-point-directory nfs options,_netdev 0 0
    
### Check efs is mount successfully or not.

    # df -h
