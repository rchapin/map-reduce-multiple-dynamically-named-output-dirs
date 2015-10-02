# Generate Dynamically Named Output Directories Based on Reduce Key Values Example

**By:** Ryan Chapin, [Contact Info](http://www.ryanchapin.com/contact.html)

An example of a MapReduce **MRv2** job that writes to dynamically named output directories and files based on the names of the keys passed to the Reducer.

This example is taken pretty much directly from the API docs of the org.apache.hadoop.mapreduce.lib.output.MultipleOutputs<K,V> class.

The purpose was to build a working copy that could be checked-out, ran and built upon for an actual project.

The sample use case is the need to parse a file that contains names and addresses and output each record sorted by the two character state value in each record into a separate directory on HDFS.  Such an output could then be used as more targeted input for a secondary M/R job.  This example does not operate on the records in the mapper or reducer, but simply sorts them.  Any additional logic could be added in either class and is left as an exercise for the reader.

## Building

To build, simply run the following command in the cloned directory.

```
mvn clean package
```

This will build the project and create jars in the target/ directory as expected.  

The ```multiple-dynamically-named-outputs-n.n.n-SNAPSHOT-jar-with-dependencies.jar``` includes all of the dependencies, minus the hadoop/hdfs jars, to run using a ```yarn jar ...``` command on a system that is configured to submit jobs to YARN.

## To Run

There is a sample input data file in ```src/test/resources``` which can be copied to your input directory location on HDFS.

After compilation execute the `yarn jar` command as follows.

```
yarn jar \
target/multiple-dynamically-named-outputs-n.n.n-SNAPSHOT-jar-with-dependencies.jar \
com.ryanchapin.example.hadoop.mosnamedoutputs.Main \
-i /path/to/input/data/on/hdfs \
-o /path/to/output/data/parent/dir/on/hdfs
```

```
usage: com.ryanchapin.example.hadoop.mosnamedoutputs.Main
-i <arg>    Path to the input directory on HDFS
-r <arg>    Path to the output directory on HDFS

```

Each record will be written into a [A-Z]{2}-r-0000 output file in a dynamically named directory based on the key (two char name of the state).

