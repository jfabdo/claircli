[![Build Status](https://travis-ci.com/joelee2012/claircli.svg?branch=master)](https://travis-ci.com/joelee2012/claircli)
[![Coverage Status](https://coveralls.io/repos/github/joelee2012/claircli/badge.svg?branch=master)](https://coveralls.io/github/joelee2012/claircli?branch=master)
# claircli
## claircli is a command line tool to interact with [Quay Clair](https://github.com/quay/clair), which has following functionalities:
- analyze docker images in local host
- analyze docker images in remote host
- analyze docker images in secure/insecure registry
- support threshold/whitelist for vulnerabilities
- report to HTML/JSON, the html report is based on [template](https://github.com/jgsqware/clairctl/blob/master/clair/templates/analysis-template.html)

# Installation

```bash
pip install claircli
```

# Commands

```
claircli -h
usage: claircli [-h] [-c CLAIR] [-f {html,json}] [-T THRESHOLD]
                [-w WHITE_LIST] [-l LOCAL_IP | -r] [-i REGISTRY] [-L LOG_FILE]
                [-d] [-V]
                IMAGE [IMAGE ...]

Command line tool to interact with Quay Clair to analyze docker image in different ways

positional arguments:
  IMAGE                 docker images or regular expression

optional arguments:
  -h, --help            show this help message and exit
  -c CLAIR, --clair CLAIR
                        clair url, default: http://localhost:6060
  -f {html,json}, --formats {html,json}
                        output report file with give format, default: ['html']
  -T THRESHOLD, --threshold THRESHOLD
                        cvd severity threshold, if any servity of
                        vulnerability above of threshold, will return non-
                        zero, default: Unknown, choices are: ['Defcon1',
                        'Critical', 'High', 'Medium', 'Low', 'Negligible',
                        'Unknown']
  -w WHITE_LIST, --white-list WHITE_LIST
                        path to the whitelist file
  -l LOCAL_IP, --local-ip LOCAL_IP
                        ip address of local host
  -r, --regex           if set, repository and tag of images will be treated
                        as regular expression
  -i REGISTRY, --insecure-registry REGISTRY
                        domain of insecure registry
  -L LOG_FILE, --log-file LOG_FILE
                        save log to file
  -d, --debug           print more logs
  -V, --version         show program's version number and exit

Examples:

    # analyze and output report to html
    # clair is running at http://localhost:6060
    claircli example.reg.com/myimage1:latest example.reg.com/myimage2:latest

    # analyze image in insecure registry
    # clair is running at http://localhost:6060
    claircli -i example.reg.com example.reg.com/myimage1:latest

    # analyze and output report to html
    # clair is running at https://example.clair.com:6060
    claircli -c https://example.clair.com:6060 example.reg.com/myimage1:latest

    # analyze and output report to html, json
    claircli -f html -f json example.reg.com/myimage1:latest

    # analyze with threshold and white list
    claircli -t High -w white_list_file.yml example.reg.com/myimage1:latest

    # analyze image on local host
    claircli -l <local ip address> myimage1:latest myimage2:latest

    # analyze image on other host foo
    export DOCKER_HOST=tcp://<ip of foo>:<port of docker listen>
    claircli -l <local ip address> myimage1:latest

    # analyze with regular expression, following will match
    # example.reg.com/myimage1:latest
    # and example.reg.com/myimage2:latest
    claircli -r example.reg.com/myimage:latest

    # analyze with regular expression, following will match
    # example.reg.com/myimage1:latest only
    claircli -r example.reg.com/^myimage1$:^latest$

```

## Optional whitelist yaml file

This is an example yaml file. You can have an empty file or a mix with only `common` or `<distribution>`.

```yaml
common:
  CVE-2017-6055: XML
  CVE-2017-5586: OpenText
ubuntu:
  CVE-2017-5230: XSX
  CVE-2017-5586: OpenText
alpine:
  CVE-2017-3261: SE
```