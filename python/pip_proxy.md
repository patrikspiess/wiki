

# PIP certificate error when a proxy is used

## Problem

```bash
user@machine:~/$ pip install requests

Could not fetch URL https://pypi.org/simple/pip/: There was a problem confirming the ssl certificate: HTTPSConnectionPool(host='pypi.org', port=443): Max retries exceeded with url: /simple/pip/ (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)'))) - skipping
```

The interesting part is the message `certificate verify failed: unable to get local issuer certificate`.

This could be when a proxy does ssl interception and the intermediate certificate is not trusted.

The same happens if you use poetry:

```bash
user@machine:~/$ poetry add requests

HTTPSConnectionPool(host='pypi.org', port=443): Max retries exceeded with url: /simple/requests/ (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)')))
```

## Solution

Add the following configuration to pip.ini:

```ini
[global]
trusted-host = pypi.python.org
               pypi.org
               files.pythonhosted.org
```

pip.ini should be located at ~/.config/pip/pip.conf. Maybe you have to create the directory and file first.
