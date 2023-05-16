

# Poetry certificate error when a proxy is used

## Problem

After you created a poetry project you're not able to install packages due to a certificate error:

```bash
user@machine:~$ poetry add requests

HTTPSConnectionPool(host='pypi.org', port=443): Max retries exceeded with url: /simple/requests/ (Caused by SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:997)')))
```

The interesting part is the message `certificate verify failed: unable to get local issuer certificate`.

This could be when a proxy does ssl interception and the intermediate certificate is not trusted.


## Solution

Add the following configuration to poetry:

```ini
poetry config repositories.files https://files.pythonhosted.org
poetry config certificates.files.cert false
poetry config certificates.PyPI.cert false
```

To revoke these settings execute:

```
poetry config --unset repositories.files
poetry config --unset certificates.files.cert
poetry config --unset certificates.PyPI.cert
```