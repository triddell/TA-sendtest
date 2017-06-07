# TA-sendtest

TA-sendtest provides Splunk configurations to faciliate testing [`TA-sendcef`](https://github.com/triddell/TA-sendcef) and [`TA-sendraw`](https://github.com/triddell/TA-sendraw) on a local Splunk instance.

## Installation

Use normal Splunk installation procedures to install the TA. This includes installation through the Splunk UI or "unzipping" and copying to `$SPLUNK_HOME/etc/apps`. The application is a Splunk .spl file, which can be renamed to .tgz and decompressed.

Released versions are available on GitHub: [`TA-sendtest`](https://github.com/triddell/TA-sendtest/releases)

Since `TA-sendtest` exists to help test and showcase [`TA-sendcef`](https://github.com/triddell/TA-sendcef) and [`TA-sendraw`](https://github.com/triddell/TA-sendraw), one or both of these applications should also be installed.

## Configuration