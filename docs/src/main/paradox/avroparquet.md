# Avro Parquet

The Avro Parquet connector provides an Apache Pekko Stream Source, Sink and Flow for push and pull data to and from Parquet files.

For more information about Apache Parquet please visit the [official documentation](https://parquet.apache.org/docs/).

@@project-info{ projectId="avroparquet" }

## Artifacts

@@dependency [sbt,Maven,Gradle] {
  group=org.apache.pekko
  artifact=pekko-connectors-avroparquet_$scala.binary.version$
  version=$project.version$
  symbol2=PekkoVersion
  value2=$pekko.version$
  group2=org.apache.pekko
  artifact2=pekko-stream_$scala.binary.version$
  version2=PekkoVersion
}

The table below shows direct dependencies of this module and the second tab shows all libraries it depends on transitively.

@@dependencies { projectId="avroparquet" }

## Source Initiation

Sometimes it might be useful to use a Parquet file as stream Source. For this we will need to create an `AvroParquetReader` 
instance which will produce records as subtypes of `GenericRecord`, the Avro record's abstract representation.
 
Scala
: @@snip (/avroparquet/src/test/scala/docs/scaladsl/AbstractAvroParquet.scala) { #prepare-source #init-reader }

Java
: @@snip (/avroparquet/src/test/java/docs/javadsl/Examples.java) { #init-reader }

After that, you can create the Parquet Source from the initialisation of `AvroParquetReader`. This object requires an instance of 
  a `org.apache.parquet.hadoop.ParquetReader` typed by a subtype of `GenericRecord`.

Scala
: @@snip (/avroparquet/src/test/scala/docs/scaladsl/AvroParquetSourceSpec.scala) { #init-source }

Java
: @@snip (/avroparquet/src/test/java/docs/javadsl/Examples.java) { #init-source }

## Sink Initiation

On the other hand, you can use `AvroParquetWriter` as the Apache Pekko Streams Sink implementation for writing to Parquet. 
In that case, its initialisation would require an instance of `org.apache.parquet.hadoop.ParquetWriter`. It will also expect any subtype of `GenericRecord` to be passed.
 
Scala
: @@snip (/avroparquet/src/test/scala/docs/scaladsl/AbstractAvroParquet.scala) { #prepare-sink }

Java
: @@snip (/avroparquet/src/test/java/docs/javadsl/AvroParquetSinkTest.java) { #init-writer }

After that, the AvroParquet Sink can already be used. 

@@@ div { .group-scala }

The below Scala example demonstrates that *any* subtype of `GenericRecord` can be passed to the stream. In this case the one used is `com.sksamuel.avro4s.Record`, which it implements the `GenericRecord` Avro interface.
See [Avro4s](https://github.com/sksamuel/avro4s) or [Avrohugger](https://github.com/julianpeeters/avrohugger) for other ways of generating these classes.

@@@
 
Scala
: @@snip (/avroparquet/src/test/scala/docs/scaladsl//AvroParquetSinkSpec.scala) { #init-sink }

Java
: @@snip (/avroparquet/src/test/java/docs/javadsl/AvroParquetSinkTest.java) { #init-sink }

## Flow Initiation

The representation of a `ParquetWriter` as a Flow is also available to use as a streams flow stage, in which as well as the other representations, it will expect subtypes of the Parquet `GenericRecord` type to be passed.
 As a result, it writes into a Parquet file and returns the same `GenericRecord`s. Such a Flow stage can be easily created by using the `AvroParquetFlow` and providing an `AvroParquetWriter` instance as a parameter.

Scala
: @@snip (/avroparquet/src/test/scala/docs/scaladsl/AvroParquetFlowSpec.scala) { #init-flow }
This is all the preparation that we are going to need.

Java
: @@snip (/avroparquet/src/test/java/docs/javadsl/Examples.java) { #init-flow }

## Running the example code

The code in this guide is part of the runnable tests of this project. You are welcome to edit the code and run it in sbt.

Scala
:   ```
    sbt
    > avroparquet/test
    ```
