# BOSH Logs NATS Message Parser

The bosh-director-logs-parser parses bosh debug log files and extracts
the NATS messages from it. It visualizes metrics about sent NATS messages.

## Log Parsing

From the NATS message log lines the following attributes are extracted:
* timestamp
* method
* subject

It calculates the following metrics from it:
* message duration (time span between message SENT and RECEIVED log)
* total messages sent
* total dropped messages
* open messages per method (messages which haven't received a response yet)

Those metrics are stored in `messages.csv` and `dropped_messages.csv`.

## Visualization

The `csv` files are visualized with [Plotly R](https://plot.ly/r/) which
generates an `html` page with four diagrams:

* duration in ms per message
* total number of sent messages over time
* number of open messages over time per method type
* total number of dropped messages over time

## Requirements

* Ruby installed

* R installed
    * For mac execute `brew install r`
    * For other operating systems, follow the instructions on the [R project website](https://cran.r-project.org/)

[RStudio](https://www.rstudio.com) is a recommended for editing R-files but it is not needed to execute this script.

## Executing

To analyze a debug log file run the ruby script as follows:
`./log_parser.rb [options] <absolute path to debug log file>`

Available options are:
`--filter FILTER    Provide a regex to filter out NATS methods, e.g. 'ping|get_state'`

It generates a self-contained `html` file which can be shared.
The file has the name of the debug log + `.html` and is stored next to it in the same directory.
