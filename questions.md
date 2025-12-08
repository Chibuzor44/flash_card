What is the primary function of the cloud services layer in Snowflake? | To provide authentication, infrastructure management, metadata management, query parsing and optimization, and access control.
Name the five key functions of Snowflake's cloud services layer. | Authentication; Infrastructure management; Metadata management; Query parsing and optimization; Access control.
What is shared data architecture in Snowflake? | A central data repository that multiple compute clusters access concurrently.
What is multi-cluster architecture in Snowflake and what benefit does it provide? | It separates storage and compute so each can scale independently.
What is a namespace used for in Snowflake? | To qualify database objects like tables by database and schema.
What is a schema privilege related to tasks? | MONITOR TASK.
Is there a limit to the number of schemas that can be created in a Snowflake database? | No, there is no limit to the number of schemas in a database.
What does the INFER_SCHEMA table function do? | It automatically discovers the structure of semi-structured data.
What is the maximum number of unique tags that can be applied to a Snowflake object? | 50 unique tags per object.
How are tags applied to table columns inherited, and can they be overridden? | Tags applied to a table are inherited by columns, but column-level tags can be overwritten.
How do you create a tag with allowed values in Snowflake? | CREATE OR REPLACE TAG tag_name ALLOWED_VALUES = ('sales', 'engineering').
How do you alter a table to apply a tag? | ALTER TABLE table_name SET TAG tag_name = 'value'.
Which function retrieves the allowed values for a tag? | SYSTEM$GET_TAG_ALLOWED_VALUES(tag_context).
What are the two data classification tag types in Snowflake? | PRIVACY_CATEGORY and SEMANTIC_CATEGORY.
How do you create a table with tags using CREATE TABLE? | CREATE TABLE table_name (columns) TAG (tag_name = 'value').
How do you use the ALTER TABLE command to apply tags? | ALTER TABLE table_name SET TAG tag_name = 'value'.
What stored procedure automatically applies system-defined tags to table columns? | SYSTEM$CLASSIFY.
How is SYSTEM$CLASSIFY called and what does it do? | CALL SYSTEM$CLASSIFY('schema.table.column', {'auto_tag': true}); it automatically applies system-defined tags to columns.
Which table type in Snowflake supports primary keys and is suitable for transactional tables? | Hybrid tables.
What happens when multiple users simultaneously access a sequence? | Each user receives a unique sequence value, potentially with gaps.
How do you create a sequence starting at 5000 and incrementing by 5? | CREATE SEQUENCE sequence_name START = 5000 INCREMENT = 5.
How do you convert a secure view to a standard view? | Use UNSET SECURE on the secure view.
What does SHOW VIEWS display for secure views? | Name and metadata are shown, but the SQL definition is hidden.
Name two restrictions when creating a materialized view. | Cannot use JOINs (including self-join); cannot use ORDER BY or window functions.
Are DML operations allowed on materialized views? | No; INSERT, DELETE, UPDATE, MERGE, TRUNCATE, COPY are not allowed.
Which aggregate functions are supported in materialized views? | Additive functions only: SUM, AVG, COUNT, MIN, MAX.
On which Snowflake editions are materialized views available? | Enterprise edition and higher.
What should you consider before creating a materialized view on frequently changing tables? | Avoid materialized views on tables that change frequently because they do not update in real-time.
What is a good use case for materialized views? | External tables, where data doesn't change frequently.
How are materialized views managed in terms of compute? | Materialized views are managed by Snowflake (serverless compute).
Which Account Usage view monitors materialized view refresh history? | MATERIALIZED_VIEW_REFRESH_HISTORY.
What is Information Schema in Snowflake? | A metadata view of existing objects in a database.
How does Account Usage differ from Information Schema? | Account Usage includes dropped objects and additional historical metadata that Information Schema does not include.
What column in Account Usage indicates whether a table has been dropped? | DELETED.
What does LAST_ALTERED reflect in Account Usage? | The last structural change to an object.
What does TABLE_TYPE column show in Account Usage? | The kind of object, such as BASE TABLE or VIEW.
What does RETENTION_TIME indicate in Account Usage? | How long Time Travel data is kept for an object.
What is DIRECT_OBJECTS_ACCESSED in Account Usage Access History? | A JSON array describing objects that were directly referenced in a query.
What is the maximum nesting depth for views? | 20 nested views.
What happens when you clone an object with the COPY GRANTS option? | The cloned object inherits current source privileges but not future privileges.
Does cloning a table include its load history? | No, cloning a table does not include load history.
What privilege is required to clone a table? | SELECT privilege on the source table.
Which types of stages can be cloned? | Only external named stages (stages referencing external storage) can be cloned.
List five object types that can be cloned. | Database (usage required); Schema (depends); Table (select required); Materialized view (depending on edition); Named external stage (with external storage).
What privilege is required to clone a pipe referencing external storage? | Ownership is required.
List four object types that cannot be cloned. | External tables; Named internal stage; Internal stages (user stage, table stage); Some account-level objects (users, roles, network policies).
Can a consumer clone a shared database provided by a provider? | No, a shared database can only be queried, not cloned.
What types of pipes can be cloned when cloning a database or schema? | Only pipes referencing external storage (S3, Azure) with AUTO_INGEST configuration.
How does the pipe's state depend when cloning with AUTO_INGEST? | The state depends on the value of AUTO_INGEST (true or false).
What table permanence types can a PERMANENT table be cloned as? | PERMANENT, TEMP, or TRANSIENT.
What table permanence types can TRANSIENT or TEMP tables be cloned as? | Only TEMP or TRANSIENT (same or lower permanence).
Can you make a clone of an existing clone? | Yes, you can make a clone of existing clones.
How does cloning differ from replication in terms of scope? | Cloning is within an account; replication is between two accounts.
How does data move in cloning versus replication? | In cloning, data is not moved; in replication, data is physically moved.
What are the two types of replication in Snowflake? | Database replication and failover/replication group replication.
What is the difference between a replication group and a failover group? | A replication group provides a read-only replica; a failover group allows the secondary to become read/write for disaster recovery.
Which Snowflake edition is required to replicate account-level objects or use failover? | Business Critical Edition or higher.
What is the read-only status of a secondary database in replication? | Secondary databases are read-only.
List six object types that can be replicated. | Schema; Views; Data configuration; Code; Streams and tasks; Tags, alerts, and security integrations.
List five object types that cannot be replicated. | Temp tables; External tables; Temp stages; Files in internal/external stages; Account-level objects (roles, grants, warehouses, resource monitors).
What function disables replication for a primary database? | SYSTEM$DISABLE_DATABASE_REPLICATION.
How do you set up a primary database for replication? | ALTER DATABASE database_name ENABLE REPLICATION TO ACCOUNTS = ('account_name').
What happens when a retention period is set on a database or schema? | Objects created within inherit the retention period by default.
What incurs storage costs related to retention? | Both time travel and fail-safe incur storage costs.
What command specifies the retention time for a table? | DATA_RETENTION_TIME_IN_DAYS.
What happens when a table is created with the name of a dropped table within the retention period? | A new version of the table is created.
Who can access or restore data in FAIL-SAFE? | Only Snowflake employees can access or restore data during fail-safe.
How long is FAIL-SAFE available after the retention period for permanent tables? | 7 days after the retention period (only for permanent tables).
Do FAIL-SAFE and TIME TRAVEL require extra storage? | Yes, both require extra storage, which incurs additional cost.
Is FAIL-SAFE a default feature in all Snowflake editions? | Yes, FAIL-SAFE is a default feature available with all Snowflake editions.
What is the retention period for temporary and transient tables? | 1 day retention with 1 day time travel.
Do temporary and transient tables have fail-safe? | No, temporary and transient tables have no fail-safe.
What is the default retention in Standard tier? | 1 day retention.
What is the maximum retention in Enterprise and above editions? | Up to 90 days (default is 1 day).
What command shows the table history for time travel? | SHOW TABLE HISTORY LIKE 'table_name' IN database_name.schema_name.
What parameters can be used with AT and BEFORE clauses for time travel? | TIMESTAMP, OFFSET, and STATEMENT.
Which clause can only be used with STATEMENT and not with TIMESTAMP or OFFSET? | BEFORE clause (works only with STATEMENT parameter, e.g., BEFORE (STATEMENT => 'query_id')).
In what order is data loaded in Snowflake? | Data is loaded in its natural order.
How do you force Snowflake to load a file again that was previously loaded? | Set FORCE = TRUE in the COPY command.
Can the PUT command be run from Snowsight? | No, PUT command can only be run from the CLI (e.g., SnowSQL).
What file size range is recommended to optimize parallel load operations? | 100–250 MB or larger (compressed).
What is a micro-partition in Snowflake? | A storage block containing 50–500 MB of uncompressed data.
What metadata does each micro-partition contain for each column? | Min, max, count, null count, count of distinct values, and partition-level stats.
Which metadata fields in micro-partitions help with partition pruning? | Min and max values.
What are two modes for continuous data loading with Snowpipe? | User-managed virtual warehouse or Snowflake-managed service.
How long is metadata maintained for data loaded to a table? | 64 days.
Can a task cron expression specify a time zone? | Yes, cron expressions in task definitions support specifying a time zone.
How many SQL statements can a task execute per run? | A task can execute only a single SQL statement.
What are two execution modes for tasks? | User-managed virtual warehouse or Snowflake-managed serverless compute.
What parameter controls the initial warehouse size for serverless tasks? | USER_TASK_MANAGED_INITIAL_WAREHOUSE_SIZE.
What is the row limit for the VALUES clause in an INSERT statement? | 16,384 rows.
What command should be used to bypass the VALUES row limit for large files? | Use COPY INTO instead of INSERT VALUES.
What values can be supplied to the VALUES clause besides actual values? | NULL and DEFAULT.
What is the simplest way to append data in Snowflake? | Using INSERT.
What does MERGE do in Snowflake? | It conditionally inserts, updates, or deletes matching rows.
Name four types of Snowflake streams. | Append-Only; Insert-Only; Standard; Update-Only.
What metadata field indicates if an action in a stream is an UPDATE? | METADATA$ISUPDATE (returns a boolean).
What metadata field tracks a row uniquely over multiple updates in a stream? | METADATA$ROW_ID.
What metadata field shows whether a change is an INSERT or DELETE in a stream? | METADATA$ACTION.
List seven object types that streams support. | Standard tables (including shared); Views (including secure views); External tables; Event tables; Iceberg tables; Dynamic tables; Directory tables.
What is the stream offset? | The pointer at which the last stream was read from; it indicates when DML operations occurred so latest changes can be read and the pointer is updated.
What data types are supported for unloading? | CSV, JSON, Parquet.
How do you unload data as a single file, and what is the trade-off? | Set SINGLE = TRUE; it produces one file but is slower because there is no parallelism.
Can you unload directly to S3 without using a stage object? | Yes, by supplying the integration and bucket URL.
What determines the number of partitions of data unloaded? | The size of the warehouse.
What is the default file format and compression when unloading data? | CSV with GZIP compression.
How do you unload data to JSON or Parquet format? | Use the FILE_FORMAT option in the UNLOAD statement.
What is the default file size when unloading data? | Approximately 16 MB per file.
How do you control the upper limit of file sizes generated during unload? | Use MAX_FILE_SIZE parameter.
List six file types supported for loading. | CSV, JSON, AVRO, ORC, PARQUET, XML.
What does Snowpipe's AUTO_INGEST = TRUE do? | It automatically detects new files in a stage.
What happens when Snowpipe's AUTO_INGEST = FALSE? | Files must be manually ingested using the REST API.
How long is Snowpipe load history stored in metadata? | 14 days.
What authentication method does Snowpipe use for REST calls? | JWT tokens.
What file staging frequency is recommended for optimal Snowpipe cost and performance? | At least 1 file per minute, with each file being 100–250 MB (compressed).
What should you do if files are small? | Batch them until they reach the recommended size (100–250 MB compressed).
What are three optional Snowpipe parameters? | ERROR_INTEGRATION; AWS_SNS_TOPIC; INTEGRATION.
What does semi-structured data refer to in Snowflake? | Data formats like JSON, AVRO, ORC, PARQUET, XML.
What parameter makes the GET command perform better when downloading large files? | PARALLEL parameter.
List three ON_ERROR options for data loading. | SKIP_FILE (_num, _num%); CONTINUE; ABORT_STATEMENT.
When both a stage and the COPY INTO statement define a file format, which takes precedence? | The file format specified in the COPY INTO statement takes precedence.
Why doesn't changing virtual warehouse size impact data loading speed? | Because data loading is an I/O operation, not compute-bound.
What warehouse size is recommended for data loading? | XSmall (xsmall).
What JSON-specific parameter controls duplicate handling in FILE_FORMAT? | ALLOW_DUPLICATE.
How do you specify a file format in COPY INTO? | FILE_FORMAT = (FORMAT_NAME = format_name).
What is the maximum number of files that can be uploaded at once in Snowsight? | 250 files at a time.
What is the maximum file size for a single file upload in Snowsight? | 250 MB.
What are two file functions for retrieving paths or URLs for unstructured files? | GET_STAGE_LOCATION and BUILD_STAGE_FILE_URL.
What functions create URLs for stage files that can be used in REST API GET requests? | BUILD_SCOPED_FILE_URL and BUILD_STAGE_FILE_URL.
What is the GEOGRAPHY data type used for? | Loading GeoJSON containing longitude and latitude data.
Which Snowflake tiers have 90-day time travel/retention? | All tiers except Standard tier.
Does Virtual Private Snowflake (VPS) have access to Data Marketplace? | No, VPS does not have access to Data Marketplace.
What is Snowflake's release schedule? | Weekly.
Name two MFA delivery options for Snowflake authentication. | Push notification on the Duo mobile app and phone call.
What uniquely identifies a specific Snowflake account? | The account identifier.
What is the format of a Snowflake account identifier? | <orgname>-<account_name>.
What are two Snowflake purchase plans? | On-demand and Pre-Purchased Capacity.
What command creates a Snowflake account? | CREATE ACCOUNT.
What function checks what organization a Snowflake account belongs to? | CURRENT_ORGANIZATION_NAME().
What function checks if a role is defined in the current session? | IS_ROLE_IN_SESSION().
What symbol is used to reference a named stage in Snowflake? | @ (at sign).
What symbol is used to reference a user stage? | @~ (at sign followed by tilde).
What symbol is used to reference a table stage? | @% (at sign followed by percent).
Name two automatically created stage types in Snowflake. | Table stage and user stage.
What command downloads data from a stage? | GET.
What command refreshes metadata of an internal stage? | ALTER STAGE stage_name REFRESH.
What metadata fields does a directory table add to stage files? | FILE_URL, RELATIVE_PATH, MD5, ETAG.
What is a SCOPED_URL in Snowflake? | A highly restricted, user-specific URL that only the user who generated it can access.
How long does a SCOPED_URL remain valid? | 24 hours.
Which function generates a SCOPED_URL? | BUILD_SCOPED_FILE_URL.
What is a PRE-SIGNED_URL in Snowflake? | A public URL with access to a specific file for users or tools not authenticated in Snowflake.
What is the default expiration time for a PRE-SIGNED_URL? | 24 hours.
Which function generates a PRE-SIGNED_URL? | GET_PRESIGNED_URL.
What privilege is required to execute COPY INTO from a stage? | READ privilege on the stage.
What REST API endpoint retrieves a file from a stage? | /api/files/.
How long is the persisted query result cached by default? | 24 hours (retention is reset each time the query is reused up to 31 days, then purged).
How do you manually disable result caching in a session? | ALTER SESSION SET USE_CACHED_RESULT = FALSE.
What view and column track query cache usage? | SNOWFLAKE.ACCOUNT_USAGE.QUERY_HISTORY; PERCENTAGE_SCANNED_FROM_CACHE column.
What conditions must be met for query result caching to apply? | The query and data must be identical; result cache is global and accessible to users with matching role and permission.
Which non-deterministic functions are not eligible for caching? | CURRENT_TIMESTAMP() and RANDOM().
Can the query profiler be viewed before a query completes? | Yes, the profiler can be viewed even before the query is complete.
How do you check if a query used warehouse cache in the profiler? | Check the "Percentage scanned from cache" metric.
What is Search Optimization Service (SOS) used for? | To efficiently identify and scan relevant micro-partitions for point-lookup queries (e.g., WHERE id = 'xyz').
What metadata structure does SOS use? | A "search access path" metadata structure.
What costs are associated with SOS? | Storage cost for the metadata structure and compute cost for building and updating the access path.
On which Snowflake edition is SOS available? | Enterprise edition and higher.
How do you add search optimization to a table? | ALTER TABLE table_name ADD SEARCH OPTIMIZATION.
What is Query Acceleration Service (QAS)? | A feature for offloading compute-intensive portions of eligible queries to specialized server resources.
On which Snowflake edition is QAS available? | Enterprise edition.
What function checks if a query qualifies for QAS? | SYSTEM$ESTIMATE_QUERY_ACCELERATION (uses query_id).
What does SYSTEM$ESTIMATE_QUERY_ACCELERATION return? | Status (eligible/ineligible) and estimatedQueryTime object.
What parameter limits the maximum size of resources QAS can use? | QUERY_ACCELERATION_MAX_SCALE_FACTOR.
What is the maximum value for QUERY_ACCELERATION_MAX_SCALE_FACTOR? | 100.
What is the default value for QUERY_ACCELERATION_MAX_SCALE_FACTOR? | 8.
How do you enable query acceleration on a warehouse? | ENABLE_QUERY_ACCELERATION = TRUE (set at warehouse creation or use ALTER WAREHOUSE).
When is QAS suitable to use? | For large table scans with selective filters and large data-loading queries that insert/copy high volume of new rows.
How do you enable query acceleration on an existing warehouse? | ALTER WAREHOUSE warehouse_name SET ENABLE_QUERY_ACCELERATION = TRUE.
What does the OVERWRITE keyword do in an INSERT statement? | It truncates the table and repopulates it atomically, good for refreshing data instead of appending.
Name four approximate estimation functions in Snowflake. | approx_count_distinct; approximate_jacard_index; approx_top_k; approx_percentile.
What function cancels a query using its ID? | SYSTEM$CANCEL_QUERY (takes query_id as input).
What is ECONOMY scaling policy for warehouses? | Queries queue for ~6 minutes before a new cluster is started; supports cost efficiency.
What is STANDARD scaling policy for warehouses? | Snowflake prioritizes performance by quickly adding more clusters to minimize queues.
What is the minimum Snowflake tier that allows multi-cluster warehouses? | Enterprise tier.
Name two warehouse modes of operation. | Auto-Scale and Maximized.
What does Maximized mode mean for a warehouse? | Min = Max > 1 (cluster count is fixed).
What are two suspension parameters for warehouses? | AUTO_SUSPEND and AUTO_RESUME.
What does AUTO_SUSPEND control? | When a warehouse is automatically suspended after inactivity.
What does AUTO_RESUME control? | Whether a warehouse resumes when new queries arrive.
How do you check what warehouses are running, suspended, or resizing? | SHOW WAREHOUSES.
What parameter controls the number of queries handled by a cluster at a time? | MAX_CONCURRENCY_LEVEL.
What parameter controls how long a statement waits in queue before being canceled? | STATEMENT_QUEUED_TIMEOUT_IN_SECONDS.
Is multi-cluster available in Standard tier? | No, multi-cluster is available only in Enterprise and higher.
How are Snowflake credits charged for virtual warehouses? | Per second with a minimum charge of 1 minute.
What is the Quiesce mode for warehouses? | A state where warehouses are waiting to finish completing queries after suspend is run.
List three types of compute resources that incur costs. | Virtual warehouse; Serverless compute; Cloud services compute.
What is the maximum activity history visible in the warehouse activity chart in Snowsight? | Up to 2 weeks of activity.
What is the minimum billing charge time for virtual warehouse usage? | 1 minute.
What variables affect Snowflake credit pricing? | Cloud provider, region, and account edition.
List three features billed by credit. | Virtual warehouse; Cloud service; Serverless features.
List two features billed at a flat rate per TB. | Data storage (per month) and data transfer.
How is serverless warehouse billed? | Per-second at feature-specific rates.
What is the recommended ordering for multi-column cluster keys? | Order columns from lowest cardinality to highest cardinality.
What is cluster depth? | The depth of overlapping micro-partitions (value of 1 or greater).
How do you use cluster depth to determine clustering benefit? | It tells you how many micro-partitions Snowflake must scan for a particular column to access a row range.
What function provides a detailed JSON report of clustering depth and overlap? | SYSTEM$CLUSTERING_INFORMATION.
What does a low cluster depth value indicate? | Better clustering.
How does cluster depth change over time? | Cluster depth increases as DML operations are performed.
How many cluster keys does Snowflake recommend per table? | Not more than 3–4 clustering keys.
What is Tri-Secret Secure? | A feature combining a customer-managed key and a Snowflake-maintained key.
On which Snowflake editions is Tri-Secret Secure available? | Business Critical and higher.
What does Tri-Secret Secure enable? | Bring Your Own Key (BYOK) functionality.
Which key management services support Tri-Secret Secure? | AWS, Azure, and GCP.
What notification methods does Resource Monitor support? | Email and Web UI.
What does periodic rekeying ensure in Snowflake encryption? | All customer data are always encrypted.
Describe the three-layer encryption key hierarchy in Snowflake. | Root key (encrypts account master keys); Account master key (encrypts table master keys); Table master key (encrypts file keys for data).
What device does the root key use for encryption? | Cloud-hosted Hardware Security Module (HSM).
When row-level and column-level security policies are both applied, which is processed first? | Row access policy is applied first to filter rows, then column masking is applied.
What does an unauthorized role see when querying a masked column? | Masked column data depending on how the masking policy was defined.
What must match between the masking policy return value and the column? | The returned data type must match the data type of the first input column.
What happens when a join is performed on a masked column via hashing? | The join returns no result because masking applies before the query executes.
What is the default access behavior when a role lacks access to an object? | Access is denied by default.
What is RBAC in Snowflake? | Role-Based Access Control: Privileges → Role → Users.
What is UBAC in Snowflake? | User-Based Access Control: privileges granted directly to users.
What must be set to ALL for UBAC to work? | USE_SECONDARY_ROLES must be set to ALL.
What happens if USE_SECONDARY_ROLES = NONE? | UBAC is ignored and RBAC is used.
What is the first step to grant table access in a schema to a role? | GRANT USAGE first on the schema.
How do you grant select on future tables in a schema to a role? | GRANT SELECT ON FUTURE TABLES IN SCHEMA schema_name TO ROLE role_name.
What is DAC in Snowflake? | Discretionary Access Control: each object has an owner who grants access.
What global privilege allows managing grants? | MANAGE GRANTS.
Name two MFA delivery options. | Receive a phone call or receive a push notification.
How do you temporarily disable MFA for a user? | ALTER USER user_name SET MINS_TO_BYPASS_MFA = 30 or SET DISABLE_MFA = TRUE.
What federated authentication method is supported? | SAML2 security integration (connect with external IdP).
What SnowSQL parameter is used for MFA? | --mfa-passcode.
What is ACCOUNTADMIN role focused on? | Account-level privileges.
What does SYSADMIN role manage? | Objects in a single account.
What does SECURITYADMIN role manage? | Security within an account.
What does ORGADMIN/GLOBALORGADMIN role manage? | Organization-level administration.
What is needed to enable account/database replication across accounts? | ORGADMIN/GLOBALORGADMIN role (after which ACCOUNTADMIN configures and manages).
Can a secondary role execute CREATE TABLE or CREATE VIEW? | No, these commands must be executed with the primary role.
What are Network Rules in Snowflake? | Value lists that feed into Network Policies (allow or block lists).
What function provides Snowflake hostname, IP address, and ports for private connectivity? | SYSTEM$ALLOWLIST_PRIVATELINK().
Where can network policies be applied? | User level; Account level; Security integration (federated auths) level.
What intermediary sits between Snowflake and a remote service? | Proxy service (manages auth, subscription, billing, etc.).
What is Avro? | An RPC framework originally developed for Hadoop.
What languages does Snowpark support? | Python, Java, and Scala.
What does a secure UDF do? | It disables certain optimizations to avoid leaking sensitive data.
Which languages can UDFs be written in? | JavaScript, Java, Python, and Scala (can also use Snowpark API).
What must be specified for a UDF? | Runtime version and handler.
Can JavaScript UDFs call themselves recursively? | Yes, JavaScript UDFs can call themselves recursively.
What does a secure UDF conceal? | Logic, imports, handler name, handler code, and packages.
How do you create a secure UDF? | CREATE OR REPLACE SECURE FUNCTION function_name(params) ... 
How do you check if a UDF is secure? | Use SHOW FUNCTIONS and check the IS_SECURE column.
What parameter stores a Java UDF compiled JAR file? | target_path.
Which languages can be uploaded via stage for UDFs? | Java, Python, and Scala.
Which languages can be written in-line for UDFs? | Java, Python, JavaScript, Scala, and SQL.
What are three types of errors in SQL scripting stored procedures? | STATEMENT_ERROR (DDL/DML failure); EXPRESSION_ERROR (expression/casting error); OTHER (fallback for uncaught exceptions).
Do stored procedures typically return values? | No, they typically do not return values; this is optional.
What are stored procedures primarily used for? | Performing actions like updating and deleting data.
Which languages can stored procedures be created with? | JavaScript, SQL, and Snowpark (Python, Java, Scala).
What are two rights options for stored procedures? | Caller's rights or owner's rights.
What is an external function? | A UDF that uses a proxy service in between for extra security and subscription model.
What are limitations of external functions? | No query optimization, can only be scalar, cannot be shared, and less secure.
What database connectivity methods do Java and C++ applications use in Snowflake? | JDBC and ODBC.
How are variables referenced in Snowflake SQL scripting blocks? | Variables referred to with a colon (e.g., :my_var).
How are session variables referenced in Snowflake SQL scripting? | Session variables referred to with a dollar sign (e.g., $my_var).
What is the maximum size of a VARIANT value? | 16 MB of uncompressed data per VARIANT value.
Can a VARIANT value contain a full object or document? | Yes, if it does not exceed 16 MB uncompressed.
How do you extract a value from a VARIANT column? | Use the GET function (e.g., SELECT GET(variant_column, 'key')::STRING).
What example metadata structure is shown for VARIANT extraction? | Metadata = {"name": "John", "age": 30}; SELECT GET(metadata, 'name')::STRING AS first_name FROM table.
Which object types can be shared via Snowflake Data Sharing? | Databases; Tables; Dynamic tables; External tables; Apache Iceberg tables; Secure views; Secure materialized views; UDFs (secure and unsecure).
How many databases can be created from a single share? | Only 1 database can be created from a share.
What technology makes Data Sharing possible? | The global services layer.
Which view types can be shared? | Only secure views (and secure materialized views).
Which UDFs can be shared? | Both secure and unsecure UDFs.
Does VPS support data sharing? | No, Virtual Private Snowflake (VPS) does not support data sharing.
Are new objects automatically added to a share when created in a shared database? | No, you must explicitly add grants to the share to make new objects accessible.
Can you share a database across regions? | No, you cannot share directly across regions.
What must the provider do to share data across regions? | Replicate the data to an account in the target region before creating the share.
Who can create a share by default? | Only ACCOUNTADMIN.
Can share creation be delegated to other roles? | Yes, ACCOUNTADMIN can delegate share creation to other roles.
What privilege is required for a consumer role to create a database from a share? | IMPORT privilege.
What is a reader account? | An account created by the provider for consumers who don't have a Snowflake account.
Who pays for reader account charges? | The provider (the data provider is billed for reader account costs).
What type of account is a reader account? | Read-only account; other data than the provider's cannot be added to it.
What are two benefits of being a data consumer? | Instant access to live data and no need for ETL.
What happens when a provider drops a share? | The database in the consumer account becomes inaccessible.
What is a personalized listing in the Snowflake Marketplace? | An invite-only listing for sensitive data; users must request access to be manually approved.
What is a standard listing in the Snowflake Marketplace? | A listing where users can click request and only accept a service agreement.
What is a Data Exchange in Snowflake Marketplace? | A private marketplace where a provider has listings for a select group of customers.
What is required to establish a Data Exchange? | Contact Snowflake to establish it.
What is the role of a Data Exchange Administrator? | Responsible for configuring the exchange and managing members.
What are two sampling modes in Snowflake? | Fraction-based sampling and fixed-size sampling.
What is SYSTEM sampling in fraction-based mode? | Sampling that uses a percentage fraction (e.g., SAMPLE SYSTEM (10)).
What parameters enable reproducibility in SYSTEM sampling? | SEED and REPEATABLE (e.g., SAMPLE SYSTEM (10) SEED (42)).
What is BERNOULLI sampling? | A row-level fraction-based sampling method (e.g., SAMPLE BERNOULLI (10)).
What is ROW sampling? | A row-level fraction-based sampling method (alias for BERNOULLI).
What is the default sample type if not specified? | BERNOULLI (using SAMPLE (10) without specifying type defaults to BERNOULLI).
When applying SAMPLE to a join result, which sampling type must be used? | BERNOULLI or ROW sampling (not SYSTEM).
What is fixed-size sampling? | Sampling a fixed number of rows (e.g., SAMPLE (10 ROWS)).
Does fixed-size sampling support SEED or REPEATABLE? | No, fixed-size sampling does not support SEED or REPEATABLE.
What is the latency of Information Schema? | Instant (no latency).
What is the retention period for Information Schema? | 7 days to 6 months.
What is the typical latency for Account Usage views? | 45 minutes to 3 hours.
What is the retention period for Account Usage? | 1 year.
What is the latency for Query History? | 45 minutes.
Who has permission to see Query History? | Only ACCOUNTADMIN.
What is the maximum data exchange size for Streamlit? | 32 MB.
Does Streamlit use the privileges of the role that owns it? | Yes.
Is a warehouse required to execute Streamlit? | Yes, a warehouse is required.
What SnowSQL option specifies the database? | -d / --dbname.
What SnowSQL option specifies the virtual warehouse? | -w / --warehouse.
What SnowSQL option specifies the role? | -r / --rolename.
What SnowSQL option specifies the user? | -u.
What SnowSQL option specifies the schema? | -s / --schemaname.
What SnowSQL parameters are used to execute SQL queries? | --query and --filename.
What is the default path for SnowSQL configuration? | ~/.snowsql/config.
What port is used for testing HTTP communication in SnowCD? | Port 443.
List ten serverless features in Snowflake. | Clustered Tables; Copy Files; Data Quality Monitoring; Hybrid Tables Requests; Logging; Materialized Views maintenance; Open Catalog; Organization Usage; Query Acceleration; Replication.
List ten more serverless features in Snowflake. | Search Optimization Service; Sensitive Data Classification; Serverless Alerts; Serverless Tasks; Serverless Tasks Flex; Snowpipe; Snowpipe Streaming; Trust Center; Continues beyond these.
What does Sensitive Data Classification in serverless features allow? | Columns to be marked as having sensitive or PII data.
