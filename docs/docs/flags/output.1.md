---
title: TRACEE-OUTPUT
section: 1
header: Tracee Output Flag Manual
date: 2023/10
...

## NAME

tracee **\-\-output** - Control how and where output is printed

## SYNOPSIS

tracee **\-\-output** <format[:file,...]\> | gotemplate=template[:file,...] | forward:url | webhook:url | option:{stack-addresses,exec-env,relative-time,exec-hash,parse-arguments,parse-arguments-fds,sort-events} ...


## DESCRIPTION

The **\-\-output** flag allows you to control how and where the output is printed.

Format options:

- **table[:/path/to/file,...]**: Output events in table format. The default path to the file is stdout. Multiple file paths can be specified, separated by commas.

- **table-verbose[:/path/to/file,...]**: Output events in table format with extra fields per event. The default path to the file is stdout. Multiple file paths can be specified, separated by commas.

- **json[:/path/to/file,...]**: Output events in JSON format. The default path to the file is stdout. Multiple file paths can be specified, separated by commas.

- **gob[:/path/to/file,...]**: Output events in gob format. The default path to the file is stdout. Multiple file paths can be specified, separated by commas.

- **gotemplate=/path/to/template[:/path/to/file,...]**: Output events formatted using a given Go template file. The default path to the file is stdout. Multiple file paths can be specified, separated by commas.

- **none**: Ignore the stream of events output. This is usually used with the **\-\-capture** flag.

Fluent Forward options:

- **forward:url**: Send events in JSON format using the Forward protocol to a Fluent receiver. Specify the URL of the Fluent receiver.

Webhook options:

- **webhook:url**: Send events in JSON format to the specified webhook URL.

Other options:

- **option:{stack-addresses,exec-env,relative-time,exec-hash,parse-arguments,sort-events}**: Augment output according to the given options. The default is none. Multiple options can be specified, separated by commas.

  - **stack-addresses**: Include stack memory addresses for each event.
  - **exec-env**: When tracing execve/execveat, show the environment variables that were used for execution.
  - **relative-time**: Use relative timestamp instead of wall timestamp for events.
  - **exec-hash**: When tracing sched_process_exec, show the file hash (sha256) and ctime.
  - **parse-arguments**: Do not show raw machine-readable values for event arguments. Instead, parse them into human-readable strings.
  - **parse-arguments-fds**: Enable parse-arguments and enrich file descriptors (fds) with their file path translation. This can cause pipeline slowdowns.
  - **sort-events**: Enable sorting events before passing them to the output. This may decrease the overall program efficiency.

## EXAMPLES

- To output events as JSON to stdout, use the following flag:

  ```console
  --output json
  ```

- To output events as JSON to `/my/out`, use the following flag:

  ```console
  --output json:/my/out
  ```

- To output events as the provided Go template to stdout, use the following flag:

  ```console
  --output gotemplate=/path/to/my.tmpl
  ```

- To output events in gob format to `/my/out`, use the following flag:

  ```console
  --output gob:/my/out
  ```

- To output events as JSON to stdout and as gob to `/my/out`, use the following flag:

  ```console
  --output json --output gob:/my/out
  ```

- To output events as JSON to both `/my/out` and `/my/out2`, use the following flag:

  ```console
  --output json:/my/out1,/my/out2
  ```

- To ignore events output, use the following flag:

  ```console
  --output none
  ```

- To output events as a table with stack addresses, use the following flag:

  ```console
  --output table --output option:stack-addresses
  ```

- To output events via the Forward protocol to `127.0.0.1` on port `24224` with the tag 'tracee' using TCP, use the following flag:

  ```console
  --output forward:tcp://user:pass@127.0.0.1:24224?tag=tracee
  ```

- To output events to the webhook endpoint `http://webhook:8080`, use the following flag:

  ```console
  --output webhook:http://webhook:8080
  ```

- To output events to the webhook endpoint `http://webhook:8080` with a timeout of 5 seconds, use the following flag:

  ```console
  --output webhook:http://webhook:8080?timeout=5s
  ```
