---
layout: default
title: Publisher API
description: API for publishers
pid: 1
isHome: false
---

<div class="bs-docs-section" markdown="1">

#Summary
> Prebid.js helps you send out pre-bid requests asynchronously, while watching out the timeout for ya.

![Prebid Diagram Image]({{ site.github.url }}/assets/images/prebid-diagram.png)

1. **Register bidder tag Ids:** 

	Define a mapping of the bidders’ tag Ids to your ad units.

2. **Ad server waits for bids:** 

	Define the timeout so your ad server would wait for a few hundred milliseconds and the bidders can respond with bids.

3. **Set targeting for bids:** 

	Set bids’ CPM for your ad units’ targeting, then send the impressions to your ad server.

<br> 

###Basic Example
Here’s a basic example for Amazon and AppNexus bidding into a DFP ad unit:

{% highlight js %}
var pbjs = pbjs || {};

// 1. Register bidder tag Ids
pbjs.adUnits = [{
    code: "/1996833/slot-1",
    sizes: [[300, 250], [728, 90]],
    bids: [{
    	bidder: "amazon",
    	bidId: { siteId: "8765" }
    }, {
        bidder: "appnexus",
        bidId: { tagId: "234235" }
    }]
}];

// 2. Ad server wait for bids
PREBID_TIMEOUT = 300;
function initAdserver() {
    (function() {
        // To load GPT Library Async
    })();
    pbjs.initAdserverSet = true;
};
setTimeout(initAdserver, PREBID_TIMEOUT);

// 3. Set targeting for bids
googletag.cmd.push(function() {
    var slot = googletag.defineSlot('/1996833/slot-1', [[300, 250], [728, 90]]);
    if (pbjs.libLoaded) {
        pbjs.setTargetingForGPTAsync(slot, '/1996833/slot-1');
    }
});

{% endhighlight %}

</div>

<div class="bs-docs-section" markdown="1">

#Register bidders


The code below registers the bidder tags for your ad units. Once the prebid.js library loads, it reads the pbjs.adUnits object and sends out bid requests. Check out the complete [reference on bidders](bidders.html).

###Example

{% highlight js %}
var pbjs = pbjs || {};
pbjs.adUnits = [
    {
        code: "/1996833/slot-1",
        sizes: [[300, 250], [728, 90]],
        bids: [{
                    bidder: "amazon",
                    bidId: {
                        siteId: "8765"
                    }
                },{
                    bidder: "appnexus",
                    bidId: {
                        tagId: "234235"
                    }
                }]
        }  
    },{
        code: "/1996833/slot-2",
        sizes: [[468, 60]],
        bids: [{
                    bidder: "amazon",
                    bidId: {
                        siteId: "8765"
                    }
                },{
                    bidder: "appnexus",
                    bidId: {
                        memberId: "343"
                        invCode: "PBJS123"
                    }
                }]
        }
    }
];
{% endhighlight %}

###AdUnit

{: .table .table-bordered .table-striped }
|	Name |	Scope 	|	 Type | Description |
| :----  |:--------| :-------| :----------- |
|	`code` |	required |	string | A unique identifier of an ad unit. This identifier will later be used to set query string targeting on the ad unit. |
| `sizes` |	required |	array |	All the sizes that this ad unit can accept. |
| `bids` |	required |	array |	An array of bid objects. Find the [complete reference here](bidders.html). |

###Bid

{: .table .table-bordered .table-striped }
|	Name |	Scope 	|	 Type | Description |
| :----  |:--------| :-------| :----------- |
| `bidder` |	required |	string |	The bidder code. Find the [complete list here](bidders.html). |
| `bidId` |	required |	object |	The bidder's preferred way of identifying a bid request. Find the [complete reference here](bidders.html). |

</div>

<a href="configure-adserver-targeting"></a>

<div class="bs-docs-section" markdown="1">

#Configure Ad Server Targeting (Optional)

Bidders all have different recommended ad server line item targeting and creative setup. To remove the headache for you, Prebid.js has a default recommended query string targeting setting for every bidder. Check out the [default recommended settings here](bidders.html). You may choose to follow the recommendation and set up your line items and creatives that way. No need to worry about the following.

If you prefer to customize your line item targeting and creative setup, you can overwrite the bidder settings as the below example shows. 

###Example

The below example overwrites the default AppNexus and Amazon query string targeting. 

{% highlight js %}
pbjs.bidderSettings = [{
    bidder: "appnexus",
    adserverTargeting: [
        {
            key: "apn_adid",
            val: function(bidResponse) {
                return bidResponse.adId;
            }
        }, {
            key: "apn_key",
            val: function(bidResponse) {
                return bidResponse.keyHg;
            }
        }
    ]
}, {
    bidder: "amazon",
    adserverTargeting: [
        {
            key: "amz_slots",
            val: function(bidResponse) {
                return bidResponse.amznslots;
            }
        }
    ]
}]
{% endhighlight %}

###Explained
The bidder setting for AppNexus is saying: send 2 pairs of key value string targeting for every AppNexus bid and for every ad unit. The 1st pair would be `apn_adid` => the value of `bidResponse.adId`. The 2nd pair would be `apn_key` => the value of `bidResponse.keyHg`. You can find the documentation of AppNexus's bidResponse object [here](bidders.html#appnexus-bidresponse). Similar case for Amazon.

{: .table .table-bordered .table-striped }
| Function Name | Description |
| :--- | :---- |
| targetingValueHandler (bidResponse) | The function returns a query string targeting value. It is used in pair with the adserverTargeting's `key` param. The key value pair together will be sent for targeting on the ad server's ad unit impression. `bidResponse` is bidder specific and you can find what're available in the [documentation here](bidders.html). |

</div>

<div class="bs-docs-section" markdown="1">

#Load the Prebid.js library

This code pulls down the prebid.js library asynchronously from the appropriate CDN and inserts it into the page.

{% highlight js %}
(function() {
    var pbjsEl = document.createElement("script"); pbjsEl.type = "text/javascript";
    pbjsEl.async = true; var isHttps = 'https:' === document.location.protocol;
    pbjsEl.src = (isHttps ? "https://a248.e.akamai.net/appnexus.download.akamai.com/89298/adnexus-prod/tag" : "http://cdn.adnxs.com/tag") + "/prebid.js";
    var pbjsTargetEl = document.getElementsByTagName("head")[0];
    pbjsTargetEl.insertBefore(pbjsEl, pbjsTargetEl.firstChild);
})();
{% endhighlight %}

</div>





<div class="bs-docs-section" markdown="1">
#Ad server timeout & targeting

###If you're using DFP

####1. Set Adserver Timeout Setting (Optional)

One challenge we've heard about from publishers we work with is how to delay GPT for a certain amount of time so that pre-bid bidders have a chance to respond. In this section we'll describe what we believe is the easiest way to delay GPT while setting a timeout. Note that delaying GPT using this technique is strictly optional.

#####How does it work?

The below code sample tells the page to wait for the amount of time specified in the `PREBID_TIMEOUT` variable before loading the GPT library. This gives pre-bid bidders that amount of time to respond.

#####Why wrap the GPT library instead of the `.display()` calls?

Because GPT sends out all of the the impressions at the first `googletag.display()` function call, wrapping every single `.display()` calls in a `setTimeout` function is unrealistic. Instead, the easiest way we have found in working with publishers is to wrap the GPT library loading call in the `setTimeout` function. This way, your page's existing GPT implementation is left intact. GPT will only send out the impressions after the library is loaded; therefore your pre-bid bidders will have the amount of time specified `PREBID_TIMEOUT` in which to respond.

{% highlight js %}

PREBID_TIMEOUT = 300;
function initAdserver() {
    (function() {
        var gads = document.createElement('script');
        gads.async = true;
        gads.type = 'text/javascript';
        var useSSL = 'https:' == document.location.protocol;
        gads.src = (useSSL ? 'https:' : 'http:') + '//www.googletagservices.com/tag/js/gpt.js';
        var node = document.getElementsByTagName('script')[0];
        node.parentNode.insertBefore(gads, node);
    })();
    pbjs.initAdserverSet = true;
};
setTimeout(initAdserver, PREBID_TIMEOUT);

{% endhighlight %}

#####Function Explained

{: .table .table-bordered .table-striped }
| Function Name | Description |
| :--- | :---- |
| setTargetingFor GPTAsync(slotObj, adUnitCode) | Set query string targeting on a given GPT ad unit. The logic for deciding query strings is described in the section [Configure AdServer Targeting](configure-adserver-targeting). This function is only relevant if you're using DFP as your ad server. |


####2. Set Targeting (Required)

Given the GPT slot object and the ad unit code, `pbjs.setTargetingForGPTAsync` will set the bid's price bucket and size targeting info on the slot object using GPT's `setTargeting` function. The logic for coming up with query string targeting can be found in the section [Configure AdServer Targeting](configure-adserver-targeting).

It's important to wrap `pbjs.setTargetingForGPTAsync` with the `pbjs.libLoaded` condition, because there's no guarantee that the Prebid.js library has been loaded at this point in the script's execution.

{% highlight js %}

googletag.cmd.push(function() {
    var slot = googletag.defineSlot('/1996833/slot-1', [[300, 250], [728, 90]]);
    if (pbjs.libLoaded) {
        pbjs.setTargetingForGPTAsync(slot, '/1996833/slot-1');
    }
});

{% endhighlight %}


###If you're using a custom ad server

You've probably already configured your custom ad server to wait for a certain amount of time before sending out the impressions. That amount of time is given to your pre-bid bidders to respond.

Note that `pbjs.getAdserverTargetingParamsForPlacement(code)` will only return targeting parameters after a few hundred milliseconds (the time it takes AppNexus to respond with bids). Make sure to wrap the call to set up targeting parameters with the `pbjs.libLoaded` condition, in case the Prebid.js library hasn't been loaded yet.

{% highlight js %}

PB_PAGE_TIMEOUT = 300;
function initAdserver() {
    pbjs.initAdserverSet = true;
    // Step 1: Set key value targeting on the placements
    if (pbjs.libLoaded) {
        // For each of your placement, get and set targeting params
        var targetingParams = pbjs.getAdserverTargetingParamsForAdUnit(code);
    }
    // Step 2: Trigger the page to send the impressions to your ad server.
};
setTimeout(initAdserver, PB_PAGE_TIMEOUT);

{% endhighlight %}

#####Function Explained

{: .table .table-bordered .table-striped }
| Function Name | Description | Return Example |
| :--- | :---- | :---- |
| getAdserver TargetingParams ForAdUnit (adUnitCode) | This function returns the query string targeting parameters for a given ad unit. This function will only return values after the prebid.js library has loaded. | [{<br>  key: "apn_adid",<br>  val: "234235" },<br>{<br>  key: "apn_key",<br>  val: "300x250p1.9" <br>}] |

</div>




