<h1>AWS EBS (Elastic Block Store)</h1>


<h2>TLDR Description</h2>
Creating an EBS volume, attaching and mounting it to an EC2 instance, and restoring it from a snapshot backup.
<br />

<h2>Purpose</h2>
The purpose of this lab is to understand the concepts of storage within AWS by creating and configuring an EBS (Elastic Block Store) volume, then also seeing how to allow it to interact with instances within AWS.
<br />

<h2>Background Info</h2>
Amazon EBS or Elastic Block Store, similarly to the other AWS services EBS allows for clients to only pay for the storage they use and need for their specific requirements. The EBS volumes are network based and can be attached to other AWS services but is most commonly used as storage options for EC2 instances which is what we used in this lab. EBS is only able to be stored in one availability zone which is fine if your organization is not spread out across multiple availability zones, however if it is Amazon recommends the use of S3 or Simple Storage Service which has the capabilities to span across multiple availability zones, such configurations were not necessary in this lab.
<br />

<h2>Lab Summary</h2>
In this lab I learned how to create a Amazon EBS (Elastic Block Store) Volume and attached it to a working AWS EC2 Instance and went on to create a backup of the instance by taking a snapshot of the instance and placing it in a created file system in our EBS volume.
<br />

<h2>Lab Commands</h2>
  -  df -h – shows available storage on instance.
<br />
  -  sudo mkfs -t ext3 /dev/sdf – created ext3 file system on our volume.
<br />
  -  sudo mkdir /mnt/data-store – created a new directory in our file system for mounting the new volume.
<br />
  -  sudo mount /dev/sdf /mnt/data-store – mounts new volume.
<br />
  -  echo "/dev/sdf /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab – configures instance to mount volume everytime its booted.
<br />
  -  cat /etc/fstab – reads configuration file to verify config.
<br />
  -  sudo sh -c "echo some text has been written > /mnt/data-store/file.txt" – creates and writes a text file.
<br />
  -  sudo rm /mnt/data-store/file.txt – removes a selected file.
<br />
  -  ls /mnt/data-store/ – views the contents of a directory.
<br />

<h2>Walk-Through:</h2>

<p align="center">
Verifying availability zone of the instance to match it to our EBS configurations.<br/>
<img src="images/img1.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
From the left side dashboard I clicked on the Volumes tab to bring me to this page where I can start creating a new volume.<br/>
<img src="images/img2.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
Here I configured the volume with the config shown in the screenshot.<br/>
<img src="images/img3.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
My volume was created and now I will attach it to the instance shown in the very first screenshot.<br/>
<img src="images/img4.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
Here I selected the instance I wanted to attach my created volume to and I clicked attach volume to confirm.<br/>
<img src="images/img5.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then remotely connected to my EC2 instance in PuTTY using a pre-configured certificate provided by the lab, I’ll cover the creation and use of certificates within AWS in a later lab.<br/>
<img src="images/img6.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<img src="images/img7.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
Logged into my ec2 instance through ssh in PuTTY.<br/>
<img src="images/img8.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
Viewing the storage available on my instance using the following command: df -h<br/>
<img src="images/img9.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then created a ext3 file system on our volume using the following command: sudo mkfs -t ext3 /dev/sdf<br/>
<img src="images/img10.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then created a directory for mounting the storage volume using the following command: sudo mkdir /mnt/data-store.<br/>
Then mounted it using: sudo mount /dev/sdf /mnt/data-store<br/>
Then configured the linux instance to mount the volume whenever it is started by stating: echo “/dev/sdf /mnt/data-store ext3 defaults,noatime 1 2” | sudo tee -a /etc/fstab<br/>
<img src="images/img11.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then viewed the file to verify the last line I just configured, and verified my files.<br/>
<img src="images/img12.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then tested my mounted volume by writing some text into a file and attempting to pull and read from that file, which worked.<br/>
<img src="images/img13.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then went back to the volume I created and configured and started the creation of a snapshot under actions.<br/>
<img src="images/img14.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I left everything default except for the tags, and then created the snapshot backup.<br/>
<img src="images/img15.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then removed the test text file I used to test the mounted volume.<br/>
<img src="images/img16.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then went to the snapshots tab in the navigation panel and selected the snapshot I had just made, then under actions I selected the option to create volume from snapshot.<br/>
<img src="images/img17.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
This is how I configured my volume created from the snapshot, I verified that the availability zone matches our initial instance config as well.<br/>
<img src="images/img18.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
I then verified that the volume had been created and reattached it to the instance it was with prior to the removal.<br/>
<img src="images/img19.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<img src="images/img20.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
Back on PuTTY I recreated the mounted file directory and mounted the volume which I verified with the ls command to show the deleted file from an earlier step prior to our backup.<br/>
<img src="images/img21.png" height="80%" width="80%" alt="AWS EBS (Elastic Block Store)"/>
<br />
<br />
</p>

<h2>Problems</h2>
<p align="center">
<img src="images/img22.png" height="60%" width="60%" alt="AWS EBS (Elastic Block Store)"/>
</p>
The only issue I had when doing this lab was adding the required provided certificate into PuTTY to be able to remotely be able to access my instance through SSH, this was an easy process after finding a guide on google but It was difficult at first as I had never done it before, I did not know where I was meant to plug in the certificate but after using the guide off of google and looking around a bit I found it fine.
<br/>
<br/>

<h2>Conclusion</h2>
In this lab I created and configured an EBS (Elastic Block Store) volume and attached and mounted it to a pre-configured EC2 Linux instance. After I had created, configured, and mounted the volume I created a test text file to verify my abilities to create a functional backup using the snapshot service. After I created the text file I created my backup and then deleted the test text file and began the process of accessing my created backup which once I did I verified that I was able to access the deleted test text file.
<br />
