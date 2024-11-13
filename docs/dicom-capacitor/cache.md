# Manipulating the Cache

The cache directory is located at `%ProgramData%\Flux\DICOM Capacitor\cache`.  This directory contains all of the DICOM
data that has been received and currently being processed by DICOM Capacitor.

In designing Capacitor we have made every effort to ensure that it can handle manual manipulation of the cache.  This
means that you can move or delete files from the cache without causing any issues.  Please note that doing so may
cause Capacitor to abort any currently pending operations on the affected files, and output errors or warnings to the
log file.

The cache directory contains the following subdirectories:

- `new/`: Contains incoming unprocessed DICOM data
- `prepared/`: Contains DICOM data that has been prepared for storage
- `receipts/`: Contains Storage Commitment receipts, which are used to confirm that data has been stored
- `logs/`: Contains individual instance logs named:
  `/{YEAR}/{MONTH}/{DAY}/{DESTINATION_AE}/{SOURCE_AE}/{SOP}_{INSTANCE_UID}.log`
  These logs are automatically deleted after `config.yml/daysToKeepLogs` days.
- `pendingneventreportrequest/`: Contains pending N-EVENT-REPORT requests for Storage Commitment
- `rejected/`: Contains DICOM data that has been rejected by recipient nodes
- `expired/`: Contains DICOM data that has expired as per the `config.yml/expiryThreshold` setting
- `failed/`: Contains DICOM data that has failed to be stored for some unexpected reason
