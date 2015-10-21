# nozzle-plugin

[![Build Status](https://travis-ci.org/cloudfoundry/firehose-plugin.svg?branch=master)](https://travis-ci.org/cloudfoundry/firehose-plugin)

## Installation

```bash
 cf add-plugin-repo CF-Community http://plugins.cloudfoundry.org/
 cf install-plugin "Firehose Plugin" -r CF-Community

```

## Usage
User must be logged in as admin

### Options
```
NAME:
   nozzle - Displays messages from the firehose

USAGE:
   cf nozzle

OPTIONS:
   -debug                 -d, enable debugging
   -filter                -f, specify message filter such as LogMessage, ValueMetric, CounterEvent, HttpStartStop
   -no-filter             -n, no firehose filter. Display all messages
   -subscription-id       -s, specify subscription id for distributing firehose output between clients
```

### With Interactive Prompt
```bash
cf nozzle
```

### Without Interactive Prompt
Error message will be displayed for unrecognized filter type

 ```bash
 # For debug
 cf nozzle --debug
 
 # For all messages
 cf nozzle --no-filter
 
 # For Log Messages
 cf nozzle --filter LogMessage
 
 # For HttpStart
 cf nozzle --filter HttpStart
 
 # For HttpStartStop
 cf nozzle --filter HttpStartStop
 
 # For HttpStop
 cf nozzle --filter HttpStop
 
 # For ValueMetric
 cf nozzle --filter ValueMetric
 
 # For CounterEvent
 cf nozzle --filter CounterEvent
 
 # For ContainerMetric
 cf nozzle --filter ContainerMetric
 
 # For Error
 cf nozzle --filter Error
 ```
#### Subscription ID
In order to distribute the firehose data evenly among multiple CLI sessions, the user must specify
the same subscription ID to each of the client connections.

 ```bash
 cf nozzle --no-filter --subscription-id myFirehose
 ```

## Uninstall

```bash
cf uninstall FirehosePlugin
```

## Testing

Run tests
```bash
./scripts/test.sh
```

If you want to install the plugin locally and test it manually
```bash
./scripts/install.sh
```

## Releasing

In order to create a new release, follow these steps

1. Create local tag and binaries
  ```
  ./scripts/build-all.sh release VERSION_NUMBER #(e.g. 0.7.0)
  ```
1. Copy the output of the previous command from the first line (should be '- name: Firehose Plugin' to the last checksum line (should be something like checksum: fde5fd52c40ea4c34330426c09c143a76a77a8db)
1. Push the tag `git push --follow-tags`
1. On github, create new release based on new tag [here](https://github.com/cloudfoundry/firehose-plugin/releases/new)
1. Upload the three binaries from the ./bin folders to the release (Linux, OSX and Win64)
1. Fork [this repo](https://github.com/cloudfoundry-incubator/cli-plugin-repo) and clone it locally
1. Edit the repo-index.yml
  ```
  vi repo-index.yml
  ```
  to override the existing section about the firehose plugin with the text previously copied in Step 2.
1. Push the change to your fork
1. Create a PR against the [original repo](https://github.com/cloudfoundry-incubator/cli-plugin-repo/compare)

```
