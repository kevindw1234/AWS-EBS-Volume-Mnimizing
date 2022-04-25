# AWS-EBS-Volume-Mnimizing
This repo is used to decrease or minimize the AWS EBS Volume . Using these steps we can reduce the size of EBS Volume. 
Amazon EBS Volume size decreasing.
*Amazon EBS volume cannot be reduced directly from the Amazon console .So we need to follow some steps to reduce the Amazon EBS voloume.
*Assuming we want to reduce this to 8GB, the first thing we will need to do is to make a note of the root volume’s block device name and our instance’s availability zone.

 1)Shut down the instance to prevent inconsistencies.

 2)Create a new smaller EBS Volume.
 
    a)Make sure the instance is shutdown.
    
    b)Click Create Volume
    
    c)Choose the same volume type same with your old volume.
    
    d)Enter your desired size; In our case 30.
    
    e)Choose the availability zone; The volume will be available to the instance in the same                                                                             availability zone, which is ap-southeast-1a .
    
    f)Add tag with Key : Name and value : new-volume

3)Attach the new volume.

     a)Right click on the new volume.
   
     b)Click Attach Volume.
   
     c)Choose instance name my-instance .
   
     d)Click Attach.
   
4)We could start the instance and login to SSH. List all available volumes with lsblk. 

     The new volume is in /dev/xvdf.

5)Format the new volume

     a)Check whether the volume has any data or not using command sudo file -s /dev/xvdf .
   
      b)If it is displaying /dev/xvdf: data, it means the volume is empty. We could format the volume.
   
      c)If it is displaying other than the above output, it means the volume has data. DO NOT format the volume if you saw this output.
   
      d)Format the volume using command sudo mkfs -t ext4 /dev/xvdf 
    

6)Mount the new volume

    a)Create a directory to mount using the command sudo mkdir /mnt/new-volume .
  
    b)Mount the new volume into the directory using command sudo mount /dev/xvdf /mnt/new-volume .
  
    c)Check volume with command df -h; The new volume should be mounted now.



7)Copy data from old volume to the new 

    a)Use rsync to copy from old volume to the new volume sudo rsync -axv / /mnt/new-volume/.
   
    b)Relax and wait until it’s finished. 

8)Prepare new volume

    a)Install grub on new-volume using the command sudo grub-install --root-directory=/mnt/new-volume/ --force /dev/xvdf .
   
    b)Unmount new-volume sudo umount /mnt/new-volume .
   
    c)Check UUID using command blkid .
   
    d)Copy the UUID from /dev/xvda1 (paste anywhere to backup this UUID); This is your old UUID.
    
    e)Use tune2fs to replace UUID sudo tune2fs -U COPIED_UUID /dev/xvdf; COPIED_UUID is the string value from point 4.
     
    f)Check the system label from old-volume using the command sudo e2label /dev/xvda1 ; It will display a string like cloudimg-rootfs .
     
    g)Replace new-volume label with old-volume label using command sudo e2label /dev/xvdf cloudimg-rootfs .
     
9)We can logout SSH now.

10)Stop instance my-instance

11)Detach old-

12)Detach new-volume

13)Attach new-volume to /dev/

14)Start instance my-instance

 
## Connect with me:

<p align="left">

<a href = "https://www.linkedin.com/in/kevindw98/"><img src="https://img.icons8.com/fluent/48/000000/linkedin.png"/></a>
<a href = "https://www.instagram.com/kevin_danile_wilson/"><img src="https://img.icons8.com/fluent/48/000000/instagram-new.png"/></a>

</p>
