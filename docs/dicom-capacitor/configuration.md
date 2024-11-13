# Configuration

---

### config.yml

The `config.yml` file contains the following settings:

#### scpPort (default: 104)

The port that DICOM Capacitor will listen on for Store SCP requests (default: 104)

#### aeTitle (default: `FLUX_CAPACITOR`)

The AE Title that DICOM Capacitor will use to identify itself, unless overridden by the `nodes.yml` file.

#### cachePath (default: `%ProgramData%\Flux\DICOM Capacitor\cache`)

The path to the cache directory that DICOM Capacitor will use to store DICOM data.  We recommend that you add this
folder to your antivirus exclusion list.

#### filters (default: ``)

A comma-separated list of filters that DICOM Capacitor will apply to incoming DICOM data.

Filter values include:

- `route`:  Routes incoming images to different nodes, discards, or saves to disk.
  Requires `routings.yml` to be present in the same directory as `config.yml`.
- `mutate`: Mutates incoming images based on a set of mutations.
  Requires `mutations.yml` to be present in the same directory as `config.yml`
- `sort`:   Re-orders DICOM images based on configurable criteria.
  Requires `sortings.yml` to be present.
- `siemens_cto_converter`: Converts all Siemens CT images to DICOM BTO format
- `hologic_sco_converter`: Converts all Hologic SCO images to DICOM BTO format

#### activationCode (default: ``)

The activation code that DICOM Capacitor will use to activate the software.  If a code is not provided, DICOM Capacitor
will run in trial mode.

#### aeTitlePrefix (default: `DCP_`)

Associations have the flexibility to utilize either the node's actual AE title or the prefixed AE title. For instance,
both `STORE_SCP` and `DCP_STORE_SCP` would point toward the same node.  This may be required when the sending device
does not allow for duplicate AE title entries.

#### autoAdjustLineSpeed (default: `false`)

Automatically adjusts the `MinimumLineSpeed` (more on that later) based on the recent transfer speeds.

#### autoSaveLineSpeed (default: `false`)

Determines whether the `MinimumLineSpeed` should be saved to the `nodes.yml` file.

#### batchSize (default: `0`)

The number of DICOM images to process in a single batch.  A value of `0` indicates that DICOM Capacitor should process
images as quickly as it can, and try to complete series as a single batch.

#### clientTimeout (default: `600`)

The number of seconds that DICOM Capacitor will wait for a client to connect before timing out.

#### ctoConvert (default: `false`)

This is a special setting that must be set to true if you are using the Siemens CTO Converter filter.

#### ctoExpiryMinutes (default: `30`)

The number of minutes that DICOM Capacitor will wait before expiring a CTO.

#### ctoOnExpire (default: `expire`)

Determines what to do with CTOs that have expired.  Possible values include:

- `expire`: Moves the expired CTO to the `expired` directory
- `accept`: Accepts the expired CTO as a new CT
- `delete`: Deletes the expired CTO (treat this with caution)

#### cycleRejectedFilesOnStartup (default: `true`)

Determines whether DICOM Capacitor should re-process files that were rejected during the last run.

#### daysToKeepLogs (default: `1`)

The number of days that DICOM Capacitor will keep individual instance logs before deleting them.

#### defaultTransferSyntax (default: ``)

The default transfer syntax that DICOM Capacitor will use to store DICOM images.  If this value is not provided,
DICOM Capacitor will use the transfer syntax of the incoming image.

Accepted values are:

[Accepted transfer syntax values](#transfer-syntaxes)

Lossless:

- ImplicitVRLittleEndian
- ExplicitVRLittleEndian
- ExplicitVRBigEndian
- JPEGProcess14SV1 or JPEGLossless
- JPEGProcess14
- JPEG2000Lossless
- RLELossless

Lossy:

- JPEG2000Lossy
- JPEGLSNearLossless
- JPEGProcess1
- JPEGProcess2_4

#### dropUnfilteredInstances (default: `false`)

Determines whether DICOM Capacitor should drop instances that do not match any filters. **BE CAREFUL WITH THIS SETTING
AS IT CAN LEAD TO DATA LOSS**

#### duplicateBehavior (default: `accept`)

Determines how DICOM Capacitor should handle duplicate instances.  Possible values include:

- `accept`: Accepts all instances, regardless of whether they are duplicates
- `reject`: Rejects duplicate instances if they are received within the `duplicateThreshold` time
- `ignore`: Accepts all instances, but discards duplicates if they are received within the `duplicateThreshold` time

#### duplicateThreshold (default: `900`)

The number of seconds that DICOM Capacitor will use to determine whether an instance is a duplicate.

#### echoInterval (default: `0`)

The number of seconds that DICOM Capacitor will wait between sending DICOM Echo requests to clients. Use this to avoid
excessive network traffic.  The default value of `0` disables this feature.

#### expiryThreshold (default: `604800`)

The number of seconds that DICOM Capacitor will use to determine whether an instance has expired. Expired instances
will be deleted from the cache.

#### findScpPort (default: `1041`)

The port that DICOM Capacitor will use to listen for Find SCP requests.

#### logSuffix (default: `#{2,2}-#{8,50}-#{10,20}`)

Attributes displayed inside log files.  The default value will display:
`SOPClass-AccessionNumber-StudyDescription`, e.g., `BT_ACC12345-12345678`

#### maxClientsPerServer (default: `0`)

The maximum number of clients that DICOM Capacitor will allow to connect to a single server. This is a technical limit
and should be set to `0` unless you have a specific reason to change it.

#### maxDataBuffer (default: `1048576`)

The maximum size of the data buffer that DICOM Capacitor will use for communication.

#### maxEnumeratedItems (default: `0`)

The maximum number of items that DICOM Capacitor will enumerate when searching a directory for files.

#### maxPDVsPerPDU (default: `1`)

The maximum number of PDVs that DICOM Capacitor will use per PDU. Leave this value at `1` unless you have a specific
reason to change it.

#### minFreeSpace (default: `10`)

The minimum percentage of free space that DICOM Capacitor will require on disk while still receiving data.

#### minimumAge (default: `0`)

The number of seconds Capacitor will wait before processing an instance.

#### minimumLineSpeed (default: `0`)

The minimum transmission speed, in KB/s, that DICOM Capacitor will use to determine if a storage operation has failed.
Capacitor will proactively abort the transfer if the average speed falls significantly below this threshold.

#### moveRejectedCachedFiles (default: `true`)

Determines whether DICOM Capacitor should move rejected files to the `rejected` directory.

#### myAeTitle (default: `FLUX_CAPACITOR`)

The AE Title that DICOM Capacitor will use to identify itself when sending DICOM data and responding to DICOM queries.
This value can be overridden by "impersonation" in the `nodes.yml` file.

#### processSchedule (default: ``)

A YAML array of time ranges that DICOM Capacitor will use to determine when to process data, e.g.,
`- 13:30 - 23:59` will process data between 1:30 PM and midnight.

#### queryRetrieveScpPort (default: `1042`)

The port that DICOM Capacitor will use to listen for Query/Retrieve SCP requests.

#### rejectAbstractSyntaxes (default: ``)

A YAML array of Abstract Syntaxes, in UID format, that DICOM Capacitor will reject.

#### removeOrphanedIncomingFiles (default: `true`)

Determines whether DICOM Capacitor should remove orphaned incoming files from the cache.

#### storageCommitAeTitle (default: `COMMIT_SCP`)

The AE Title that DICOM Capacitor will use to identify itself when sending DICOM Storage Commitment requests.

#### storageCommitPendingNEventReportRequestExpiry (default: `12000`)

The number of seconds that DICOM Capacitor will use to determine whether a Storage Commitment request has expired.

#### storageCommitScpPort (default: `1043`)

The port that DICOM Capacitor will use to listen for Storage Commitment SCP requests.

#### storageCommitSendTimeout (default: `15000`)

The number of milliseconds that DICOM Capacitor will use to determine whether a Storage Commitment request has timed out.

#### storageInterval (default: `250`)

The number of milliseconds that DICOM Capacitor will wait between storage operations.

#### storageReceiptExpiryHours (default: `1`)

The number of hours that DICOM Capacitor will use to determine whether a Storage Commitment receipt has expired.
Expired receipts will be deleted from the `cache/receipts/` directory.

#### studyThreshold (default: `0`)

The number of seconds that DICOM Capacitor will wait before arrival of the last image before processing
the images contained in a single study.  This setting is important if you are using the `sort` filter.

#### worklistDays (default: `1`)

The number of days of worklist records that Capacitor will attempt to retrieve as part of cached `Worklist`
queries.

#### worklistFilterStored (default: `false`)

TODO: Document this setting

#### worklistInterval (default: `30000`)

The number of milliseconds that DICOM Capacitor will wait between worklist queries.

#### Command Line config.yml Overrides

Most `config.yml` settings can be explicitly overriden via command line options when prefixed with `--config.`, 
e.g., `--config.myAeTitle=MY_AE_TITLE` is analogous to `myAeTitle: MY_AE_TITLE` in the config.yml file.

#### Environment Variable config.yml Overrides

Most `config.yml` settings can also be overridden via environment variables by prefixing the setting name with 
`CAPACITOR_`, e.g., `CAPACITOR_MY_AE_TITLE=AE_TITLE` is analogous to `myAeTitle: AE_TITLE` in the config.yml file.

Command line overrides supersede environment variable overrides which in turn supersede config.yml file settings. This 
allows flexible configuration of DICOM Capacitor via environment variables that can be set externally in 
container/VM environments without needing to modify config files.

### nodes.yml

The `nodes.yml` file is a list of nodes that DICOM Capacitor will use to send and receive DICOM data.

A node is defined as follows, e.g., for a Storage SCP node:

```yaml
---
- AeTitle: STORE_SCP
  Aliases:
    - PACS
  HostName: 127.0.0.1
  Port: 104
  TransferSyntax:
  NodeRole: Storage
  BatchSize: 0
  MinimumLineSpeed: 0
  ProcessSchedule:
    - 00:00 - 23:59
  ProcessNetworks:
    - 192.168.100.4 / 255.255.255.0
```

#### AeTitle (default: `DESTINATION`)

The AE Title that is used when initiating associations with this node.  In the example above, clients that wish to send
DICOM data to this node should use the *Called* AE Title `STORE_SCP`.

Capacitor will respond affirmatively to any C-ECHO and C-Store association intended for this AE Title, as well as this
AE title prefixed with `config.yml/aeTitlePrefix`.

#### Aliases (optional, default: `[]`)

A YAML array of aliases that DICOM Capacitor will use to identify the node.  In the example above, associations
intended for the `PACS` will be delivered to `STORE_SCP` instead.

Capacitor will respond affirmatively to any C-ECHO and C-Store association intended for these aliases.

#### HostName (default: `127.0.0.1`)

The IP address or hostname of the node.

#### Port (default: `104`)

The port that the node is listening on.

#### TransferSyntax (default: ``)

The transfer syntax that Capacitor will use when sending data to this node.  If this value is not provided, Capacitor
will use the transfer syntax of the incoming image, or the value defined in `config.yml/defaultTransferSyntax` if set.

Accepted values are the same as those in `config.yml/defaultTransferSyntax` ([see above](#transfer-syntaxes)).

#### NodeRole (default: `Storage`)

The role of the node. Accepted values are:

- `Storage`: The node is a Storage SCP, and Capacitor will relay data to it
- `QueryRetrieve`: The node is a Query/Retrieve SCP, and Capacitor will relay queries to it
- `Worklist`: The node is a Worklist SCP, and Capacitor will relay queries to it, and cache (and mutate) the results
- `StorageCommitSCP`: The node is a Storage Commitment SCP, and Capacitor will relay requests to it
- `StorageCommitSCU`: The node is a Storage Commitment SCU, and Capacitor will relay requests to it

Specific roles may require and allow additional configuration in `config.yml`, `nodes.yml`, and `mutations.yml`.

#### BatchSize (default: ``)

The number of DICOM images to process in a single batch.  Any value other than blank overrides the value set in
`config.yml/batchSize` for only this node.

#### MinimumLineSpeed (default: ``)

The minimum transmission speed, in KB/s, that DICOM Capacitor will use to determine if a storage operation has failed.
This value overrides the `config.yml/minimumLineSpeed` setting for this node.

#### ProcessSchedule (default: ``)

A YAML array of time ranges that DICOM Capacitor will use to determine when to process data for this node.

#### ProcessNetworks (default: ``)

A YAML array of CIDR ranges DICOM Capacitor will compare against all local network adapters to determine when to
process data for this node.

Examples:
  ```yaml
    ProcessSchedule:
    - 00:00 - 23:59
    ProcessNetworks:
    - 192.168.100.4 / 255.255.255.0  # requires specific subnet using mask
    - 192.168.100.0                  # requires specific subnet address
    - 192.168.100.4                  # requires specific ip address (no mask)
  ```
