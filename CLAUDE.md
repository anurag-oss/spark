# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build and Development Commands

### Building
- `sbt/sbt package` - Build Spark and all examples
- `sbt/sbt assembly` - Create assembly JAR with all dependencies
- `sbt/sbt compile` - Compile source code only
- `sbt/sbt clean` - Clean build artifacts

### Testing
- `sbt/sbt test` - Run all tests
- `sbt/sbt "project core" test` - Run tests for core module only
- `sbt/sbt "testOnly spark.RDDSuite"` - Run specific test suite
- Tests are located in `*/src/test/scala/` directories

### Running Examples
- `./run <class> <params>` - Run example programs
- `./run spark.examples.SparkPi local[2]` - Run Pi calculation locally with 2 threads
- `./run spark.examples.SparkLR local[2]` - Run logistic regression example

### Interactive Shell
- `./spark-shell` - Start interactive Spark shell

## Project Structure

### Core Architecture
- **SparkContext**: Entry point for all Spark functionality, manages cluster connection and RDD creation
- **RDD (Resilient Distributed Dataset)**: Core abstraction representing immutable, partitioned collections that can be computed in parallel
- **DAGScheduler**: Converts high-level operations into stages and tasks based on RDD dependencies
- **Scheduler**: Abstract interface implemented by LocalScheduler and MesosScheduler for task execution
- **Task**: Unit of work executed on cluster nodes (ShuffleMapTask, ResultTask)

### Module Structure
- `core/` - Core Spark functionality (RDDs, scheduling, serialization, caching)
- `repl/` - Interactive shell implementation
- `examples/` - Example programs demonstrating Spark usage
- `bagel/` - Bagel graph processing library built on Spark

### Key Components
- **Broadcasting**: Efficient distribution of read-only data to all nodes
- **Caching**: In-memory caching of RDD partitions for reuse
- **Serialization**: Pluggable serialization (Java, Kryo) for network transfer
- **Shuffle**: Data redistribution between map and reduce phases
- **Partitioning**: Data distribution strategies across cluster nodes

## Configuration

### Hadoop Version
- Current version: 0.20.205.0 (Hadoop 1.x)
- To change: Edit `HADOOP_VERSION` in `project/SparkBuild.scala`
- For Hadoop 2.x: Set `HADOOP_MAJOR_VERSION = "2"`

### Environment Variables
- `SPARK_HOME` - Spark installation directory
- `SCALA_HOME` - Scala installation directory  
- `SPARK_MEM` - Memory allocation per executor (default: 512m)
- `SPARK_CLASSPATH` - Additional JARs to include
- `MESOS_NATIVE_LIBRARY` - Path to Mesos native library for cluster execution

### Configuration Files
- `conf/spark-env.sh` - Environment variable configuration
- `conf/log4j.properties` - Logging configuration
- `conf/java-opts` - JVM options

## Development Notes

### Scala Version
- Built with Scala 2.9.2
- Uses SBT (Simple Build Tool) for build management

### Scheduler Backends
- **LocalScheduler**: Single-machine execution
- **MesosScheduler**: Distributed execution on Mesos clusters

### Testing Framework
- Uses ScalaTest for unit testing
- JUnit XML output for CI integration
- Test naming convention: `*Suite.scala`