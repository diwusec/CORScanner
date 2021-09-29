## About corscanner 

corscanner is a python tool designed to discover CORS misconfigurations vulnerabilities of websites. It helps website administrators and penetration testers to check whether the domains/urls they are targeting have insecure CORS policies. 

### Features
* **Fast**. It uses [gevent](https://github.com/gevent/gevent) instead of Python threads for concurrency, which is much faster for network scanning.
* **Comprehensive**. It covers all [the common types of CORS misconfigurations](#misconfiguration-types) we know.
* **Flexible**. It supports various self-define features (e.g. file output), which is helpful for large-scale scanning.
* 🆕 CORScanner supports installation via pip (`pip install corscanner` or `pip install cors`)
* 🆕 CORScanner can be used as a library in your project.



## Installation

- Download this tool
```
git clone https://github.com/diwusec/corscanner.git
```
- Install dependencies
```
sudo pip3 install -r requirements.txt
```
- Install setup.py
```
sudo python3 setup.py install
```

CORScanner depends on the `requests`, `gevent`, `tldextract`, `colorama` and `argparse` python modules.

## Python Version:

* Both Python 2 (**2.7.x**) and Python 3 (**3.7.x**) are supported.

- Example code:
```python
>>> from corscanner.cors_scan import cors_check
>>> ret = cors_check("https://www.instagram.com", None)
>>> ret
{'url': 'https://www.instagram.com', 'type': 'reflect_origin', 'credentials': 'false', 'origin': 'https://evil.com', 'status_code': 200}
```

You can also use corscanner via the `corscanner` or `cors` command: `cors -vu https://www.instagram.com`

## Usage

Short Form    | Long Form     | Description
------------- | ------------- |-------------
-u            | --url         | URL/domain to check it's CORS policy
-d            | --headers     | Add headers to the request
-i            | --input       | URL/domain list file to check their CORS policy
-t            | --threads     | Number of threads to use for CORS scan
-o            | --output      | Save the results to json file
-v            | --verbose     | Enable the verbose mode and display results in realtime
-T            | --timeout     | Set requests timeout (default 10 sec)
-p            | --proxy       | Enable proxy (http or socks5)
-h            | --help        | show the help message and exit

### Examples

* To check CORS misconfigurations of specific domain:

``python cors.py -u example.com``

* To enable more debug info, use -v:

``python cors.py -u example.com -v``

* To save scan results to a JSON file, use -o:

``python cors.py -u example.com -o output_filename``

* To check CORS misconfigurations of specific URL:

``python cors.py -u http://example.com/restapi``

* To check CORS misconfiguration with specific headers:

``python cors.py -u example.com -d "Cookie: test"``

* To check CORS misconfigurations of multiple domains/URLs:

``python cors.py -i top_100_domains.txt -t 100``

* To enable proxy for corscanner, use -p

```python cors.py -u example.com -p http://127.0.0.1:8080```

To use socks5 proxy, install PySocks with `pip install PySocks`

```python cors.py -u example.com -p socks5://127.0.0.1:8080```

* To list all the basic options and switches use -h switch:

```python cors.py -h```

## Misconfiguration types
This tool covers the following misconfiguration types:

Misconfiguration type    | Description
------------------------ | --------------------------
Reflect_any_origin       | Blindly reflect the Origin header value in `Access-Control-Allow-Origin headers` in responses, which means any website can read its secrets by sending cross-orign requests.
Prefix_match             | `wwww.example.com` trusts `example.com.evil.com`, which is an attacker's domain.
Suffix_match             | `wwww.example.com` trusts `evilexample.com`, which could be registered by an attacker.
Not_escape_dot           | `wwww.example.com` trusts `wwwaexample.com`, which could be registered by an attacker.
Substring match          | `wwww.example.com` trusts `example.co`, which could be registered by an attacker.
Trust_null               | `wwww.example.com` trusts `null`, which can be forged by iframe sandbox scripts
HTTPS_trust_HTTP         | Risky trust dependency, a MITM attacker may steal HTTPS site secrets
Trust_any_subdomain      | Risky trust dependency, a subdomain XSS may steal its secrets
Custom_third_parties     | Custom unsafe third parties origins like `github.io`, see more in [origins.json](./origins.json) file. Thanks [@phackt](https://github.com/phackt)!
Special_characters_bypass| Exploiting browsers’ handling of special characters. Most can only work in Safari except `_`, which can also work in Chrome and Firefox. See more in [Advanced CORS Exploitation Techniques](https://www.corben.io/advanced-cors-techniques/). Thanks [@Malayke](https://github.com/Malayke).

## License

CORScanner is licensed under the MIT license. take a look at the [LICENSE](./LICENSE) for more information.

forked from chenjj/CORScanner
