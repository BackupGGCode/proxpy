#ProxPy
ProxPy is a highly customizable HTTP/HTTPS proxy, written in Python. It is very handy for web penetration testers and for developers interested in testing their web applications.

ProxPy works as a "man-in-the-middle" between the browser and the target application. It has been developed with the purpose to be easily customizable. At this aim, users can write plug-in with minimal effort. Plug-ins are written in Python, and can modify HTTP/HTTPS requests and response on-the-fly.

Please note that ProxPy is currently under heavy development, so the plug-ins interface may change in the near future.

# A sample plug-in #

Consider this simple ProxyPy plug-in:
```
def proxy_mangle_request(req):
    req.setHeader("User-Agent", "ProxPy Agent")
    return req

def proxy_mangle_response(res):
    v = res.getHeader("Content-Type")
    if len(v) > 0 and "text/html" in v[0]:
        res.body = res.body.replace("Google", "elgooG")
    return res
```
If present, the `proxy_mangle_request` and `proxy_mangle_response` methods are invoked on each HTTP request and response, respectively. In this example, the plug-in performs the following operations:
  * For each HTTP request, the value of the `User-Agent` HTTP header is set to "`ProxPy Agent`"
  * For each HTTP response, any occurrence of the "`Google`" substring is replaced with "`elgooG`"
Obviously real-world plug-ins are typically more complex than this.

# Usage #

To test the plug-in described in the previous section, run ProxPy with a command-line similar to the following one:
```
$ ./proxpy.py -x plugins/changeagent.py 
[*] <b73986c0> Server 0.0.0.0 listening on port 8080
```
Then, the browser should be configured to connect through ProxPy on TCP port `8080`. All available command-line options are shown invoking ProxPy with the "`-h`" switch.
