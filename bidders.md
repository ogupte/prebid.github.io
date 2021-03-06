---
layout: default
title: Bidders
description: Documentation on bidders
pid: 2
isHome: false
---

<div class="bs-docs-section" markdown="1">
#Common

<a name="common-bidresponse"></a>

###bidResponse
These parameters in the bidReponse object are common across all bidders. Some may not be available for certain bidders and they're marked for each bidder:

{: .table .table-bordered .table-striped }
| Name | Type | Description | Example |
| :--- | :---- | :---------- | :------ |
| `cpm` |	Float |	The bid price in the currency's unit amount. For example, 1.2 in USD would be 1 dollar and 20 cents ($1.20). |	1.2 |
| `adTag` | String | The creative's payload in HTML | `<html><body><img src="http://cdn.com/creative.png"></body></html>` |
| `width` |	Integer |	The width of the bid creative in pixels. |	300 |
| `height` |	Integer |	The height of the bid creative in pixels. |	250 |
| `dealId` |	String |	The Deal ID. |	"827372323" |

</div>

<div class="bs-docs-section" markdown="1">
#AppNexus

###bidder code: 
`appnexus`

###bidId

{: .table .table-bordered .table-striped }
| Name | Scope | Description | Example |
| :--- | :---- | :---------- | :------ |
| `tagId` | required | The placement ID from AppNexus. | "234234" |
| `invCode` | optional | The inventory code you set up in a placement. Has to be used together with memberId. | "code234234" |
| `memberId` | optional | Your member ID in AppNexus. Only useful to be used together with invCode. | "123" |

<a href="appnexus-bidresponse"></a>

###bidResponse

{: .table .table-bordered .table-striped }
| Name | Type | Description | Example |
| :--- | :---- | :---------- | :------ |
| `adId` |	String |	The unique identifier of a bid's creative. It's used by the line item's creative. |	"234235_746323" |
| `keyLg` |	String |	A concatenated key showing the ad size and a "low granularity" CPM at $0.50 increments. |	"300x250p1.50" |
| `keyMg` |	String |	A concatenated key showing the ad size and "medium granularity" CPM at $0.10 increments. |	"728x90p1.90" |
| `keyHg` |	String |	A concatenated key showing the ad size and "high granularity" CPM at $0.01 increments. |	"468x60p1.59" |
| `adUrl` |	String |	The creative URL. Mime type is text/html. Can be rendered in an iFrame. |	"http://nym1.b.adnxs.com/ab?e=wqT..." |

[Common bidResponse](#common-bidresponse) supported: `cpm`, `adTag`, `width`, `height`, `dealId`.


###Line item setup
Depending on what granularity of keys you have chosen from `keyLg`, `keyMg`, `keyHg`, set up the corresponding line items:

* `keyLg`: 20 line items per size at $0.50 increment. For example: 
* `keyMg`: 100 line items per size at $0.10 increment. For example: 
* `keyHg`: 1000 line items per size at $0.01 increment. For example: 

###Creative setup
In your adserver, for each size, you can have a 3rd party tag creative with the below content. The value passed into apn_adid will be what pbjs.renderAd() will use to identify the creative. 

{% highlight js %}
<script type="text/JavaScript">
    try{ window.top.pbjs.renderAd(document, '%%PATTERN:apn_adid%%'); } catch(e) {/*ignore*/}
</script>
{% endhighlight %}

</div>

<div class="bs-docs-section" markdown="1">
#Amazon

###bidder code: 
`amazon`

###bidId

{: .table .table-bordered .table-striped }
| Name | Scope | Description | Example |
| :--- | :---- | :---------- | :------ |
| `aid` | required | The site ID for Amazon. | "1234" |

###bidResponse

{: .table .table-bordered .table-striped }
| Name | Type | Description | Example |
| :--- | :---- | :---------- | :------ |
| `keys` | String | The keys for Amazon | `["a3x2p13", "a1x6p11"]` |


</div>

<div class="bs-docs-section" markdown="1">

#Criteo

###bidder code: 
`criteo`

###bidId

{: .table .table-bordered .table-striped }
| Name | Scope | Description | Example |
| :--- | :---- | :---------- | :------ |
| `nid` | required | The nid from Criteo. | "1234" |
| `cookiename` | required | The cookie name for Criteo. | "ckn_pub" |
| `varname` | optional | The default is `crtg_content`. | "crtg_content" |

###bidResponse

{: .table .table-bordered .table-striped }
| Name | Type | Description | Example |
| :--- | :---- | :---------- | :------ |
| `keys` | String | The keys from crtg_content | `["pub728=1", "pub320=1"]` |

</div>

<div class="bs-docs-section" markdown="1">

#OpenX

###bidder code: 
`openx`

###bidId

{: .table .table-bordered .table-striped }
| Name | Scope | Description | Example |
| :--- | :---- | :---------- | :------ |
| `pid` | required | The publisher ID | "534205285" |
| `unitId` | required | the unit ID | "538562284" |
| `cookiename` | required | The cookie name for OpenX. | "ox_pub" |

###bidResponse

{: .table .table-bordered .table-striped }
| Name | Type | Description | Example |
| :--- | :---- | :---------- | :------ |
| `keys` | String | The keys from crtg_content | `["pub728=1", "pub320=1"]` |


</div>

<div class="bs-docs-section" markdown="1">
#Pubmatic

###bidder code: 
`pubmatic`

###bidId

{: .table .table-bordered .table-striped }
| Name | Scope | Description | Example |
| :--- | :---- | :---------- | :------ |
| `pubId` | required | The publisher ID | "32572" |
| `slot` | required | the unit ID | "38519891@300x250" |

###bidResponse

{: .table .table-bordered .table-striped }
| Name | Type | Description | Example |
| :--- | :---- | :---------- | :------ |
| `bidid` | String | The unique identifier of a bid's creative. It's used by the line item's creative. | "38519891@300x250" |
| `cpm` | String | Refer to [Common bidReponse](#common-bidresponse) | |

###Creative Setup

{% highlight js %}
<script type="text/javascript">
if(!window.PubMaticGrouped){ document.write('<script type="text/javascript" src="'+ ( location.protocol === "https:" ? "https:" : "http:" ) +'//ads.pubmatic.com/AdServer/js/gshowad.js"><\/script>');}
</script>
<script type="text/javascript">
PubMaticGrouped.displayCreative("%%PATTERN:bidid%%");
</script>
{% endhighlight %}


</div>

<div class="bs-docs-section" markdown="1">
#Rubicon

###bidder code: 
`rubicon`

###bidId

{: .table .table-bordered .table-striped }
| Name | Scope | Description | Example |
| :--- | :---- | :---------- | :------ |
| `account` | required | The publisher account ID | "4934" |
| `site` | required | The site ID | "@300x250" |
| `zonesize` | required | The concatenation of zone and size | "23948-15" |

###bidResponse

{: .table .table-bordered .table-striped }
| Name | Type | Description | Example |
| :--- | :---- | :---------- | :------ |
| `adId` | String | The unique identifier of a bid's creative. It's used by the line item's creative. |	"234235_746323" |
| `cpm` | String | Refer to [Common bidReponse](#common-bidresponse) | |

###Creative Setup

In your adserver, for each size, you can have a 3rd party tag creative with the below content. The value passed into apn_adid will be what pbjs.renderAd() will use to identify the creative. 

{% highlight js %}
<script type="text/JavaScript">
    try{ window.top.pbjs.renderAd(document, '%%PATTERN:rb_adid%%'); } catch(e) {/*ignore*/}
</script>
{% endhighlight %}


</div>