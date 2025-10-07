# Basic requests
## simple GET
```bash
curl https://example.com
```

---

## GET and show response headers + body
```bash
curl -i https://example.com
```

---
## only headers (HEAD request)
```bash
curl -I https://example.com
Save / download
```

---
## write to file

```bash
curl -o page.html https://example.com
```

---
## save with remote filename

```bash
curl -O https://example.com/file.zip
```

---
## resume interrupted download

```bash
curl -C - -O https://example.com/big.iso
```

## Follow redirects

```bash
curl -L https://example.com  # follow 301/302 -> final URL
```

## HTTP methods & payloads
```bash
# POST form data (application/x-www-form-urlencoded)
curl -X POST -d "name=joe&age=30" https://api.example.com/form

# POST JSON (recommended: --json when available, otherwise set header)
curl -X POST -H "Content-Type: application/json" \
     -d '{"user":"joe","pw":"p"}' \
     https://api.example.com/login

# shorthand for JSON (curl 7.82+)
curl --json '{"user":"joe"}' https://api.example.com/login

# PUT with raw file
curl -X PUT --data-binary @local.bin https://example.com/upload

# POST multipart (file upload)
curl -F "file=@/path/photo.jpg" -F "desc=hi" https://example.com/upload
```

## Common headers & auth
```bash
# custom header
curl -H "X-My-Header: 1" https://example.com

# bearer token
curl -H "Authorization: Bearer <TOKEN>" https://api.example.com/secure

# HTTP Basic auth
curl -u username:password https://api.example.com/secret

# use .netrc for credentials (safer than inline)
# create ~/.netrc with: machine api.example.com login user password pass
curl --netrc https://api.example.com

# proxy with auth
curl -x http://proxy.example:3128 --proxy-user user:pw https://example.com
```

## Cookies
```bash
# send cookie
curl -b "name=value" https://example.com

# read cookies from file and save cookies
curl -c cookies.txt -b cookies.txt https://example.com

TLS / HTTPS options
# ignore bad certs (dangerous; use only for testing)
curl -k https://self-signed.badssl.com

# use specific CA bundle
curl --cacert /path/ca.pem https://example.com

# client cert/key (PEM)
curl --cert client.crt --key client.key https://example.com
```

## Debugging & verbosity

```bash
# show verbose info (headers, handshake)
curl -v https://example.com

# get full low-level trace (huge)
curl --trace-ascii trace.txt https://example.com

# show only HTTP response code
curl -s -o /dev/null -w "%{http_code}\n" https://example.com

# print timing info (total time, DNS, connect, etc.)
curl -s -o /dev/null -w "time_total: %{time_total}s\n" https://example.com
# multiple fields:
curl -s -o /dev/null -w "code:%{http_code} time:%{time_total}s\n" https://example.com
```

## Retry / timeouts / speed limits

```bash
# overall timeout (seconds)
curl --max-time 30 https://example.com

# connect timeout
curl --connect-timeout 5 https://example.com

# automatic retries on transient errors
curl --retry 5 --retry-delay 2 --retry-connrefused https://example.com

# limit download speed
curl --limit-rate 100K -O https://example.com/bigfile
```

## Compression, HTTP versions, and raw
```bash
# accept compressed responses (gzip)
curl --compressed https://example.com

# force HTTP/2 (if supported)
curl --http2 https://example.com

# try HTTP/3 (if curl + libs support)
curl --http3 https://example.com

# send raw request body without automatic processing
curl --data-raw $'line1\nline2' https://example.com
```

## UNIX sockets / interface control
```bash
# talk to HTTP-over-UNIX-socket (useful for systemd/docker APIs)
curl --unix-socket /var/run/docker.sock http://localhost/containers/json

# use specific network interface
curl --interface eth0 https://example.com
```

## SOCKS proxy (useful for SSH tunnels / socks5 proxies)

```bash
# socks5 proxy
curl --socks5-hostname 127.0.0.1:1080 https://example.com
# or --socks5 for DNS via local machine; use -hostname variant to resolve via proxy
```

## Output formatting + piping
```bash
# pretty print JSON with jq
curl -s https://api.example.com/data | jq .

# save response headers to file
curl -D headers.txt -o body.bin https://example.com

# show both headers and body on stdout
curl -i https://example.com

# save only header to STDOUT and discard body
curl -s -o /dev/null -D - https://example.com
```

## Useful small recipes
```bash
# check TLS certificate details (connect and show cert)
curl -vI https://example.com 2>&1 | sed -n '/^* Server certificate:/,$p'

# follow redirects but limit to N hops
curl -L --max-redirs 5 https://example.com

# POST form & include response headers in output
curl -i -X POST -F "u=user" -F "p=pass" https://example.com/login

# show effective URL after redirects
curl -s -o /dev/null -w "%{url_effective}\n" -L https://example.com
```

## Quick reference (most used flags)

- `-X` : HTTP method (GET/POST/PUT/etc)
- `-d`, --data-raw, --data-binary : request body
- `-F` : multipart/form-data
- `-H` : add header
- `-u` : basic auth user:pass
- `-I` : HEAD (headers only)
- `-i` : include response headers in output
- `-L` : follow redirects
- `-o` / -O : output file
- `-v` : verbose; --trace-ascii : full trace
- `-s` : silent; -S : show error when silent
- `-k` : insecure (ignore TLS)
- `--max-time` : total timeout
- `--retry` : retry failed requests