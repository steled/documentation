## get dns IDs

``` { .sh }
curl -sX GET "https://api.cloudflare.com/client/v4/zones/<ZONE_ID>/dns_records" -H "Content-Type:application/json" -H "Authorization: Bearer <API_TOKEN>" | jq -r .result
```
