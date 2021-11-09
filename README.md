# Arabesque: Distributed graph mining made simple

[http://arabesque.io](http://arabesque.io)

*Current Version:* 1.0-BETA

Arabesque is a distributed graph mining system that enables quick and easy
development of graph mining algorithms, while providing a scalable and efficient
execution engine running on top of Hadoop.

Benefits of Arabesque:
* Simple and intuitive API, specially tailored for Graph Mining algorithms.
* Transparently handling of all complexities associated with these algorithms.
* Scalable to hundreds of workers.
* Efficient implementation: negligible overhead compared to equivalent centralized solutions.

Arabesque is open-source with the Apache 2.0 license.

## Requirements for running

* Linux/Mac with 64-bit JVM
* [A functioning installation of Hadoop2 with MapReduce (local or in a cluster)](http://www.alexjf.net/blog/distributed-systems/hadoop-yarn-installation-definitive-guide/)

## Preparing your input
Arabesque currently takes as input graphs with the following format:

```
# <num vertices> <num edges>
<vertex id> <vertex label> [<neighbour id1> <neighbour id2> ... <neighbour id n>]
<vertex id> <vertex label> [<neighbour id1> <neighbour id2> ... <neighbour id n>]
...
```

Vertex ids are expected to be sequential integers between 0 and (total number of vertices - 1).

## Test/Execute the included algorithms

You can find an execution-helper script and several configuration files for the different algorithms under the [scripts
folder in the repository](https://github.com/Qatar-Computing-Research-Institute/Arabesque/tree/master/scripts):

* `run_arabesque.sh` - Launcher for arabesque executions. Takes as parameters one or more yaml files describing the configuration of the execution to be run. Configurations are applied in sequence with configurations in subsequent yaml files overriding entries of previous ones.
* `cluster.yaml` - File with configurations related to the cluster and, so, common to all algorithms: number of workers, number of threads per worker, number of partitions, etc.
* `<algorithm>.yaml` - Files with configurations related to particular algorithm executions using as input the [provided citeseer graph](https://github.com/Qatar-Computing-Research-Institute/Arabesque/tree/master/data):
  * `fsm.yaml` - Run frequent subgraph mining over the citeseer graph.
  * `cliques.yaml` - Run clique finding over the citeseer graph.
  * `motifs.yaml` - Run motif counting over the citeseer graph.
  * `triangles.yaml` - Run triangle counting over the citeseer graph.

**Steps:**

1. Compile Arabesque using 
  ```
  mvn package
  ```
  You will find the jar file under `target/`
  
2. Copy the newly generated jar file, the `run_arabesque.sh` script and the desired yaml files onto a folder on a computer with access to an Hadoop cluster. 

3. Upload the input graph to HDFS. Sample graphs are under the `data` directory. Make sure you have initialized HDFS first.

  ```
  hdfs dfs -put <input graph file> <destination graph file in HDFS>
  ```

4. Configure the `cluster.yaml` file with the desired number of containers, threads per container and other cluster-wide configurations.

5. Configure the algorithm-specific yamls to reflect the HDFS location of your input graph as well as the parameters you want to use (max size for motifs and cliques or support for FSM).

6. Run your desired algorithm by executing:

  ```
  ./run_arabesque.sh cluster.yaml <algorithm>.yaml
  ```

7. Follow execution progress by checking the logs of the Hadoop containers.

8. Check any output (generated with calls to the `output` function) in the HDFS path indicated by the `output_path` configuration entry.


## Implementing your own algorithms
The easiest way to get to code your own implementations on top of Arabesque is by forking our [Arabesque Skeleton Project](https://github.com/Qatar-Computing-Research-Institute/Arabesque-Skeleton). You can do this via
[Github](https://help.github.com/articles/fork-a-repo/) or manually by executing the following:

```
git clone https://github.com/Qatar-Computing-Research-Institute/Arabesque-Skeleton.git $PROJECT_PATH
cd $PROJECT_PATH
git remote rename origin upstream
git remote add origin $YOUR_REPO_URL
```


## issue
```
 bash run_arabesque.sh cluster.yaml cliques.yaml
21/11/08 19:13:57 INFO conf.YamlConfiguration: Loading settings from jar:file:/users/zz_y/Arabesque/scripts/arabesque-1.0-BETA-jar-with-dependencies.jar!/arabesque.default.yaml
21/11/08 19:13:58 INFO conf.YamlConfiguration: Loading settings from file:/users/zz_y/Arabesque/scripts/cluster.yaml
21/11/08 19:13:58 INFO conf.YamlConfiguration: Loading settings from file:/users/zz_y/Arabesque/scripts/cliques.yaml
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.nettyRequestEncoderBufferSize
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.nettyRequestEncoderBufferSize] to [1048576] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.nettyServerThreads
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.nettyServerThreads] to [32] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.SplitMasterWorker
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.SplitMasterWorker] to [false] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.useNettyPooledAllocator
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.useNettyPooledAllocator] to [true] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: arabesque.clique.maxsize
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [arabesque.clique.maxsize] to [4] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.nettyClientExecutionThreads
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.nettyClientExecutionThreads] to [32] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.nettyClientThreads
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.nettyClientThreads] to [32] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.channelsPerServer
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.channelsPerServer] to [4] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.useBigDataIOForMessages
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.useBigDataIOForMessages] to [true] in GiraphConfiguration
21/11/08 19:13:58 INFO conf.YamlConfiguration: Invalid YAML key, assuming giraph -ca: giraph.useNettyDirectMemory
21/11/08 19:13:58 INFO conf.YamlConfiguration: Setting custom argument [giraph.useNettyDirectMemory] to [true] in GiraphConfiguration
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/local/hadoop-2.6.0/share/hadoop/common/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/users/zz_y/Arabesque/scripts/arabesque-1.0-BETA-jar-with-dependencies.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
21/11/08 19:13:58 INFO Configuration.deprecation: mapreduce.job.counters.limit is deprecated. Instead, use mapreduce.job.counters.max
21/11/08 19:13:58 INFO Configuration.deprecation: mapred.job.map.memory.mb is deprecated. Instead, use mapreduce.map.memory.mb
21/11/08 19:13:58 INFO Configuration.deprecation: mapred.job.reduce.memory.mb is deprecated. Instead, use mapreduce.reduce.memory.mb
21/11/08 19:13:58 INFO Configuration.deprecation: mapred.map.tasks.speculative.execution is deprecated. Instead, use mapreduce.map.speculative
21/11/08 19:13:58 INFO Configuration.deprecation: mapreduce.user.classpath.first is deprecated. Instead, use mapreduce.job.user.classpath.first
21/11/08 19:13:58 INFO Configuration.deprecation: mapred.map.max.attempts is deprecated. Instead, use mapreduce.map.maxattempts
21/11/08 19:13:58 INFO job.GiraphJob: run: Since checkpointing is disabled (default), do not allow any task retries (setting mapred.map.max.attempts = 1, old value = 4)
21/11/08 19:13:58 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
21/11/08 19:13:58 INFO client.RMProxy: Connecting to ResourceManager at master/10.10.1.1:8032
21/11/08 19:13:58 WARN bsp.BspOutputFormat: checkOutputSpecs: ImmutableOutputCommiter will not check anything
21/11/08 19:13:59 INFO mapreduce.JobSubmitter: number of splits:1
21/11/08 19:13:59 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1636422893765_0003
21/11/08 19:14:00 INFO impl.YarnClientImpl: Submitted application application_1636422893765_0003
21/11/08 19:14:00 INFO mapreduce.Job: The url to track the job: http://master:8088/proxy/application_1636422893765_0003/
21/11/08 19:14:00 INFO job.GiraphJob: Tracking URL: http://master:8088/proxy/application_1636422893765_0003/
21/11/08 19:14:00 INFO job.GiraphJob: Waiting for resources... Job will start only when it gets all 2 mappers
21/11/08 19:14:14 INFO job.HaltApplicationUtils$DefaultHaltInstructionsWriter: writeHaltInstructions: To halt after next superstep execute: 'bin/halt-application --zkServer c220g5-111317.wisc.cloudlab.us:22181 --zkNode /_hadoopBsp/job_1636422893765_0003/_haltComputation'
21/11/08 19:14:14 INFO mapreduce.Job: Running job: job_1636422893765_0003
21/11/08 19:14:15 INFO mapreduce.Job: Job job_1636422893765_0003 running in uber mode : false
21/11/08 19:14:15 INFO mapreduce.Job:  map 100% reduce 0%
21/11/08 19:14:16 INFO mapreduce.Job: Job job_1636422893765_0003 failed with state FAILED due to: Task failed task_1636422893765_0003_m_000000
Job failed as tasks failed. failedMaps:1 failedReduces:0

21/11/08 19:14:16 INFO mapreduce.Job: Counters: 8
	Job Counters
		Failed map tasks=1
		Launched map tasks=1
		Other local map tasks=1
		Total time spent by all maps in occupied slots (ms)=8908
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=8908
		Total vcore-seconds taken by all map tasks=8908
		Total megabyte-seconds taken by all map tasks=9121792
  ```
