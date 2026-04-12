# Lab5_Cloud_Attacks_and_Network_Techniques

**Student:** <Ditt namn>  
**Course:** GDT3CR – Ethical Hacking  
**Lab:** 5  

---

## 5.1.3 Cloud Scanning

### IP Range Calculation

To estimate the number of possible IPv4 addresses in a cloud region, the provided script was used:

```bash
python3 countips.py oci_eu.txt
```

Result:  
`38400`


### Network Scan

A scan was performed on a private RFC1918 /16 network using masscan targeting TLS-related ports:

```bash
sudo masscan 10.0.0.0/16 -p443,3389 --rate 10000
```

Output:
```bash
Starting masscan...
Initiating SYN Stealth Scan
Scanning 65536 hosts [2 ports/host]
```

## 5.1.4 Cloud Bucket Discovery
### Bucket Discovery Test

Tested bucket URL:
```bash
https://axgvurke2p6j.compat.objectstorage.eu-stockholm-1.oraclecloud.com/lab2-bucket
```

Command used:
```bash
curl -i https://axgvurke2p6j.compat.objectstorage.eu-stockholm-1.oraclecloud.com/lab2-bucket
```

Response:
```bash
HTTP/1.1 404 Not Found
Code: NoSuchBucket
```

### Non-existing Bucket Test
A second test was performed with a non-existing bucket:  
`lab2-bucket123`

Command:
```bash
curl -i https://axgvurke2p6j.compat.objectstorage.eu-stockholm-1.oraclecloud.com/lab2-bucket123
```

Result:
```bash
HTTP/1.1 404 Not Found
Code: NoSuchBucket
```

### Python Bucket Scanner (POC)
A simple Python script was created to automate bucket checks:  
`python3 bucket_scan.py`

Output:
```bash
lab2-bucket → 404
lab2-bucket123 → 404
```

### Questions
#### 1. Can you find a bucket discovery tool for OCI?

There are currently few dedicated bucket discovery tools for Oracle Cloud Infrastructure (OCI).
Most existing tools are focused on AWS S3.

Therefore, a custom Python script can be used to test bucket existence by sending HTTP requests to the OCI object storage endpoint.

#### 2. Is it possible to distinguish between a private bucket and a non-existing bucket?

No.

Both private and non-existing buckets return the same response:
```bash
HTTP/1.1 404 Not Found
Code: NoSuchBucket
```

This is a security feature that prevents attackers from enumerating valid bucket names.

## Conclusion

This lab demonstrated practical techniques for cloud reconnaissance and security testing.

- Large IP ranges can be efficiently scanned using tools like `masscan`
- Cloud storage endpoints can be tested using simple HTTP requests
- Oracle Cloud Infrastructure does not reveal whether a bucket exists or is private

These behaviors highlight important security mechanisms in modern cloud environments.