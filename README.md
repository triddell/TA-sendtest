# TA-sendtest

TA-sendtest provides Splunk configurations to faciliate testing [`TA-sendcef`](https://github.com/triddell/TA-sendcef) and [`TA-sendraw`](https://github.com/triddell/TA-sendraw) on a local Splunk instance.

## Installation

Use normal Splunk installation procedures to install the TA. This includes installation through the Splunk UI or "unzipping" and copying to `$SPLUNK_HOME/etc/apps`. The application is a Splunk .spl file, which can be renamed to .tgz and decompressed.

Released versions are available on GitHub: [`TA-sendtest`](https://github.com/triddell/TA-sendtest/releases)

Since `TA-sendtest` exists to help test and showcase [`TA-sendcef`](https://github.com/triddell/TA-sendcef) and [`TA-sendraw`](https://github.com/triddell/TA-sendraw), one or both of these applications should also be installed.

### Splunk Indexes

The following indexes are created for testing purposes:
* ceftarget
* rawtarget

### Splunk Inputs

The following table outlines the Splunk network inputs that `TA-sendtest` creates:

| Protocol | Port    | Index           | Sourcetype   | 
| -------- | ------- | --------------  | ------------ |
| TCP      | 10520   | rawtarget       | splunkd      |
| UDP      | 10521   | rawtarget       | splunkd      |
| TCP      | 10522   | ceftarget       | syslog       |
| UDP      | 10523   | ceftarget       | syslog       |

## Configuration

In the `default` directory, two sample configuration files are provided, one for TA-sendcef and one for TA-sendraw. Use the following steps to configure those applications with these sample configuration files:

1. Substitute `<your_server_ip>` in each file with the local IP address of the test Splunk server.
1. Rename the files to remove the `.sample` extension.
1. Copy the configuration file to the `local` directory under the associated application. The `local` directory may need to be creatd.

**Note:** A restart of the Splunk server is _not_ required when modifying `sendcef.conf` or `sendraw.conf`.

## TA-sendcef Testing

Run the following Splunk search over the past 24 hours.

```
index=_internal sourcetype=splunkd ERROR 
| convert timeformat="%m-%d-%Y %H:%M:%S.%3N" ctime(_time) AS cef_time 
| eval cef_host = host, 
    cef_device_vendor = "Vendor", 
    cef_device_product = "Product", 
    cef_device_version = "Version", 
    cef_event_class = "Class", 
    cef_name = "Name", 
    cef_severity = "10", 
    cef_extension = "message, source" 
| sendcef ceftarget:tcp
```

Now, verify that the CEF-formatted events are in the `ceftarget` index with the following search:

```
index=ceftarget
```

## TA-sendraw Testing

Run the following Splunk search over the past 24 hours.

```
index=_internal sourcetype=splunkd ERROR 
| sendraw rawtarget:tcp
```

Now, verify that the raw events are in the `rawtarget` index with the following search:

```
index=rawtarget
```

## Next Steps
* Modify the above searches to use `ceftarget:udp` and `rawtarget:udp`.
* Change the base search to use custom, non-internal Splunk data.
* Modify the CEF header and CEF extention fields to better understand the command behavior.