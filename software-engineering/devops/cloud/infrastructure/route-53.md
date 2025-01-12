# Route 53



## Transfer Domain from GoDaddy to Route 53

1. Create new Hosted Zone in Route 53 for the new Domain
2. Go to current DNS Provider (e.g GoDaddy) Domain settings and remove old DNS Nameservers and add all AWS name servers listed for the new created hosted zone
3. &#x20;Go to AWS Certificate Manager and request new certificate for the Domain
4. After requested, click on Certificate and click "Create CNAME Record"

{% embed url="https://www.youtube.com/watch?v=yLlK5avYvKo" %}

## Configure Subdomains

1. Create new Hosted Zone for subdomain
2. Copy nameservers of new subdomain
3. Navigate to Hosted Zone of root domain and create new NS Record. Copy nameservers in the value field

[https://repost.aws/knowledge-center/create-subdomain-route-53](https://repost.aws/knowledge-center/create-subdomain-route-53)
