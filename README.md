# Parse varnish logs
This script will parse NCSA style logs. It calculates 80th, 90th, and 95th percentile information along with other things, such as longest requests, requests per second, cache-hit ratio, etc.

This script is the ability to only search back N seconds in a long, preventing unnecessary calculations as well as providing you statistics that serve as point-in-time references.

While this script is specifically for parsing varnish logs, it can easily be ported to any NCSA style log with very little work.
### Using the script
The script is meant to be invoked with a time period in mind (30 seconds, 60 seconds, etc). This provides feedback that is easily graphed. For example, you may want to keep track of periods when your cache-hit ratio drops so you can investigate what type of traffic is causing it.

The output is easily parsable YAML or JSON. 
## Example
```
GET:
    max: 8.212969065
    mean: 0.21271371246875
    min: 0.000153406
    number_of_requests: 160
    percentile_85: 0.06508030880000001
    percentile_90: 0.2906837220999999
    percentile_95: 1.004998481499997
POST:
    max: 1.25316596
    mean: 0.3580077487333334
    min: 0.002954347
    number_of_requests: 15
    percentile_85: 0.4557036874
    percentile_90: 0.8685553071999997
    percentile_95: 1.1619045253999998
all:
    max: 8.212969065
    mean: 0.22516748700571432
    min: 0.000153406
    number_of_requests: 175
    percentile_85: 0.13012456880000006
    percentile_90: 0.35789580339999993
    percentile_95: 1.1364880558999992
request_statistics:
    get_cache_hit_ratio: 0.5681818181818182
    logs:
        processed: 176
        skipped: 0
        total: 176
    longest_request: 10.xx.xx.xx - [26/Jan/2014:04:56:29 +0000] "GET http://mywebsite.com HTTP/1.1" 200 24347 8.212969065 pass
    requests_per_second: 0.17045454545454544
```
By default the output is YAML. Json can also be specified with the ```-o json``` option. You can see a full list of options by running with ```-h```.
```
usage: varnish_request_statistics [-h] -s SECONDS [-o OUTPUT] [-f FILENAME]

Look back N seconds in a log and print out useful statistics. By default the
output will be YAML.

optional arguments:
  -h, --help   show this help message and exit
  -s SECONDS   How many seconds to go back into the log file
  -o OUTPUT    Output format. Supported types are yaml and json.
  -f FILENAME  File to perform parsing operations. Default:
               /var/log/varnish/varnishncsa.log
```
## Setup/installation
The script doesn't require any strange libraries, but you do need to specify the log format within the script.

You can find the log format near the top of the script. The columns must match what you have in your varnishncsa settings:
```
log_column = {
  'date': 3,
  'method': 5,
  'response': 8,
  'response_time': 10,
  'cache': 11,
}
```
