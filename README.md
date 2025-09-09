
# WordCount-Using-MapReduce-Hadoop

This repository is designed to test MapReduce jobs using a simple word count dataset. In this project we provide a input file and then we create a maaper and reducer logic to count the occurence of each word in the given input. There are sample input and Expected output for the sample input.

## Approach and implementation
1. Mapper Logic: We use StringTokenizer to create tokens from the input file and loop it using while loop to map all the words in the input file with key value pairs. In this mapper, it will not count characters that are smaller than 3.

2. Reducer Logic: Using the output of Mapper logic we increase a variable sum value as we encounter same words and retun them. this way we will get a list of words and the number of times it occured in the input file as output.

## Setup and Execution

### 1. **Start the Hadoop Cluster**

Run the following command to start the Hadoop cluster:

```bash
docker compose up -d
```

### 2. **Build the Code**

Build the code using Maven:

```bash
mvn clean package
```

### 4. **Copy JAR to Docker Container**

Copy the JAR file to the Hadoop ResourceManager container:

```bash
docker cp target/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 5. **Move Dataset to Docker Container**

Copy the dataset to the Hadoop ResourceManager container:

```bash
docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 6. **Connect to Docker Container**

Access the Hadoop ResourceManager container:

```bash
docker exec -it resourcemanager /bin/bash
```

Navigate to the Hadoop directory:

```bash
cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
```

### 7. **Set Up HDFS**

Create a folder in HDFS for the input dataset:

```bash
hadoop fs -mkdir -p /input/data
```

Copy the input dataset to the HDFS folder:

```bash
hadoop fs -put ./input.txt /input/data
```

### 8. **Execute the MapReduce Job**

Run your MapReduce job using the following command: Here I got an error saying output already exists so I changed it to output1 instead as destination folder

```bash
hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar com.example.controller.Controller /input/data/input.txt /output1
```

### 9. **View the Output**

To view the output of your MapReduce job, use:

```bash
hadoop fs -cat /output1/*
```

### 10. **Copy Output from HDFS to Local OS**

To copy the output from HDFS to your local machine:

1. Use the following command to copy from HDFS:
    ```bash
    hdfs dfs -get /output1 /opt/hadoop-3.2.1/share/hadoop/mapreduce/
    ```

2. use Docker to copy from the container to your local machine:
   ```bash
   exit 
   ```
    ```bash
    docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output1/ shared-folder/output/
    ```
3. Commit and push to your repo so that we can able to see your output

## Challenges Faces and Solution

The main challenge I face was while trying to run the comands again to check docker was not allowing me to run as the name of the container was already running from my previous run. So I found out that we have to stop the container that are not in use to enable for the same containers to run.

## Sample Input: 
 ```bash
  Lorem Ipsum is simply dummy text of the printing and typesetting industry
  Lorem Ipsum has been the industry's standard dummy text ever since the 1500s
  when an unknown printer took a galley of type and scrambled it to make a type specimen book
  It has survived not only five centuries, but also the leap into electronic typesetting
  remaining essentially unchanged
   ```

## Expected output: 
 ```bash
    the	4
    typesetting	2
    text	2
    dummy	2
    Ipsum	2
    Lorem	2
    and	2
    has	2
    type	2
    scrambled	1
    centuries,	1
    unchanged	1
    industry	1
    five	1
    unknown	1
    been	1
    book	1
    input	1
    simply	1
    took	1
    into	1
    but	1
    ever	1
    since	1
    industry's	1
    own	1
    printer	1
    essentially	1
    leap	1
    printing	1
    specimen	1
    Create	1
    1500s	1
    make	1
    survived	1
    only	1
    not	1
    dataset	1
    your	1
    also	1
    electronic	1
    remaining	1
    galley	1
    when	1
    standard	1
   ```
