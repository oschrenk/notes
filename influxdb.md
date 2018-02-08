# InfluxDB

Send data points from file

```
cpu_load_short,host=server02 value=0.67
cpu_load_short,host=server02,region=us-west value=0.55 1422568543702900257
cpu_load_short,direction=in,host=server01,region=us-west value=2.0 1422568543702900257
```

## Line protocol

Syntax

```
<measurement>[,<tag_key>=<tag_value>[,<tag_key>=<tag_value>]] <field_key>=<field_value>[,<field_key>=<field_value>] [<timestamp>]
```

Each line, separated by the newline character `\n`, represents a single point in InfluxDB. Line Protocol is whitespace sensitive.

Element	Optional/Required	Description	Type
* `Measurement`. Required. The measurement name. InfluxDB accepts one measurement per point.	*String*
* `Tag set`.Optional.	All tag key-value pairs for the point.	Tag keys and tag values are both strings.
* `Field set`. Required. Points must have at least one field.	All field key-value pairs for the point.	Field keys are strings. Field values can be floats, integers, strings, or booleans.
* `Timestamp`. Optional. InfluxDB uses the server’s local nanosecond timestamp in UTC if the timestamp is not included with the point.	The timestamp for the data point. InfluxDB accepts one timestamp per point.	Unix nanosecond timestamp. Specify alternative precisions with the HTTP API.
