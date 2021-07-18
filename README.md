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


### 💂‍ H1 Disclosed Reports analysis 

* [ReDoS on GraphQL API endpoint](https://hackerone.com/reports/1000567) - Regex BOMB to shutter the server CPU.

```
Takeaway : FUZZ with certain characters such as \u0000 to try and trigger ReGeX verbose errors
```

* [Ticket Trick](https://hackerone.com/reports/999765) - was able to register with noreply@github.com, system created support tickets by emailing to support@company.com, the email verification landed to his account.

```
Takeaway: If a company won't require email address verification and will automatically generate support tickets, try and sign up with noreply@github.com
```

* [CSRF due to authenticity_token not being validated](https://hackerone.com/reports/994504) - The authenticity_token had a fixed value which wasn't validated, leads to being vulnerable to CSRF -> Org takeover on shopify.

```
Takeaway: whenever authenticity_token is presented on requests validate if the value is being processed in the back-end.
```

* [Application level DOS with crafted payload](https://hackerone.com/reports/993582) - "name" parameter on POST request crashed upon supplying (((((()0))))) as input.

```
Takeaway: try (((((()0))))) when fuzzing post requests.
```

* [IDOR on steam id cookie](https://hackerone.com/reports/990878) - Utilizing a POST request with the victim steamid cookie value performed the action as the victims behalf

```
Takeaway: Swap identifyable cookie values between lateral accounts.
```

* [Bitbucket public repo credential leak](https://hackerone.com/reports/961428)

```
Takeaway: Look through org's public repos for Bitbucket content
```


# Disclaimer

Some of the one liners or data presented might be taken from other repos and was tampered by me, I only share here stuff I use regulary or encountered in the last year, if you find here anything that was originally crafted by you lemme know and I'll credit you.
