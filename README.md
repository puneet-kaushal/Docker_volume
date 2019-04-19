# Docker_volume
## Volume 
<ol>
<li><strong>Volumes</strong> are the preferred mechanism for persisting data generated by and used by docker containers.</br>
In docker whenever, you create a container they has to be someplace where the data for the container will be stored. Now in case you do not provide any explicit location, for that data to be stored it gets stored within the container, and when you delete the container or remove the container the data is also lost. However, when you work on enterprise projects, we won't that data is not lost, We can actually remove a container but still persist the data. in this case, It required to create more containers with the old data or to share the data between container it should be possible.</li>

<li>Details about docker volume</li>  
<strong>$ docker volume</strong></br>
<strong>$docker volume --help</strong></br>

![1](https://user-images.githubusercontent.com/47202519/56355616-60797d80-61f4-11e9-9f52-c10eadb6e276.png)

<ol>
<li>Create a volume</br>
docker volume create name_of_volume</br>
<strong>$ docker volume create myvol</strong></li> 

![c_vol](https://user-images.githubusercontent.com/47202519/56355268-8a7e7000-61f3-11e9-8824-ad69893c4bdb.png)

<li>To check the list of volume</br>
<strong>$ docker volume ls</strong></li>  

![ls](https://user-images.githubusercontent.com/47202519/56355705-9e76a180-61f4-11e9-95eb-a44338d6e615.png)

<li>To get the details about the volume</br>
<strong>$ docker volume inspect myvol</strong></br> 

![2](https://user-images.githubusercontent.com/47202519/56355810-e4cc0080-61f4-11e9-8656-6897f971295f.png)

<strong>Mountpoint</strong>: is the location of your volume and it can not be edited by the functions locally, that is one of the advantages and this volume is safe now.</li>
<li>To remove the volume</br>
docker volume rm name_of_volume</br>
<strong>$ docker volume rm myvol</strong></li>

![3](https://user-images.githubusercontent.com/47202519/56355915-23fa5180-61f5-11e9-96d7-850e1fa9fcee.png)

<li>To remove all unused volume you can use:-</br>
<strong>$ docker volume prune</strong></br>    

![4](https://user-images.githubusercontent.com/47202519/56356091-86ebe880-61f5-11e9-9f2a-b37912555eae.png)

It will remove all local volumes which are not used by any one of the containers.</li>    

![4 2](https://user-images.githubusercontent.com/47202519/56356166-aa169800-61f5-11e9-88f5-73b6d0f5144f.png)

</ol>

### How it works
<ol>
  
<li>pull the jenkins image</br>
<strong>$ pull the jenkins image</strong></li>  

<li> To start container</br>
<strong>--name =name_of_container</strong></br>
<strong>-p:flag</strong> to expose the ports=8080 on the local system and 8080 on the jenkins server.</br> 
For the API(Application Programming Interface) we using <strong>port=50000</strong>, with the name of image.</br>
Run this command, it will surely start the Jenkins container and we will be able to have it running on <strong>localhost:8080</strong></br> 
Attach a volume= <strong>-v:flag</strong> name_of_volume that we just created. This will correspond to the jenkins home directory</br> 

docker run --name name_of_container -v name_of_volume:it should get data from where jenkins home</br> 
<strong>$ docker run --name myjenkins2 -v myvol2:/var/jenkins_home -p 8080:8080 -p 50000:50000 jenkins</strong></br>

![6 1](https://user-images.githubusercontent.com/47202519/56357079-3a55dc80-61f8-11e9-9373-c45b86a48ba2.png)

We want all the data of jenkins home to be available in our volume</li>

<li>Need to copy this initial admin password</li>   

![6 2](https://user-images.githubusercontent.com/47202519/56357416-5312c200-61f9-11e9-9c7c-2e03dd2ab991.png)

<li>Open localhost:8080 on the browser </br>
Provide the admin password that we copied</li>  

![6 3](https://user-images.githubusercontent.com/47202519/56357617-d7654500-61f9-11e9-8c35-5706e13cc964.png)

<li>We getting a option to select plugins to install</br>

![6 4](https://user-images.githubusercontent.com/47202519/56358492-63786c00-61fc-11e9-85cc-b8affe474b28.png)

Click <strong>'none'</strong> from the menu bar to all, to save time and then install it.</br>
Skip the process by click on <strong>continue as admin</strong></br>
click on <strong>'start using jenkins'</strong></li>  

![6 5](https://user-images.githubusercontent.com/47202519/56358511-6f642e00-61fc-11e9-84d5-e53474832c07.png)

<li>Main idea here is to show how can we share the volumes</li> 
<li>In jenkins <strong>create a new job</strong></li>  

![6 6](https://user-images.githubusercontent.com/47202519/56358559-928edd80-61fc-11e9-8c76-cee9bf55b409.png)

<li>Enter an item name</br>
<strong>eg: testjob1</strong></br>
Click on <strong>freestyle project</strong></li>    

![6 7](https://user-images.githubusercontent.com/47202519/56358707-18128d80-61fd-11e9-9694-d6a52d9c792e.png)

<li>Add build steps->click on <strong>'execute shell'</strong> write command (like: ls) and apply to save it.</li>  

![6 8](https://user-images.githubusercontent.com/47202519/56358792-5a3bcf00-61fd-11e9-8250-cf809ad165dc.png)

<li>Go to the dash board click on jenkins :- you can see the <strong>testjob1</strong> is created</li>    

![6 9](https://user-images.githubusercontent.com/47202519/56359317-db479600-61fe-11e9-85d9-5f8cef704072.png)

<li>Now start another jenkins container:- open another terminal </br>
Change the name_of_container and use same volume that we used for eariler jenkins container</br>
Use another port because 8080 is already in use.</br> 
<strong>$ docker run --name myjenkins3 -v myvol2:/var/jenkins_home -p 9090:8080 -p 60000:50000 jenkins</strong></br>    

![6 10](https://user-images.githubusercontent.com/47202519/56359266-af2c1500-61fe-11e9-815e-635b61910170.png)</br>

It will start another instance of Jenkins or a different docker container with Jenkins</li>

<li>Now we need to check the volume is shared between these two Jenkins or not</br> 
Jenkins is fully up an running </br>
Open <strong>localhost:9090</strong></li>  

![6 11](https://user-images.githubusercontent.com/47202519/56359450-3c6f6980-61ff-11e9-9d0d-00a9c521a66b.png)

<li>It has not given us the initial plug-in  screen it is just giving us login screen 
<strong>username=admin</strong>
same password that we had for last jenkins</li>
<li>If this allows us to log in means the data is being shared with these two Jenkins containers.It allowed us to login and you can see this has the testjob1 is here. 
So data is being shared between both Jenkins container which running on <strong>8080 port</strong> and this container which is running on <strong>port 9090</strong>.
So we are able to share the volume between these two containers.
The volume will remain intact even if we close any of these containers or even delete or remove the containers. We will still have our volume that we can use for any other container as well.</li>  

![6 12](https://user-images.githubusercontent.com/47202519/56360005-ccfa7980-6200-11e9-900e-bb3256d3f40d.png)

</ol>

### Bind Mount 
<li>A file or directory on the host machine is mounted into a container.</li>
<ol>
<li><strong>Bind mounts</strong> means you can actually use physical location instead of volume.<br>
<strong>$ docker run --name myjenkins_bm -v /home/pardise/work/jenkins:/var/jenkins_home -p 9191:8080 -p 40000:50000 jenkins</strong></br>
change port no = 9191 and 40000</br>
<strong>name = myjenkins_bm(container_name)</strong></br>
instead of using volume we going to use physical location of our system.</br>
<strong>/home/pardise/work/jenkins</strong></br>
Create a folder jenkins, all the data of jenkins_home will get stored here.</li>  

![6 13](https://user-images.githubusercontent.com/47202519/56360320-ed770380-6201-11e9-9e11-4359b35ba24d.png)

<li>Some new file and folder copied in jenkins folder.</br>
Open <strong>localhost:9191</strong></br>
Open another terminal and check running containers</br>
<strong>$ docker ps</strong></li>    

![6 14](https://user-images.githubusercontent.com/47202519/56360411-35962600-6202-11e9-88a5-a45bcaaf7877.png)

<li>Stop the running containers</li>  

![6 15](https://user-images.githubusercontent.com/47202519/56360478-5d858980-6202-11e9-8966-ae00dd9fa144.png)

<li>Remove the container</li>   

![6 16](https://user-images.githubusercontent.com/47202519/56360510-7d1cb200-6202-11e9-84ef-9f92499d8ba6.png)

<li>Remove all containers </li>  

![6 17](https://user-images.githubusercontent.com/47202519/56360553-a8070600-6202-11e9-9e7b-761133a7563c.png)

<li>Now check the volume myvol2 is still there</li>  

![6 18](https://user-images.githubusercontent.com/47202519/56360608-d4bb1d80-6202-11e9-97ad-ad41c5fb44a1.png)

<li>We can use same volume with another container to share the data with any other container</br>
<strong>$ docker volume inspect myvol2</strong></li>  

![6 19](https://user-images.githubusercontent.com/47202519/56360633-e43a6680-6202-11e9-87f3-f151fb57591c.png)</br>

Our physical jenkins_home is created in our system.
</ol>
</ol>
