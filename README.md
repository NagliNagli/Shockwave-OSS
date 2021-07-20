<p align="center">
<h1> BountyTricks </h1>
</p>

**Sharing Bug Bounty tips and tricks with the community including but not limited to automation, one liners and useful thoughts** 


**Cyllabus** 

## :guardsman:  Misc

[Regex Validator](https://www.debuggex.com/)

[Homograph Generator](https://www.irongeek.com/homoglyph-attack-generator.php)

[Shodan-Scripts](https://github.com/random-robbie/My-Shodan-Scripts/tree/1b01bceecc9be0b74b202f445874920eee48bba5)

[HTTP Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

[MIME Types](https://www.iana.org/assignments/media-types/media-types.xhtml)

[Reverse-Proxies](https://github.com/GrrrDog/weird_proxies)

[Writeups](https://github.com/ngalongc/bug-bounty-reference)

[HTTP Request Smuggling](https://github.com/defparam/smuggler)

- [x] Github local recon - usage: gitsecrets “word” | gf pattern

```
gitsecrets(){
{ find .git/objects/pack/ -name "*.idx"|while read i;do git show-index < "$i"|awk '{print $2}';done;find .git/objects/ -type f|grep -v '/pack/'|awk -F'/' '{print $(NF-1)$NF}'; }|while read o;do git cat-file -p $o;done|grep -E "$1"
}
```

- [x] ffuf on many files

```
ffuf -u URL/FUZZ -w allipstoffuf:URL -w ~/.config/wordlists/envpath:FUZZ -maxtime 300 -t 500 -c -v
```



### :guardsman: Private Nuclei templates

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

### Root Domains
- [x] Google Dorks:
```
Root Domains - "org" subsidiaries
intext: credit company
```

- [x] Amass
```
1. Get company's ASN numbers - amass intel -org DoD
2. Turn ASN numbers into CIDR - whois -h whois.radb.net -- "-i origin $asn" | grep -Eo "([0-9.]+){4}/[0-9]+" | sort -u >> $recondir/cidr
3. Get TLDS from ASN - amass intel -asn $asn
4. Get TLDS from whois data - amass intel -whois -d TLD (facebook.com)
5. Get TLDS from CIDR - amass intel -cidr xxxxxx/23
```

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

* [Java RCE via POST request "month" parameter](https://hackerone.com/reports/987065) - The month parameter was vulnerable to code injection with the following payload: 1${T(java.lang.System).getenv()}

```
Takeaway: When Fuzzing java application to try and insert code injection queries like ${T(java.lang.System).getenv()}
```

* [SSO takeover while adding trailing space](https://hackerone.com/reports/976603) - Adding trailing space to the organisation name would trim it whenever user tried to authenticate

```
Takeaway: When supplying org name check what is the behaviour with adding " " (space) on it's name
```

* [Host Header Cache Poisoning to DOS via appending port number](https://hackerone.com/reports/1096609)

```
Takeaway: Tampering with the host header with situations who involve caching, can append port to the host to cause DOS
```

* [Firebase API link shortening key exposed in JS file](https://hackerone.com/reports/1066410)

```
Takeaway: Go through the "main.slug.js" files and look for API Keys, this one looks like the google maps one (AI....)
```

* [Open S3 bucket discloses all uploaded images](https://hackerone.com/reports/905641)

```
Takeaway: Look for websites who has bucket like https://s3.amazonaws.com/BUCKETNAME and try to run aws s3 ls BUCKETNAME
```

* [Reset any password due to no rate limit on 6 digit OTP](https://hackerone.com/reports/703972)

```
Takeaway: Check each step of reset password phase who might not be protected with rate limiting, this could even be a third step after clicking an email, allowing to skip phase 2.
```

* [Adming password was exposed on javascript source file](https://hackerone.com/reports/344566)

```
Takeaway: on Admin / custom made login panels check the source code to determine if there are some leaks including password.
```

* [SSRF through image parameter](https://hackerone.com/reports/826097)

* [SQLI via "features" get parameter - WAF bypass](https://hackerone.com/reports/1039383)

```
Takeaway: %27||/**/(case%20when(/*%c3*/length/*%c3*/(user)=5)then/**/(1)else(1/0)end)||%27
```

* [Open redirect on OAuth flow](https://hackerone.com/reports/972601)

```
Takeaway: Change the scope parameter to arbitrary file and see if the redirect_url will redirect to external domain
```

# Disclaimer

Some of the one liners or data presented might be taken from other repos and was tampered by me, I only share here stuff I use regulary or encountered in the last year, if you find here anything that was originally crafted by you lemme know and I'll credit you.
