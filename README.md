# BountyTricks

**Sharing Bug Bounty tricks and treats with the community: automation, one liners and useful data** 


**Cyllabus** 

- [x] SSRF nuclei template - Feed endpoints with query params to probe for SSRF interaction automatically, the module tries to fetch simple interaction on the provided query params, and later appends common SSRF path's to the original request.

Sample:

```
echo "https://checkout.stripe.com/api/color?image_url=" | nuclei -t ssrf.yaml 
```
