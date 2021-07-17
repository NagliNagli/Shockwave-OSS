<p align="center">
<h1> BountyTricks </h1>
</p>

**Sharing Bug Bounty tricks and treats with the community including but not limited to automation, one liners and useful thoughts** 


**Cyllabus** 

- [x] SSRF nuclei template - Feed endpoints to probe for SSRF interaction automatically, the module tries to fetch simple interaction on the provided input, and later appends common SSRF query params to the original request.

Sample:

```
echo "https://checkout.stripe.com/api/color?image_url=" | nuclei -t ssrf.yaml 
```

![nuclei_ssrf](https://raw.githubusercontent.com/NagliNagli/BountyTricks/main/media/nuclei_ssrf.png)
