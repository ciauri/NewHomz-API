NewHomz API Documentation
=========================
This document will go through the different calls available to you while using the NewHomz API. There is currently no rate limit on these methods. However, if abuse is detected, we will, at our own discretion, revoke access.

Source will be posted once it is cleaned up.

## Resources
### /nhmz/comm_map.php
___
Provides methods for finding listings based on location, price, bed, bath, and square footage
#### Methods
##### GET
---
`/nhmz/comm_map.php?loc`

or
`/nhmz/comm_map.php?loc&pl&ph&sl&sh&bed&ba`

or
`/nhmz/comm_map.php?latStart&latStop&lonStart&lonStop`

Returns a list of [Listings](#listing) that conform to the given parameters. May be used only in the above combinations.
###### Request Query Parameters
|Parameter|Value|Description|
|---------|-----|-----------|
|loc|string| A location within California, USA that will be geocoded using Google's Geocode API|
|pl|int| Price Low. The lower bound in dollars of the desired price range|
|ph|int| Price High. The upper bound in dollard of the desired price range|
|sl|int| Square feet low. The lower bound in square feet of the desired living space|
|sh|int| Square feet high. The upper bound in square feet of the desired living space|
|bed|int| Bedrooms. The desired number of bedrooms|
|ba|int| Baths. The desired number of bedrooms and bathrooms|
|latStart|float| The beginning of the Latitude range of the search region|
|latStop|float| The end of the Latitude range of the search region|
|lonStart|float| The beginning of the Longitude range of the search region|
|lonStop|float| The end of the Longitude range of the search region|

###### Example Queries
`/nhmz/comm_map.php?loc=Los+Angeles+County`

`/nhmz/comm_map.php?loc=Irvine%2C+CA&pl=$300K&ph=$1.5M&sl=400&sh=8000&bed=1&ba=1`

`/nhmz/comm_map.php?latStart=33.63418720159652&latStop=34.39191431844466&lonStart=-118.51086645628779&lonStop=-117.9479796691537`

### /nhmz/build_list.php
___
Provides methods for finding builders based on name, and number of communities they own
#### Methods
##### GET
---
`/nhmz/build_list.php`

or
`/nhmz/build_list.php?name&nl&nh`

Returns a list of [Builder](#builder) objects that conform to the given parameters. If no parameters are specified, all builders are returned.
###### Request Query Parameters
|Parameter|Value|Description|
|---------|-----|-----------|
|name|string| A partial or full name of the desired builder|
|nl|int| Number Low. The lower bound of communities owned by the builder|
|nh|int| Number High. The upper bound of communities owned by the builder|

###### Example Queries
`/nhmz/build_list.php`

`/nhmz/build_list.php?q=&name=&nl=1&nh=20`

### /nhmz/profile_builder.php
___
Provides a method for obtaining detailed information about a builder, including all of their active listings
#### Methods
##### GET
---
`/nhmz/profile_builder.php?id`

Returns the [BuilderDetail](#builderdetail) object of the builder associated with the id provided by the parameter.
###### Request Query Parameters
|Parameter|Value|Description|
|---------|-----|-----------|
|id|int| The builder ID|

###### Example Queries
`/nhmz/profile_builder.php?id=36`

## Objects
Consumers of these objects should tolerate the addition of new fields and variance in ordering of fields with ease. Not all fields appear in all contexts. It is generally safe to consider a nulled field, an empty set, and the absence of a field as the same thing. Please note that objects found in search results vary somewhat in structure from this document.
### Listing
___
These are the core of the map-based view of the application. While these objects do not provide detailed information about each listing, they provide the bare minimum amount of data for them to be displayed on a map with at-a-glance information


|Field|Type|Description|
|:----|:---|:----------|
|communityID|int|The ID associated with the listing|
|community|string|(deprecated) The name of the listing|
|json|string|The URL where the [ListingDetail](#listingdetail) object resides|
|builderID|int|The ID associated with the listing's builder|
|builderImg|string|The URL of the builder's logo|
|builderImgSrc|string|(deprecated) The URL of the builder's logo|
|builderName|string|The name of the builder|
|name|string|The name of the listing|
|city|string|The city where the listing is located|
|county|string|The county where the listing is zoned|
|state|string|The 2-digit state acronym|
|priceTxt|string|The qualifying text to preceed the price value|
|price|int|(nullable) The lowest price of the listing (No, a null value does not mean free house)|
|photoThumbnailUrl|string|The URL of the featured photo|
|lat|float|Latitude|
|lng|float|Longitude|
|active|int|Status of the listing. 1 = Standard. 2 = Featured|
|mpId|int|(nullable) ID of the Master Plan the listing belongs to, if any|
|mpName|string|(nullable) Name of the Master Plan|
|mpLat|float|(nullable) Latitude of the Master Plan's sales office|
|mpLng|float|(nullable) Longitude of the Master Plan's sales office|
|mpPhoto|string|(nullable) URL of the Logo associated with the Master Plan|
#### Example
```json
{
    "communities":[{
      "communityID": "2823",
      "community": "Melody at Beacon Park",
      "json": "http:\/\/nhmzj.s3.amazonaws.com\/comm_2823.json",
      "builderID": "108",
      "builderImg": "http:\/\/s3.amazonaws.com\/nhmzb\/delf42710xwgo84wo0.jpg",
      "builderImgSrc": "http:\/\/s3.amazonaws.com\/nhmzb\/delf42710xwgo84wo0.jpg",
      "builderName": "Lennar Homes",
      "name": "Melody at Beacon Park",
      "city": "Irvine",
      "county": "Orange",
      "state": "Ca",
      "priceTxt": "From the mid",
      "price": "900000",
      "photoThumbnailUrl": "http:\/\/cdn.lennar.net\...",
      "lat": "33.69367620",
      "lng": "-117.73137580",
      "active": "2",
      "mpId": "1",
      "mpName": "Great Park Neighborhoods",
      "mpLat": "33.68866900",
      "mpLng": "-117.73377300",
      "mpPhoto": "http:\/\/s3.amazonaws.com\/nhmzb\/1ob63yodpf0gwsook0.png"
      }]
}
```



### ListingDetail
___
This object provides all of the data associated with a particular listing.


|Field|Type|Description|
|:----|:---|:----------|
|communityID|int|The ID associated with the listing|
|builder|string|The name of the builder|
|builderID|int|The ID associated with the listing's builder|
|builderImg|string|The URL of the builder's logo|
|builderImgSrc|string|(deprecated) The URL of the builder's logo|
|builderPaid|int|Indicates whether or not the builder is featured|
|builderWeb|string|URL of the builder's homepage|
|builderMsg|string|Statement indicating the Co-Op status of the builder|
|name|string|The name of the listing|
|description|string|A description of the listing|
|videoID|string|(nullable) YouTube ID of the video associated with the listing|
|photoThumbnailUrl|string|The URL of the featured photo|
|gallery|Collection of strings|An array of URLs of photos associated with the listing|
|phone|string|Formatted string of the sales office's phone number|
|priceTxt|string|The qualifying text to preceed the price value|
|price|int|(nullable) The lowest price of the listing (No, a null value does not mean free house)|
|paymentTxt|string|Estimated interest rate and down payment|
|city|string|The city where the listing is located|
|county|string|The county where the listing is zoned|
|lat|float|Latitude|
|lng|float|Longitude|
|details|ListingHelper|(deprecated) Contains summary information about the listing|
|floorplans|Collection of [CaptionedImages](#captionedimage)|
|active|int|Status of the listing. 1 = Standard. 2 = Featured|
|website|string|URL of the listing website|
|bedLow|int|Lower bound of bedrooms in floorplans|
|bedHigh|int|Upper bound of bedrooms in floorplans|
|bathLow|int|Lower bound of bathrooms in floorplans|
|bathHigh|int|Upper bound of bathrooms in floorplans|
|sqftLow|int|Lower bound of square footage of floorplans|
|sqftHigh|int|Upper bound of square footage of floorplans|
|zip|int|Zip code of listing|
|propType|string|Description of the type of property (Single Family Residense, Attached, etc.)|
#### Example
```json

{
    "community": [{
        "communityID": "2823",
        "builderID": "108",
        "builder": "Lennar Homes",
        "builderImg": "http:\/\/s3.amazonaws.com\/nhmzb\/delf42710xwgo84wo0.jpg",
        "builderImgSrc": "http:\/\/s3.amazonaws.com\/nhmzb\/delf42710xwgo84wo0.jpg",
        "builderPaid": "0",
        "builderWeb": "http:\/\/www.newhomz.com\/redirect.php?u=www.lennar.com%2F",
        "brokerMsg": "Broker Co-Op",
        "name": "Melody at Beacon Park",
        "description": "Lennar is proud to announce...",
        "videoID": "",
        "photoThumbnailUrl": "http:\/\/cdn.lennar.net\...",
        "gallery": [
            ["http:\/\/cdn.lennar.net...", 
            "http:\/\/cdn.lennar.net..."]
        ],
        "phone": "(888) 220-5502",
        "priceTxt": "From the mid",
        "price": "900000",
        "paymentTxt": "*Est. rate 4.5% and 20% down",
        "fullBaths": "4.5",
        "bedrooms": "4",
        "lat": "33.69367620",
        "lng": "-117.73137580",
        "details": [{
            "City": "Irvine",
            "Community": "Melody at Beacon Park",
            "County": "Orange",
            "Zip Code": "92618",
            "Builder": "Lennar Homes",
            "Property Type": "Detached",
            "Sqft": "2,321 - 2,774",
            "Lot Size": "0 sqft",
            "Bedrooms": "4 - 4",
            "Baths": "4 - 4.5",
            "empty1": "0"
        }],
        "floorplans": [{
            "caption": "Residence 1",
            "image": "http:\/\/s3.amazonaws.com\/nhmzf\/syl6xncvcfkogggkck.jpg"
        }, {
            "caption": "Residence 2",
            "image": "http:\/\/s3.amazonaws.com\/nhmzf\/7m2vpiptdpc0o8wcsg.jpg"
        }, {
            "caption": "Residence 3",
            "image": "http:\/\/s3.amazonaws.com\/nhmzf\/qwy4t7m0yes84c4s4o.jpg"
        }, {
            "caption": "Residence 4",
            "image": "http:\/\/s3.amazonaws.com\/nhmzf\/l8qgs28etvk48gsc4.jpg"
        }],
        "active": "2",
        "website": "http:\/\/www.lennar.com",
        "bedLow": "4",
        "bedHigh": "4",
        "bathLow": "4",
        "bathHigh": "4.5",
        "zip": "92618",
        "sqftLow": "2321",
        "sqftHigh": "2774",
        "propType": "Detached"
    }]
}
```

### Builder
___
Provides an at-a-glance summary of the builder and data necessary for follow-up calls


|Field|Type|Description|
|:----|:---|:----------|
|builderID|int|The ID associated with the builder|
|photoThumbnailUrl|string|The URL of a randomly selected [Listing](#listing) photo|
|builderImg|string|The URL of the builder's logo|
|name|string|The builder's name|
|communities|int|Number of communities the builder owns|
|paid|boolean|Featured status of the builder|

#### Example
```json
{
    "builders": [{
      "builderID": "27",
      "photoThumbnailUrl": "http:\/\/nhmzc.s3.amazonaws.com\/img3.jpg",
      "builderImg": "http:\/\/nhmzb.s3.amazonaws.com\/tmp_builder.jpg",
      "builderImgSrc": "",
      "name": "BDC Development",
      "communities": "3",
      "paid": "0"
      }]
}
```

### BuilderDetail
___
Similar to the [Builder](#builder) object, with the addition of an exhaustive list of [Listings](#listing)


|Field|Type|Description|
|:----|:---|:----------|
|builderID|int|The ID associated with the builder|
|photoThumbnailUrl|string|The URL of a randomly selected [Listing](#listing) photo|
|builderImg|string|The URL of the builder's logo|
|builderName|string|The builder's name|
|commCnt|int|Number of communities the builder owns|
|communities|Collection of sample Listing data|
#### Example
```json
{
    "profile": [{
        "builderID": "36",
        "builderImg": "http:\/\/s3.amazonaws.com\/nhmzb\/113yet7jb3io8gk4c4.jpg",
        "builderImgSrc": "http:\/\/s3.amazonaws.com\/nhmzb\/113yet7jb3io8gk4c4.jpg",
        "builderName": "Brandywine Homes",
        "photoThumbnailUrl": "http:\/\/nhmzc.s3.amazonaws.com\/img6.jpg",
        "commCnt": 11,
        "communities": [{
            "communityID": "3267",
            "json": "http:\/\/nhmzj.s3.amazonaws.com\/comm_3267.json",
            "name": "Lakehouse",
            "city": "Anaheim",
            "state": "CA",
            "priceTxt": "Coming Soon",
            "price": "0",
            "photoThumbnailUrl": "http:\/\/nhmzc.s3.amazonaws.com\/7wur6z4ykb5jjxkcdh",
            "status": "1"
        }]
    }]
}
```

### ListingHelper
___
A legacy object that contains summary data about a [Listing](#listing)


|Field|Type|Description|
|:----|:---|:----------|
|City|string|City of the listing|
|Community|string|Listing name|
|County|string|The county where the listing is zoned|
|Zip Code|string|Zip of the listing|
|Builder|string|Name of the listing's builder|
|Property Type|string|Description of the type of property (Single Family Residense, Attached, etc.)|
|Sqft|string|Range of the availbable listing's square footage|
|Bedrooms|string|Range of the listing's bedrooms|
|Baths|string|Range of the listing's bathrooms|
#### Example
```json
"details": [{
    "City": "Anaheim",
    "Community": "Lakehouse",
    "County": "Orange",
    "Zip Code": "92805",
    "Builder": "Brandywine Homes",
    "Property Type": "Detached",
    "Sqft": "2,144 - 2,755",
    "Lot Size": "0 sqft",
    "Bedrooms": "0 - 0",
    "Baths": "0 - 0",
}]
```

### CaptionedImage
___
An object containing a URL and caption for the image


|Field|Type|Description|
|:----|:---|:----------|
|caption|string|The image's caption|
|image|string|URL of the image|

#### Example
```json
"floorplans": [{
    "caption": "Residence 1",
    "image": "http:\/\/s3.amazonaws.com\/nhmzf\/syl6xncvcfkogggkck.jpg"
}, {
    "caption": "Residence 2",
    "image": "http:\/\/s3.amazonaws.com\/nhmzf\/7m2vpiptdpc0o8wcsg.jpg"
}, {
    "caption": "Residence 3",
    "image": "http:\/\/s3.amazonaws.com\/nhmzf\/qwy4t7m0yes84c4s4o.jpg"
}, {
    "caption": "Residence 4",
    "image": "http:\/\/s3.amazonaws.com\/nhmzf\/l8qgs28etvk48gsc4.jpg"
}]
```
