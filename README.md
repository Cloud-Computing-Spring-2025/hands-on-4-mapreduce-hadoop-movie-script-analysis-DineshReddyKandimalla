Project Overview
This project implements a Movie Script Analysis using Hadoop MapReduce. The goal is to process a dataset of movie dialogues, analyze the word frequency, and generate insights into the most commonly used words in scripts.

Approach and Implementation
Mapper Logic: Reads each line from the input file. Splits the line into words using a StringTokenizer. Emits (word, 1) pairs for each word.

Example Output from Mapper:
hello 1 world 1

Reducer Logic: Receives (word, [1, 1, 1, ...]) as input from the mapper. Sums up the values to get the total count for each word. Emits (word, count) as output.
Example Output from Reducer: hello 2

Setup and Execution
1. Start the Hadoop Cluster
Run the following command to start the Hadoop cluster:

docker compose up -d
2. Build the Code
Build the code using Maven:

mvn install
3. Move JAR File to Shared Folder
Move the generated JAR file to a shared folder for easy access:

mv target/*.jar input/code/
4. Copy JAR to Docker Container
Copy the JAR file to the Hadoop ResourceManager container:

docker cp input/code/hands-on2-movie-script-analysis-1.0-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
5. Move Dataset to Docker Container
Copy the dataset to the Hadoop ResourceManager container:

docker cp input/data/movie_dialogues.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/
6. Connect to Docker Container
Access the Hadoop ResourceManager container:

docker exec -it resourcemanager /bin/bash
Navigate to the Hadoop directory:

cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/
7. Set Up HDFS
Create a folder in HDFS for the input dataset:

hadoop fs -mkdir -p /input/dataset
Copy the input dataset to the HDFS folder:

hadoop fs -put ./movie_dialogues.txt /input/dataset
8. Execute the MapReduce Job
Run your MapReduce job using the following command:

hadoop jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/hands-on2-movie-script-analysis-1.0-SNAPSHOT.jar com.movie.script.analysis.MovieScriptAnalysis /input/dataset/movie_dialogues.txt /output
9. View the Output
To view the output of your MapReduce job, use:

hadoop fs -cat /output/*
10. Copy Output from HDFS to Local OS
To copy the output from HDFS to your local machine:

Use the following command to copy from HDFS:

hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/
use Docker to copy from the container to your local machine:

exit 
docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ output/

Challenges:
Issues regarding docker connection and moving dataset from localmachine to hdfs.
The path was different and correcting it resolved the issue.

