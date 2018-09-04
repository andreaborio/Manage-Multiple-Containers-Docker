# Managing Multiple Containers Docker
# Quick start example
Handbook of commands for managing multiple containers <br/>
What we want to achieve:<br/>

<b>
  
 Run a nginx, MySQL and an apache (httpd) server. <br/>
 Run all of them  --detach and give them a name with --name <br/>
 nginx should listen on 80:80 httpd on 8080:80 mysql on 3306:3306 <br/>
 Set an --env var to mysql container to pass in MYSQL_RANDOM_ROOT_PASSWORD=yes <br/>
 Use docker container logs on mysql to find the random password created on startup <br/>
 Clean all <br/>
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


It's time to clean up!
Now you should have something like this 
<pre>CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
60a26e21a411        httpd               &quot;httpd-foreground&quot;       29 minutes ago      Up 24 seconds       0.0.0.0:8080-&gt;80/tcp                Myhttpd
4f053e51ff2c        mysql               &quot;docker-entrypoint.s…&quot;   About an hour ago   Up 16 seconds       0.0.0.0:3306-&gt;3306/tcp, 33060/tcp   MyMySQL
9361d770d456        nginx               &quot;nginx -g &apos;daemon of…&quot;   About an hour ago   Up 3 seconds        0.0.0.0:80-&gt;80/tcp                  MyNginx
</pre>

Now lets simply run sudo docker stop for each container <br /> 
<pre><font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker stop 60
60
<font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker stop 4f
4f
<font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker stop 93
93
<font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
</pre>
<b> Fine! Now we have to delete containers </b> <br/>
In fact if we add -a flag to ps command we can see that we have only stop containers but they are still present on our system.
<pre><font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
60a26e21a411        httpd               &quot;httpd-foreground&quot;       38 minutes ago      Exited (0) 5 minutes ago                       Myhttpd
4f053e51ff2c        mysql               &quot;docker-entrypoint.s…&quot;   About an hour ago   Exited (0) 5 minutes ago                       MyMySQL
9361d770d456        nginx               &quot;nginx -g &apos;daemon of…&quot;   About an hour ago   Exited (0) 5 minutes ago                       MyNginx
<font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker rm 60
60
<font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker rm 4f
4f
<font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker rm 93
93
</pre>

Let's check if everything's gone fine
<pre><font color="#586E75"><b>chinaski@chinaski-XPS-15-9550</b></font>:<font color="#839496"><b>~</b></font>$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
</pre>

<h2> <b> MISSION ACCOMPLISHED! </b> </h2> <br/>
Thanks for your attention!<br/>
PS In this tutorial i've run each command without condensed them . But you can also run commands composed like "sudo docker rm firstid secondid ..." <br/>

Have a great day! <br/>
© Andrea Borio 2018
