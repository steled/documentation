# ABP Filter

- below is a list of my currently used ABP filters

``` { .sh }
||popads.media^
||dywolfer.de/*
*c1527284015933.js
||scr.wfcdn.de/1/*/*.jpg
||goldesel.sx/img/*.gif
winfuture.de##.OUTBRAIN.nodigidip
finanzen.net###news-contentads
winfuture.de##.acceptable_ad
winfuture.de###frnAdSkyPos
finanztrends.info##.yes_scrollbox
winfuture.de##.OUTBRAIN.ob-shrink
computerbase.de##.adbox-wrapper.adbox-wrapper--rectangle
computerbase.de##.adbox.adbox-skyscraper.ax-skyscraper
computerbase.de##.adbox.adbox-banner.ax-banner
heise.de##.teaser_adliste.akwa-ad-container.akwa-ad-container__microsite-list.hoteaser
heise.de##.us_ad.akwa-ad-container.akwa-ad-container__us-ad
heise.de##.teaser_adliste.akwa-ad-container.akwa-ad-container__microsite-list.developerteaser
heise.de##.adbottom.akwa-ad-container.akwa-ad-container__ad-bottom
boersennews.de##div[class*="pub_ads"]+div
boersennews.de##.col-md-4
finanzen.net##img[referrerpolicy*="unsafe-url"]
goldesel.sx#?#:-abp-has(> div > a > img)
gmx.net##nav-app-dialog[class*=dialog-app__blocker]
2conv.com##.simple-modal_opened
@@||securepubads.g.doubleclick.net/gpt/pubads_impl_2021120601.js
@@||securepubads.g.doubleclick.net/pagead/ppub_config?ippd=www.heise.de
@@||eu-tlp01.kameleoon.eu/visit.gif?lp=3&spt=1641308686395&p=c2l0ZUNvZGU9eXhzdTV1ZmQybSZ2aXNpdG9yQ29kZT00MzBieDRiM2gwamgwcTA2JnN0YXJ0T2ZWaXNpdD1mYWxzZSZzY3JpcHRWZXJzaW9uPTIwMTkwMTE1Jm5vbmNlPTI1QjVENTI5Qzc4OTZCODMmZXZlbnRUeXBlPXBhZ2UmdGltZT0xNjQxNzMyMjg5MzA3JmhyZWY9aHR0cHMlM0ElMkYlMkZ3d3cuaGVpc2UuZGUlMkZuZXdzJTJGTmV1c3RhcnQtdm9uLWF1dG9tYXRpc2NoZXItS2VubnplaWNoZW5lcmZhc3N1bmctaW4tQnJhbmRlbmJ1cmctb2ZmZW4tNjMyMTA2OS5odG1sJnRpdGxlPU5ldXN0YXJ0JTIwdm9uJTIwYXV0b21hdGlzY2hlciUyMEtlbm56ZWljaGVuZXJmYXNzdW5nJTIwaW4lMjBCcmFuZGVuYnVyZyUyMG9mZmVuJTIwJTdDJTIwaGVpc2UlMjBvbmxpbmUma2V5UGFnZXM9JTVCbnVsbCU1RCZyZWZlcnJlcnM9JTVCbnVsbCU1RA%3D%3D
@@||eu-tlp01.kameleoon.eu/visit.gif?lp=3&spt=1641308686395&p=c2l0ZUNvZGU9eXhzdTV1ZmQybSZ2aXNpdG9yQ29kZT00MzBieDRiM2gwamgwcTA2JnN0YXJ0T2ZWaXNpdD1mYWxzZSZzY3JpcHRWZXJzaW9uPTIwMTkwMTE1Jm5vbmNlPTQ0NDJGNzExNTQzMUU4NEImZXZlbnRUeXBlPXN0YXRpY0RhdGEmdGltZT0xNjQxNzMyMjg5MzE1JnRpbWVTaW5jZVByZXZpb3VzVmlzaXQ9NjQyMjg5MDkwNSZsYW5kaW5nUGFnZUhyZWY9aHR0cHMlM0ElMkYlMkZ3d3cuaGVpc2UuZGUlMkZuZXdzJTJGTmV1c3RhcnQtdm9uLWF1dG9tYXRpc2NoZXItS2VubnplaWNoZW5lcmZhc3N1bmctaW4tQnJhbmRlbmJ1cmctb2ZmZW4tNjMyMTA2OS5odG1sJTNGd3RfbWMlM0Ryc3MucmVkLmhvLmhvLnJkZi5iZWl0cmFnLmJlaXRyYWcmbGFuZGluZ1BhZ2VUaXRsZT1OZXVzdGFydCUyMHZvbiUyMGF1dG9tYXRpc2NoZXIlMjBLZW5uemVpY2hlbmVyZmFzc3VuZyUyMGluJTIwQnJhbmRlbmJ1cmclMjBvZmZlbiUyMCU3QyUyMGhlaXNlJTIwb25saW5lJmxhbmRpbmdQYWdlcz0lNUJudWxsJTVEJmZpcnN0UmVmZXJyZXJIcmVmPW51bGwmZmlyc3RSZWZlcnJlcnM9JTVCbnVsbCU1RCZsYW5ndWFnZT1udWxsJmJyb3dzZXI9MCZicm93c2VyVmVyc2lvbj05NyZtb2JpbGVCcm93c2VyPWZhbHNlJm9zPTAmd2luZG93V2lkdGg9MTE2NyZ3aW5kb3dIZWlnaHQ9NjA3JnNjcmVlbldpZHRoPTM0NDAmc2NyZWVuSGVpZ2h0PTE0NDAmamF2YUVuYWJsZWQ9ZmFsc2UmdGltZVpvbmVJZD1FdXJvcGUlMkZCZXJsaW4mbG9jYWxlTGFuZ3VhZ2VUYWc9ZGUtREUmZGV2aWNlVHlwZT1ERVNLVE9QJmJyb3dzZXJOYW1lPUNocm9tZSZvc05hbWU9V2luZG93cyZ0aW1lWm9uZUdyb3Vwcz0lNUJudWxsJTVEJnZpc2l0TnVtYmVyPTE4
@@||eu-tlp01.kameleoon.eu/visit.gif?lp=3&spt=1641308686395&p=c2l0ZUNvZGU9eXhzdTV1ZmQybSZ2aXNpdG9yQ29kZT00MzBieDRiM2gwamgwcTA2JnN0YXJ0T2ZWaXNpdD1mYWxzZSZzY3JpcHRWZXJzaW9uPTIwMTkwMTE1Jm5vbmNlPTQwNUYxNUVCRTI5REIwMkMmZXZlbnRUeXBlPWFjdGl2aXR5JnRpbWU9MTY0MTczMjI4OTMxOSZhY3RpdmU9dHJ1ZSZudW1iZXJDbGlja3M9MCZ0YWJDb3VudD0w
heise.de##.recommendations wide
linuxize.com###ez-cookie-dialog-wrapper
linuxize.com##.ez_banner.pre-render-cmp.ez-main-cmp-wrapper.ez-twentytwentytwo
computerbase.de/js/main.*.js
cp.winfuture.de/chunks/chunk-cmp-consentmanager.*.js
cdn.consentmanager.net/delivery/js/cmp_de.min.js
winfuture.de###taboola-below-article-thumbnails-desktop
https://a.delivery.consentmanager.net/delivery/cmp.php?&cdid=91c2d8acd39c&h=https%3A%2F%2Fwinfuture.de
```