# Manage-Multiple-Containers-Docker
A quick list of commands for manage multiple example containers <br/>
What we want to do:<br/>

<b>
  
 Run a nginx, MySQL and an apache (httpd) server. <br/>
 Run all of them  --detach and give them a name with --name <br/>
 nginx should listen on 80:80 httpd on 8080:80 mysql on 3306:3306 <br/>
 Set an --env var to mysql container to pass in MYSQL_RANDOM_ROOT_PASSWORD=yes <br/>
 Use docker container logs on mysql to find the random password it created on startup <br/>
 Clean it all up <br/>
</b>

 <b>Let's start ensuring that everything is clean before creating new containers </b> <br/>
   <pre>sudo docker ps
</pre><br/>
You should see a table   with only the headers and without running containers <br/>
 <b>Let's create nginx container. </b> <br/>
    <pre>sudo docker container run --publish 80:80 --detach --name MyNginx nginx 
</pre> <br/>
Now you should see the containers running with the sudo docker ps -a <br/>
  <b> Now it's time to create our MySQL container </b> <br/>
<pre> sudo docker container run --publish 3306:3306 --detach --env MYSQL_RANDOM_ROOT_PASSWORD=&quot;yes&quot; --name MyMySQL mysql
</pre> <br/>
   <br/>
Let's read some logs to find out the db psw, first you need to know what is your container's id. <br/>
 you can find it in the line after your command. <br/>
In my case it starts with 4f0 <br/>
so just type <br/>
<pre>sudo docker logs 4f
</pre> 
and search for our password that  should look like this<br/>
GENERATED ROOT PASSWORD: ox1ju5oMae6paishailoxaeGhe5em8io<br/>
<br>
<b> Now it's time to create the last container for the httpd service </b>

<pre>sudo docker container run --publish 8080:80 --detach  --name Myhttpd  httpd
</pre>
