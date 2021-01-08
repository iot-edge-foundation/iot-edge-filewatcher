# iot-edge-filewatcher

Azure IoT Edge module which reads CSV files and puts the rows as messages on the IoT Edge routing 

## Image URI

This module is open source so you can build your own Docker container.

For convenience, a Linux AMD64 version is availabel at svelde/iot-edge-filewatcher:1.0.0-amd64 

## Container create options

This module makes use of Docker volumes to detect and access CSV files on disk.

Use these container create options:

```
{
  "HostConfig": {
    "Binds": [
      "[Host folder]:/app/exchange"
    ]
  }
}
```

An example is:

```
{
  "HostConfig": {
    "Binds": [
      "/var/iot-edge-filewatcher/exchange:/app/exchange"
    ]
  }
}
``` 

The module will automatically create the 'exchange' folder. Please give the folder elevated rights to allow renaming files after processing. 

This can be done with:

```
sudo chmod 666 exchange
```

## Module twin desired properties

The module check the 'exchange' folder every few seconds to detect new files using a certain search pattern.

if a file is found, it expects the column names on the first row and it tries to map the column to the rows in the file.

It splits each line using a delimeter.

After processing, the file is renamed using a new extension which is appended to the orginal filename.

With desired properties, we can change the behavior:

* delimiter, a single character - Default ','
* interval, the interval in milliseconds - Default 10000
* searchPattern, the pattern mapped on filenames  - Default '*.txt'
* renameExtension - the appended extension to prevent a file being read twice - Default '.old'

## Messages

The messages outputted are equal to the columns and lines of the incoming file.

If the file contains 11 rows (1 header row and 10 rows with values), the module will send 10 messages.

Empty lines in the file are ignored.

Each messages gets also these extra values:

* lineNumber
* fileName
* timestamp
* moduleId
* deviceId

## File access

Keep in mind the module needs access to the folder. Please check the console log output of the module to see if right are elevated correctly. 

A simple check for access to the file is built in.

## File location

This module is tested with files available on the local file system only.

## Contributions

Sourcecode is available at [GitHub](https://github.com/iot-edge-foundation/iot-edge-filewatcher).

An example container is available on [Docker Hub](https://hub.docker.com/repository/docker/svelde/iot-edge-filewatcher).

Our project accepts GitHub pull requests :-) 
