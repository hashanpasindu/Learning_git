# xk6-kafka


1. Install the latest version of Go(Recommended to use [gvm](https://github.com/moovweb/gvm))

2. Install `xk6`:
      ```shell
      $ go install github.com/k6io/xk6/cmd/xk6@latest
     ```
3. Set up the Kafka Development environment

    Prerequisites - Install Docker
    1. Download [docker_image](https://media.datacumulus.com/confluent-schemareg/code.zip) 
    2. Move to `2-start-kafka` folder(this is a folder in the docker image) and execute the below command
         ```shell
            $ docker-compose up
         ```

      After executing the above command, visit [localhost:3030](http://localhost:3030) to get into the fast-data-dev environment.

4. Clone this git repository and move to xk6-kafka folder. 


      To avoid getting the following error while running the test (The reason for this error is not having the topic which we use in the k6 script):

      ```bash
       Failed to write message: [5] Leader Not Available: the cluster is in the middle of a leadership election and there is currently no leader for this partition and  hence it is unavailable for writes
      ```
      A topic can be created in the fast-data-dev environment using either of two ways.
      - Create a topic in the fast-data-dev environment using the docker

      Execute the below commands inside the docker container to create the topic you used in the test script(adding `partitions` and `replication-factor` is optional)
      ```shell
            kafka-topics --create --topic <name of the topic> --bootstrap-server localhost:9092 --partitions 2 --replication-factor 1
      ```
      or 
      - Create a topic in the fast-data-dev environment using the xk6 script

      Run the following command in the xk6-kafka folder
      ```shell
      xk6 run scripts/test_topics.js
      ```

5. Run the test using the below command.
```shell
xk6 run scripts/main.js
```

------------------------------- Optional----------------------------------------------------------------------------------------

Following command can be used if you want to build directly the binary from the k6 Kafka extension
Build the binary:
  ```shell
  $ xk6 build --with github.com/mostafa/xk6-kafka@latest
  ```
Test can be exeucted using this command

```bash
$ ./k6 run scripts/test_poc.js
```


Listed below are some of the other useful commands which can be execute in container

To view the created topic
```shell
  $ kafka-topics --bootstrap-server localhost:9092 --list 
  ```
To view more details about the topics
```shell
  $ kafka-topics --topic test1 --bootstrap-server localhost:9092 --describe 
  ```

### For more information
Visit [xk6-kafka](https://github.com/mostafa/xk6-kafka)  
