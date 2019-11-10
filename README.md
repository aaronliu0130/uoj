# Universal Online Judge

## Dependencies
This is a dockerized version of UOJ. Before installation, please make sure that [Docker](https://www.docker.com/) has already been installed on your OS.

The docker image of UOJ is **64-bit**, so installing on a **32-bit** host OS may fail.

## Installation
First please download [JDK7u76](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u76-oth-JPR) and [JDK8u31](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html#jdk-8u31-oth-JPR). Place them in the `docker/` folder. Make sure you download the files ending in `-linux-x64.tar.gz`. These files are used by judge\_client for judging Java programs. If you are too lazy to download these tremendous files, or you do not plan to judge Java programs, you can simply place two empty .tar.gz files there.

Next, you can run the following command in your terminal: (not the one in the `docker/` directory!)
```sh
./install
```
If everything goes well, you will see `Successfully built <image-id>` in the last line of the output.

To start your main UOJ server, please run:
```sh
docker run -it -p 80:80 -p 3690:3690 <image-id>
```
If you are using docker on Mac OS or having 'std: compile error. no comment' message on uploading problem data, you could use this command as an alternative:
```sh
docker run -it -p 80:80 -p 3690:3690 --cap-add SYS_PTRACE <image-id>
```

The default hostname of UOJ is `local_uoj.ac`, so you need to modify your host file in your OS to map `127.0.0.1` to `local_uoj.ac`. (`/etc/hosts` on Linux) After that, you can access UOJ in your web browser.

The first user registered after the installation of UOJ will be a superuser. If you need another superuser, please register a user and change its `usergroup` to "<samp>S</samp>" in the table `user_info`. Run
```sh
mysql app_uoj233 -u root -p
```
to login to MySQL in the terminal.

Notice that if you want only one judge client, then everything is ok now. Cheers!

However, if you want more judge clients, you need to set up them one by one. Run:
```sh
./config_judge_client
```
Then answer the following questions:

* Judge IP: the IP address of the main server.
* Judge Container ID: the container id of the main server.
* Judge Name: you can take a name you like, such as judger, judger\_2, very\_strong\_judger. (Do not put sh or python special characters such as space without escape and semicolon, it will have consequences.)

After that, a SQL command is given, which we will use later.

Next, we need to run:
```sh
./install_judge_client
```
to build the docker image. If you want to run the judge at the same server, you just need to run
```sh
docker run -it <image-id>
```
And, you need to complete the SQL command given just now with the IP address of the judger docker and modify the database. To someone who does not know how to get the IP address of a docker container, here is the answer:
```sh
docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container-id>
```

Or, if you want to run judger at a different server, you need to copy the image to the other server and run
```sh
docker run -p 2333 -it <image-id>
```
Similarly, you need to complete the SQL command and modify the database. This time, you need to fill with the IP address of the host machine of the judger docker.

You may meet many difficulties during the installation. Good luck and have fun!

## Notes

MySQL default password: root

local\_main\_judger password: judger

You can change the default hostname and something else in `/var/www/uoj/app/.config.php`. However, not all the config is here, haha.

## More Documentation
More documentation is here: [https://vfleaking.github.io/uoj/](https://vfleaking.github.io/uoj/)

## License
MIT License.
