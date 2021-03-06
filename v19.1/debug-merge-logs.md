---
title: Merge Debug Logs for All Nodes
summary: Learn the command for merging the collected debug logs from all nodes in your cluster.
toc: true
---

The `debug merge-logs` [command](cockroach-commands.html) merges log files from multiple nodes into a single time-ordered stream of messages with an added per-message prefix to indicate the corresponding node. You can use it in conjunction with logs collected using the [`debug zip`](https://www.cockroachlabs.com/docs/stable/debug-zip.html) command to aid in debugging.

{{site.data.alerts.callout_danger}}
The file produced by `cockroach debug merge-log` can contain highly sensitive, unanonymized information, such as usernames, passwords, and possibly your table's data. You should share this data only with Cockroach Labs developers and only after determining the most secure method of delivery.
{{site.data.alerts.end}}

## Subcommands

While the `cockroach debug` command has a few subcommands, users are expected to only use the [`encryption-active-key`](debug-encryption-active-key.html), [`zip`](debug-zip.html), and `debug-merge` subcommands.

`debug`'s other subcommands are useful only to CockroachDB's developers and contributors.

## Synopsis

~~~ shell
$ cockroach debug merge-logs [log file directory] [flags]
~~~

## Flags

Use the following flags to filter the `debug merge-logs` results for a specified regular expression or time range.

Flag | Description
-----|-----------
`--filter` | Limit the results to the specified regular expression
`--from` | Start time for the time range filter.
`--to` | End time for the time range filter.

## Example

Generate a debug zip file:

{% include copy-clipboard.html %}
~~~ shell
$ cockroach debug zip ./cockroach-data/logs/debug.zip --insecure
~~~

Unzip the file:

{% include copy-clipboard.html %}
~~~ shell
$ unzip ./cockroach-data/logs/debug.zip
~~~

Merge the logs in the debug folder:

{% include copy-clipboard.html %}
~~~ shell
$ cockroach debug merge-logs debug/nodes/*/logs/*
~~~

Alternatively, filter the merged logs for a specified time range:

{% include copy-clipboard.html %}
~~~ shell
$ cockroach debug merge-logs debug/nodes/*/logs/* --from= "18:36:28.208553" --to= "18:36:29.232864"
~~~

You can also filter the merged logs for a regular expression:

{% include copy-clipboard.html %}
~~~ shell
./cockroach debug merge-logs debug/nodes/*/logs/* --filter="RUNNING IN INSECURE MODE"
~~~

## See also

- [File an Issue](file-an-issue.html)
- [Other Cockroach Commands](cockroach-commands.html)
- [Troubleshooting Overview](troubleshooting-overview.html)
