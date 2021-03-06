Release 0.4.7
------------------------------------

### Highlights
 * Major releases with fundamental changes to filesystem listing & write failure handling
 * Introduced the first version of HoodieTimelineServer that runs embedded on the driver
 * With all executors fetching filesystem listing via RPC to timeline server, drastically reduced filesystem listing!
 * Failing concurrent write tasks are now handled differently to be robust against spark stage retries
 * Bug fixes/clean up around indexing, compaction

### Full PR List
 * **@bvaradar** - HUDI-135 - Skip Meta folder when looking for partitions #698
 * **@bvaradar** - HUDI-136 - Only inflight commit timeline (.commit/.deltacommit) must be used when checking for sanity during compaction scheduling #699
 * **@bvaradar** - HUDI-134 - Disable inline compaction for Hoodie Demo #696
 * **@v3nkatesh** - default implementation for HBase index qps allocator #685
 * **@bvaradar** - SparkUtil#initLauncher shoudn't raise when spark-defaults.conf doesn't exist #670HUDI-131 Zero File Listing in Compactor run #693
 * **@vinothchandar** - Fixed HUDI-116 : Handle duplicate record keys across partitions #687
 * **@leilinen** - HUDI-105 : Fix up offsets not available on leader exception #650
 * **@bvaradar** - Allow users to set hoodie configs figs for Compactor, Cleaner and HDFSParquetImporter utility scripts #691
 * **@bvaradar** - Spark Stage retry handling #651
 * **@pseudomoto** - HUDI-113: Use Pair over # delimited string #672
 * **@bvaradar** - Support nested types for recordKey, partitionPath and combineKey #684
 * **@vinothchandar** - Downgrading fasterxml jackson to 2.6.7 to be spark compatible #686
 * **@bvaradar** - Timeline Service with Incremental View Syncing support #600

Release 0.4.6
------------------------------------

### Highlights
 * Index performance! Interval trees + bucketized checking speed up index lookup upto 10x!
 * Faster writing due to cached avro encoder/decoders, lighter memory usage, lesser data shuffled.
 * Support for spark jobs using > 1 cores per executor
 * DeltaStreamer bug fixes (inline compaction, hive sync, error record handling)
 * Empty Record payload to support deletes out-of-box easily
 * Fixes to hive/spark bundles around dependencies, versioning, shading

### Full PR List

 * **@bvaradar** - Minor CLI documentation change in delta-streamer #679
 * **@n3nash** - converting map task memory from mb to bytes #678
 * **@bvaradar** - Fix various errors found by long running delta-streamer tests #675
 * **@vinothchandar** - Bucketized Bloom Filter checking #671
 * **@pseudomuto** - SparkUtil#initLauncher shoudn't raise when spark-defaults.conf doesn't exist #670
 * **@abhioncbr** - HUDI-101: added exclusion filters for signature files. #669
 * **@ovj** - migrating kryo's dependency from twitter chill to plain kryo library #649
 * **@bvaradar** - Revert "HUDI-101: added mevn-shade plugin with filters." #665
 * **@abhioncbr** - HUDI-101: added mevn-shade plugin with filters. #659
 * **@bvaradar** - Rollback inflights when using Spark [Streaming] write #660
 * **@vinothchandar** - Making DataSource/DeltaStreamer use defaults for combining #634
 * **@vinothchandar** - Fixes HUDI-85 : Interval tree based pruning for Bloom Index #653
 * **@takezoe** - Fix to enable hoodie.datasource.read.incr.filters #655
 * **@n3nash** - Removing OLD MAGIC header #648
 * **@bvaradar** - Revert "Read and apply schema for each log block from the metadata header instead of the latest schema" #647
 * **@lyogev** - Add empty payload class to support deletes via apache spark #635
 * **@bvaradar** - Move to apachehudi dockerhub repository & use openjdk docker containers #644
 * **@bvaradar** - Fix Hive RT query failure in hoodie demo #645
 * **@ovj** - Revert - Replacing Apache commons-lang3 object serializer with Kryo #642
 * **@n3nash** - Read and apply schema for each log block from the metadata header instead of the latest schema #640
 * **@bhasudha** - FIXES HUDI-98: Fix multiple issues when using build_local_docker_images for demo setup #636
 * **@n3nash** - Performing commit archiving in batches to avoid keeping a huge chunk in memory #631
 * **@bvaradar** - Essential Hive packages missing in hoodie spark bundle #633
 * **@n3nash** - 1. Minor changes to fix compaction 2. Adding 2 compaction policies 3. Adding a Hbase index property #629
 * **@milantracy** - [HUDI-66] FSUtils.getRelativePartitionPath does not handle repeated f… #627
 * **@vinothchandar** - Fixing small file handling, inline compaction defaults #599
 * **@vinothchandar** - Follow up HUDI-27 : Call super.close() in HoodieWraperFileSystem::close() #621
 * **@vinothchandar** - Fix HUDI-27 : Support num_cores > 1 for writing through spark #620
 * **@vinothchandar** - Fixes HUDI-38: Reduce memory overhead of WriteStatus #616
 * **@vinothchandar** - Fixed HUDI-87 : Remove schemastr from BaseAvroPayload #619
 * **@vinothchandar** - Fixes HUDI-9 : Check precondition minInstantsToKeep > cleanerCommitsR… #617
 * **@n3nash** - Fixing source schema and writer schema distinction in payloads #612
 * **@ambition119** - [HUDI-63] Removed unused BucketedIndex code #608
 * **@bvaradar** - run_hive_sync tool must be able to handle case where there are multiple standalone jdbc jars in hive installation dir #609
 * **@milantracy** - add a script that shuts down demo cluster gracefully #606
 * **@n3nash** - Enable multi rollbacks for MOR table type #546
 * **@ovj** - Replacing Apache commons-lang3 object serializer with Kryo serializer #583
 * **@kaka11chen** - Add compression codec configurations for HoodieParquetWriter. #604
 * **@smarthi** - HUDI-75: Add KEYS #601
 * **@vinothchandar** - Removing docs folder from master branch #602
 * **@bvaradar** - Fix hive sync and deltastreamer issue in demo #593
 * **@bhasudha** - Fix quickstart documentation for querying via Presto #598
 * **@ovj** - Handling duplicate record update for single partition (duplicates in single or different parquet files) #584
 * **@kaka11chen** - Fix avro doesn't have short and byte type. #595
 * **@bvaradar** - FIleSystem View to handle same fileIds across partitions correctly #572
 * **@vinothchandar** - Upgrade various jar, gem versions for maintenance #575

Release 0.4.5
------------------------------------

### Highlights
 * Dockerized demo with support for different Hive versions
 * Smoother handling of append log on cloud stores
 * Introducing a global bloom index, that enforces unique constraint across partitions
 * CLI commands to analyze workloads, manage compactions
 * Migration guide for folks wanting to move datasets to Hudi
 * Added Spark Structured Streaming support, with a Hudi sink
 * In-built support for filtering duplicates in DeltaStreamer
 * Support for plugging in custom transformation in DeltaStreamer
 * Better support for non-partitioned Hive tables
 * Support hard deletes for Merge on Read storage
 * New slack url & site urls
 * Added presto bundle for easier integration
 * Tons of bug fixes, reliability improvements


### Full PR List

 * **@bhasudha** - Create hoodie-presto bundle jar. fixes #567 #571 
 * **@bhasudha** - Close FSDataInputStream for meta file open in HoodiePartitionMetadata . Fixes issue #573 #574  
 * **@yaoqinn** - handle no such element exception in HoodieSparkSqlWriter #576  
 * **@vinothchandar** - Update site url in README 
 * **@yaooqinn** - typo: bundle jar with unrecognized variables #570 
 * **@bvaradar** - Table rollback for inflight compactions MUST not delete instant files at any time to avoid race conditions #565  
 * **@bvaradar** - Fix Hoodie Record Reader to work with non-partitioned dataset ( ISSUE-561) #569  
 * **@bvaradar** - Hoodie Delta Streamer Features : Transformation and Hoodie Incremental Source with Hive integration #485 
 * **@vinothchandar** - Updating new slack signup link #566   
 * **@yaooqinn** - Using immutable map instead of mutables to generate parameters #559 
 * **@n3nash** - Fixing behavior of buffering in Create/Merge handles for invalid/wrong schema records #558   
 * **@n3nash** - cleaner should now use commit timeline and not include deltacommits #539  
 * **@n3nash** - Adding compaction to HoodieClient example #551   
 * **@n3nash** - Filtering partition paths before performing a list status on all partitions #541  
 * **@n3nash** - Passing a path filter to avoid including folders under .hoodie directory as partition paths #548   
 * **@n3nash** - Enabling hard deletes for MergeOnRead table type #538  
 * **@msridhar** - Add .m2 directory to Travis cache #534   
 * **@artem0** - General enhancements #520 
 * **@bvaradar** - Ensure Hoodie works for non-partitioned Hive table #515  
 * **@xubo245** - fix some spell errorin Hudi #530   
 * **@leletan** - feat(SparkDataSource): add structured streaming sink #486   
 * **@n3nash** - Serializing the complete payload object instead of serializing just the GenericRecord in HoodieRecordConverter #495   
 * **@n3nash** - Returning empty Statues for an empty spark partition caused due to incorrect bin packing #510  
 * **@bvaradar** - Avoid WriteStatus collect() call when committing batch to prevent Driver side OOM errors #512   
 * **@vinothchandar** - Explicitly handle lack of append() support during LogWriting #511   
 * **@n3nash** - Fixing number of insert buckets to be generated by rounding off to the closest greater integer #500   
 * **@vinothchandar** - Enabling auto tuning of insert splits by default #496  
 * **@bvaradar** - Useful Hudi CLI commands to debug/analyze production workloads #477  
 * **@bvaradar** - Compaction validate, unschedule and repair #481   
 * **@shangxinli** - Fix addMetadataFields() to carry over 'props' #484  
 * **@n3nash** - Adding documentation for migration guide and COW vs MOR tradeoffs #470  
 * **@leletan** - Add additional feature to drop later arriving dups #468 
 * **@bvaradar** - Fix regression bug which broke HoodieInputFormat handling of non-hoodie datasets #482 
 * **@vinothchandar** - Add --filter-dupes to DeltaStreamer #478 
 * **@bvaradar** - A quickstart demo to showcase Hudi functionalities using docker along with support for integration-tests #455 
 * **@bvaradar** - Ensure Hoodie metadata folder and files are filtered out when constructing Parquet Data Source #473 
 * **@leletan** - Adds HoodieGlobalBloomIndex #438 
 

Release 0.4.4
------------------------------------

### Highlights
 * Dependencies are now decoupled from CDH and based on apache versions!
 * Support for Hive 2 is here!! Use -Dhive11 to build for older hive versions
 * Deltastreamer tool reworked to make configs simpler, hardended tests, added Confluent Kafka support
 * Provide strong consistency for S3 datasets
 * Removed dependency on commons lang3, to ease use with different hadoop/spark versions
 * Better CLI support and docs for managing async compactions
 * New CLI commands to manage datasets

### Full PR List

 * **@vinothchandar** - Perform consistency checks during write finalize #464 
 * **@bvaradar** - Travis CI tests needs to be run in quieter mode (WARN log level) to avoid max log-size errors #465
 * **@lys0716** - Fix the name of avro schema file in Test #467
 * **@bvaradar** - Hive Sync handling must work for datasets with multi-partition keys #460 
 * **@bvaradar** - Explicitly release resources in LogFileReader and TestHoodieClientBase. Fixes Memory allocation errors #463
 * **@bvaradar** - [Release Blocking] Ensure packaging modules create sources/javadoc jars #461
 * **@vinothchandar** - Fix bug with incrementally pulling older data #458
 * **@saravsars** - Updated jcommander version to fix NPE in HoodieDeltaStreamer tool #443
 * **@n3nash** - Removing dependency on apache-commons lang 3, adding necessary classes as needed #444
 * **@n3nash** - Small file size handling for inserts into log files. #413
 * **@vinothchandar** - Update Gemfile.lock with higher ffi version
 * **@bvaradar** -  Simplify and fix CLI to schedule and run compactions #447
 * **@n3nash** - Fix a failing test case intermittenly in TestMergeOnRead due to incorrect prev commit time #448
 * **@bvaradar**- CLI to create and desc hoodie table #446
 * **@vinothchandar**- Reworking the deltastreamer tool #449
 * **@bvaradar**- Docs for describing async compaction and how to operate it #445
 * **@n3nash**- Adding check for rolling stats not present in existing timeline to handle backwards compatibility #451
 * **@bvaradar** **@vinothchandar** - Moving all dependencies off cdh and to apache #420
 * **@bvaradar**- Reduce minimum delta-commits required for compaction #452
 * **@bvaradar**- Use spark Master from environment if set #454


Release 0.4.3
------------------------------------

### Highlights
 * Ability to run compactions asynchrously & in-parallel to ingestion/write added!!!
 * Day based compaction does not respect IO budgets i.e agnostic of them
 * Adds ability to throttle writes to HBase via the HBaseIndex
 * (Merge on read) Inserts are sent to log files, if they are indexable.

### Full PR List

 * **@n3nash** - Adding ability for inserts to be written to log files #400
 * **@n3nash** - Fixing bug introducted in rollback for MOR table type with inserts into log files #417
 * **@n3nash** - Changing Day based compaction strategy to be IO agnostic #398
 * **@ovj** - Changing access level to protected so that subclasses can access it #421
 * **@n3nash** - Fixing missing hoodie record location in HoodieRecord when record is read from disk after being spilled #419
 * **@bvaradar** -  Async compaction - Single Consolidated PR #404
 * **@bvaradar** - BUGFIX - Use Guava Optional (which is Serializable) in CompactionOperation to avoid NoSerializableException #435
 * **@n3nash** - Adding another metric to HoodieWriteStat #434
 * **@n3nash** - Fixing Null pointer exception in finally block #440
 * **@kaushikd49** - Throttling to limit QPS from HbaseIndex #427

Release 0.4.2
------------------------------------

### Highlights
 * Parallelize Parquet writing & input record read resulting in upto 2x performance improvement
 * Better out-of-box configs to support upto 500GB upserts, improved ROPathFilter performance 
 * Added a union mode for RT View, that supports near-real time event ingestion without update semantics
 * Added a tuning guide with suggestions for oft-encountered problems
 * New configs for configs for compression ratio, index storage levels
 
### Full PR List 

 * **@jianxu** - Use hadoopConf in HoodieTableMetaClient and related tests #343
 * **@jianxu** - Add more options in HoodieWriteConfig #341
 * **@n3nash** - Adding a tool to read/inspect a HoodieLogFile #328
 * **@ovj** - Parallelizing parquet write and spark's external read operation. #294
 * **@n3nash** - Fixing memory leak due to HoodieLogFileReader holding on to a logblock #346
 * **@kaushikd49** - DeduplicateRecords based on recordKey if global index is used #345 
 * **@jianxu** - Checking storage level before persisting preppedRecords #358
 * **@n3nash** -  Adding config for parquet compression ratio #366 
 * **@xjodoin** - Replace deprecated jackson version #367 
 * **@n3nash** - Making ExternalSpillableMap generic for any datatype #350 
 * **@bvaradar** - CodeStyle formatting to conform to basic Checkstyle rules. #360 
 * **@vinothchandar** -  Update release notes for 0.4.1 (post) #371  
 * **@bvaradar** - Issue-329 : Refactoring TestHoodieClientOnCopyOnWriteStorage and adding test-cases #372 
 * **@n3nash** - Parallelized read-write operations in Hoodie Merge phase #370 
 * **@n3nash** - Using BufferedFsInputStream to wrap FSInputStream for FSDataInputStream #373 
 * **@suniluber** -  Fix for updating duplicate records in same/different files in same pa… #380 
 * **@bvaradar** - Fixit : Add Support for ordering and limiting results in CLI show commands #383
 * **@n3nash** - Adding metrics for MOR and COW #365  
 * **@n3nash** - Adding a fix/workaround when fs.append() unable to return a valid outputstream #388  
 * **@n3nash** - Minor fixes for MergeOnRead MVP release readiness #387 
 * **@bvaradar** - Issue-257: Support union mode in HoodieRealtimeRecordReader for pure insert workloads #379 
 * **@n3nash** - Enabling global index for MOR #389 
 * **@suniluber** - Added a new filter function to filter by record keys when reading parquet file #395 
 * **@vinothchandar** - Improving out of box experience for data source #295  
 * **@xjodoin** - Fix wrong use of TemporaryFolder junit rule #411  

Release 0.4.1
------------------------------------

### Highlights

 * Good enhancements for merge-on-read write path : spillable map for merging, evolvable log format, rollback support
 * Cloud file systems should now work out-of-box for copy-on-write tables, with configs picked up from SparkContext
 * Compaction action is no more, multiple delta commits now lead to a commit upon compaction
 * API level changes include : compaction api, new prepped APIs for higher plugability for advanced clients


### Full PR List

 * **@n3nash** - Separated rollback as a table operation, implement rollback for MOR #247
 * **@n3nash** - Implementing custom payload/merge hooks abstractions for application #275
 * **@vinothchandar** - Reformat project & tighten code style guidelines #280
 * **@n3nash** - Separating out compaction() API #282
 * **@n3nash** - Enable hive sync even if there is no compaction commit #286
 * **@n3nash** - Partition compaction strategy #281
 * **@n3nash** - Removing compaction action type and associated compaction timeline operations, replace with commit action type #288
 * **@vinothchandar** - Multi/Cloud FS Support for Copy-On-Write tables #293
 * **@vinothchandar** - Update Gemfile.lock #298
 * **@n3nash** - Reducing memory footprint required in HoodieAvroDataBlock and HoodieAppendHandle #290
 * **@jianxu** - Add FinalizeWrite in HoodieCreateHandle for COW tables #285
 * **@n3nash** - Adding global indexing to HbaseIndex implementation #318
 * **@n3nash** - Small File Size correction handling for MOR table type #299
 * **@jianxu** - Use FastDateFormat for thread safety #320
 * **@vinothchandar** - Fix formatting in HoodieWriteClient #322
 * **@n3nash** - Write smaller sized multiple blocks to log file instead of a large one #317
 * **@n3nash** - Added support for Disk Spillable Compaction to prevent OOM issues #289
 * **@jianxu** - Add new APIs in HoodieReadClient and HoodieWriteClient #327
 * **@jianxu** - Handle inflight clean instants during Hoodie instants archiving #332
 * **@n3nash** - Introducing HoodieLogFormat V2 with versioning support #331
 * **@n3nash** - Re-factoring Compaction as first level API in WriteClient similar to upsert/insert #330


Release 0.4.0
------------------------------------

### Highlights 

 * [Spark datasource API](https://uber.github.io/hoodie/quickstart.html#datasource-api) now supported for Copy-On-Write datasets, across all views
 * BloomIndex can now [prune based on key ranges](https://uber.github.io/hoodie/configurations.html#bloomIndexPruneByRanges) & cut down index tagging time dramatically, for time-prefixed/ordered record keys
 * Hive sync tool registers RO and RT tables now.
 * Client application can now specify the partitioner to be used by bulkInsert(), useful for low-level control over initial record placement
 * Framework for metadata tracking inside IO handles, to implement Spark accumulator-style counters, that are consistent with the timeline
 * Bug fixes around cleaning, savepoints & upsert's partitioner.

### Full PR List

 * **@gekath** - Writes relative paths to .commit files #184
 * **@kaushikd49** - Correct clean bug that causes exception when partitionPaths are empty #202
 * **@vinothchandar** - Refactor HoodieTableFileSystemView using FileGroups & FileSlices #201
 * **@prazanna** - Savepoint should not create a hole in the commit timeline #207
 * **@jianxu** - Fix TimestampBasedKeyGenerator in HoodieDeltaStreamer when DATE_STRING is used #211
 * **@n3nash** - Sync Tool registers 2 tables, RO and RT Tables #210
 * **@n3nash** - Using FsUtils instead of Files API to extract file extension #213
 * **@vinothchandar** - Edits to documentation #219
 * **@n3nash** - Enabled deletes in merge_on_read #218
 * **@n3nash** - Use HoodieLogFormat for the commit archived log #205
 * **@n3nash** - fix for cleaning log files in master branch (mor) #228
 * **@vinothchandar** - Adding range based pruning to bloom index #232
 * **@n3nash** - Use CompletedFileSystemView instead of CompactedView considering deltacommits too #229
 * **@n3nash** - suppressing logs (under 4MB) for jenkins #240
 * **@jianxu** - Add nested fields support for MOR tables #234
 * **@n3nash** - adding new config to separate shuffle and write parallelism #230
 * **@n3nash** - adding ability to read archived files written in log format #252
 * **@ovj** - Removing randomization from UpsertPartitioner #253
 * **@ovj** - Replacing SortBy with custom partitioner #245
 * **@esmioley** - Update deprecated hash function #259
 * **@vinothchandar** - Adding canIndexLogFiles(), isImplicitWithStorage(), isGlobal() to HoodieIndex #268
 * **@kaushikd49** - Hoodie Event callbacks #251
 * **@vinothchandar** - Spark Data Source (finally) #266


Previous Releases
------------
* Refer to [github](https://github.com/uber/hoodie/releases)
