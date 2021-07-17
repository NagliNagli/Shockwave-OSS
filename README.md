<p align="center">
<h1> BountyTricks </h1>
</p>

**Sharing Bug Bounty tips and tricks with the community including but not limited to automation, one liners and useful thoughts** 


**Cyllabus** 

### Private Nuclei templates

- [x] SSRF nuclei template - Feed endpoints to probe for SSRF interaction automatically, the module tries to fetch simple interaction on the provided input, and later appends common SSRF query params to the original request.

Sample:

```
echo "https://checkout.stripe.com/api/color?image_url=" | nuclei -t ssrf.yaml 
```

![nuclei_ssrf](https://raw.githubusercontent.com/NagliNagli/BountyTricks/main/media/nuclei_ssrf.png)


### Tips & Tricks from the wild

- [x] WAF bypass by changing scheme:
```
http://web.com/?XSSendpoint ===> no WAF
https://web.com/?XSSendpoint ===> WAF implemented
```


### Subdomain Reconnaissance

- [x] CIDR to hostnames
```
prips 144.160.32.0/19 | hakrevdns  -d | httpx -title -status-code -follow-redirects
```
