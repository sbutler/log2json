# log2json
Convert Apache style logs to JSON format. Handy if you want to pipe them
to `jq` for further processing.

# Requirements
Uses a set of modules that should be available on any modern system.

* DateTime
* DateTime::Format::Strptime
* DateTime::TimeZone
* Getopt::Long
* JSON
* Pod::Usage
* URI

# Options

## --pretty, -p
Pretty print the output. This will add newlines and indenting to make it easier
to read.

## --input-date-format, --idate-format, --idf
Input date format string, using the specifiers of `strptime`.

**Default:** `%d/%b/%Y:%H:%M:%S %z`

## --input-date-timezone, --idate-tz, --idz
Input date timezone name, using the database from `DateTime::TimeZone`.
Date fields will use this as the default timezone, unless one is specified
in the `input-date-format`.

**Default:** `local`

## --output-date-format, --odate-format, --odf
Output date format string, using the specifiers of `strftime`.

**Default:** `%FT%TZ`

## --output-date-timezone, --odate-tz, --odz

Output date timezone name, using the database from `DateTime::TimeZone`.
Date fields will be converted to this timezone before formatting.

**Default:** `UTC`

## --log-format, --format, --fmt

Input log format. This understands the common choices of `common`,
`vhost_common`, `combined`, and `vhost_combined`. You can also specify
your own regular expression here, which will provide all the named capture
groups in the JSON object. These named groups are processed specially:

### status, bytes, time, keepalive, port, pid, I, O, S
Will be converted to an integer, unless it is "-" in which case it will be `null`.

### request

Will be provided as `request`, but an attempt will be made to parse it as the
Apache format `%m %U%q %H`. These fields will be provided in `oRequest` as
`method`, `uri`, and `protocol`.

### oRequest.uri, referer

These fields will be attempted to parse and its result provided as
`oRequest.oURI|oReferer` with the various parts of the URI provided as
properties. If there is a query string, then it will also be parsed and
provided as `oQuery`.
