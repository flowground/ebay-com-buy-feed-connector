# ![LOGO](logo.png) Item Feed Service **flow**ground Connector

## Description

A generated **flow**ground connector for the Item Feed Service API (version v1_beta.13.0).

Generated from: https://api.apis.guru/v2/specs/ebay.com/buy-feed/v1_beta.13.0/swagger.json<br/>
Generated at: 2019-05-07T17:40:19+03:00

## API Description

The Feed API provides the ability to download TSV_GZIP feed files containing eBay items and an hourly snapshot file of the items that have changed within an hour for a specific category, date and marketplace. In addition to the API, there is an open source Feed SDK written in Java that downloads, combines files into a single file when needed, and unzips the entire feed file. It also lets you specify field filters to curate the items in the file.

## Authorization

Supported authorization schemes:
- OAuth2

For OAuth 2.0 you need to specify OAuth Client credentials as environment variables in the connector repository:
* `OAUTH_CLIENT_ID` - your OAuth client id
* `OAUTH_CLIENT_SECRET` - your OAuth client secret

## Actions

### This method lets you download a TSV_GZIP (tab separated value gzip) Item feed file. The feed file contains all the items from all the child categories of the specified category. The first line of the file is the header, which labels the columns and indicates the order of the values on each line. Each header is described in the Response fields section. There are two types of item feed files generated: A daily Item feed file containing all the newly listed items for a specific category, date, and marketplace (feed_scope = NEWLY_LISTED) A weekly Item Bootstrap feed file containing all the items in a specific category and marketplace (feed_scope = ALL_ACTIVE) Note: Filters are applied to the feed files. For details, see Feed File Filters. When curating the items returned, be sure to code as if these filters are not applied as they can be changed or removed in the future. URLs for this method Production URL: https://api.ebay.com/buy/feed/v1_beta/ Sandbox URL: https://api.sandbox.ebay.com/buy/feed/v1_beta/ Downloading feed files Item feed files are binary gzip files. If the file is larger than 100 MB, the download must be streamed in chunks. You specify the size of the chunks in bytes using the Range request header. The Content-range response header indicates where in the full resource this partial chunk of data belongs and the total number of bytes in the file. For more information about using these headers, see Retrieving a gzip feed file. In addition to the API, there is an open source Feed SDK written in Java that downloads, combines files into a single file when needed, and unzips the entire feed file. It also lets you specify field filters to curate the items in the file. Note: The response is always a TSV_GZIP file. However for documentation purposes, the response is shown as JSON fields so that the value returned in each column can be explained. The order of the response fields, shows you the order of the columns in the feed file. Restrictions For a list of supported sites and other restrictions, see API Restrictions.

*Tags:* `item`

#### Input Parameters
* `X-EBAY-C-MARKETPLACE-ID` - _required_ - The ID of the eBay marketplace where the item is hosted. Note: This value is case sensitive. For example: &nbsp;&nbsp;X-EBAY-C-MARKETPLACE-ID = EBAY_US For a list of supported sites see, API Restrictions.
* `Range` - _required_ - This header specifies the range in bytes of the chunks of the gzip file being returned. Format: bytes=startpos-endpos For example, the following retrieves the first 10 MBs of the feed file. &nbsp;&nbsp;Range bytes=0-10485760 For more information about using this headers, see Retrieving a gzip feed file. Maximum: 100 MB
* `feed_scope` - _required_ - Specifies the type of feed file to return. Valid Values: NEWLY_LISTED - Returns the daily Item feed file containing all items that were listed on the day specified by the date parameter in the category specified by the category_id parameter. The items are Good 'Til Cancelled and non-Good 'Til Cancelled items. If the item is a non-Good 'Til Cancelled item, the item's end date will be returned in the itemEndDate column. /item?feed_scope=NEWLY_LISTED&amp;category_id=15032&amp;date=20170925 ALL_ACTIVE - Returns the weekly Item Bootstrap feed file containing all the 'Good 'Til Cancelled' items in the category specified by the category_id parameter. /item?feed_scope=ALL_ACTIVE&amp;category_id=15032
* `category_id` - _required_ - An eBay top-level category ID of the items to be returned in the feed file. The list of eBay category IDs changes over time and category IDs are not the same across all the eBay marketplaces. To get a list of the top-level categories for a marketplaces, you can use the Taxonomy API getCategoryTree method. This method retrieves the complete category tree for the marketplace. The top-level categories are identified by the categoryTreeNodeLevel field. For example: &nbsp;&nbsp;&quot;categoryTreeNodeLevel&quot;: 1 For details see Get Categories for Buy APIs. Restriction: Must be a top-level (L1) category
* `date` - _optional_ - The date of the daily Item feed file (feed_scope=NEWLY_LISTED) you want. The date is required only for the daily Item feed file. If you specify a date for the Item Bootstrap file (feed_scope=ALL_ACTIVE), the date is ignored and the latest file is returned. The date the Item Bootstrap feed file was generated is returned in the Last-Modified response header. The Item feed files are generated every day and there are always 14 files available. The daily Item feed files are available each day after 9AM MST (US Mountain Standard Time), which is -7 hours UTC time. There is a 48 hour latency when generating the Item feed files. This means you can download the file for July 10th on July 12 after 9AM MST. Note: For categories with a large number of items, the latency can be up to 72 hours. Format: yyyyMMdd Requirements: Required when feed_scope=NEWLY_LISTED Must be within 3-14 days in the past

### This method lets you download a TSV_GZIP (tab separated value gzip) Item Group feed file. An item group is an item that has various aspect differences, such as color, size, storage capacity, etc. There are two types of item group feed files generated: A daily Item Group feed file containing the item group variation information associated with items returned in the Item feed file for a specific day, category, and marketplace. (feed_scope = NEWLY_LISTED) A weekly Item Group Bootstrap feed file containing all the item group variation information associated with items returned in the Item Bootstrap feed file for all the items in a specific category. (feed_scope = ALL_ACTIVE) Note: Filters are applied to the feed files. For details, see Feed File Filters. When curating the items returned, be sure to code as if these filters are not applied as they can be changed or removed in the future. The contents of these feed files are based on the contents of the corresponding daily Item or Item Bootstrap feed file. When a new Item or Item Bootstrap feed file is generated, the service reads the file and if an item in the file has a primaryItemGroupId value, which indicates the item is part of an item group, it uses that value to return the item group (parent item) information for that item in the corresponding Item Group or Item Group Bootstrap feed file. This information includes the name/value pair of the aspects of the items in this group returned in the variesByLocalizedAspects column. For example, if the item was a shirt some of the variation names could be Size, Color, etc. Also the images for the various aspects are returned in the additionalImageUrls column. The first line in any feed file is the header, which labels the columns and indicates the order of the values on each line. Each header is described in the Response fields section. Combining the Item Group and Item feed files The Item Group or Item Group Bootstrap feed file contains details about the item group (parent item), including the item group ID itemGroupId. You match the value of itemGroupId from the Item Group feed file with the value of primaryItemGroupId from the corresponding daily Item or Item Bootstrap feed file. URLs for this method Production URL: https://api.ebay.com/buy/feed/v1_beta/ Sandbox URL: https://api.sandbox.ebay.com/buy/feed/v1_beta/ Downloading feed files Item Group feed files are binary gzip files. If the file is larger than 100 MB, the download must be streamed in chunks. You specify the size of the chunks in bytes using the Range request header. The content-range response header indicates where in the full resource this partial chunk of data belongs and the total number of bytes in the file. For more information about using these headers, see Retrieving a gzip feed file. Note: The response is always only a TSV_GZIP file. However for documentation purposes, the response is shown as JSON fields so that the value returned in each column can be explained. The order of the response fields, shows you the order of the columns in the feed file. Restrictions For a list of supported sites and other restrictions, see API Restrictions.

*Tags:* `item_group`

#### Input Parameters
* `X-EBAY-C-MARKETPLACE-ID` - _required_ - The ID of the eBay marketplace where the item is hosted. Note: This value is case sensitive. For example: &nbsp;&nbsp;X-EBAY-C-MARKETPLACE-ID = EBAY_US For a list of supported sites see, API Restrictions.
* `Range` - _optional_ - This header specifies the range in bytes of the chunks of the gzip file being returned. Format: bytes=startpos-endpos For example, the following retrieves the first 10 MBs of the feed file. &nbsp;&nbsp;Range bytes=0-10485760 For more information about using this headers, see Retrieving a gzip feed file. Maximum: 100 MB
* `feed_scope` - _required_ - Specifies the type of file to return. Valid Values: NEWLY_LISTED - Returns the Item Group feed file containing the item group variation information for items in the daily Item feed file that were associated with an item group. The items in this type of Item feed file are items that were listed on the day specified by the date parameter in the category specified by the category_id parameter. The items will be Good 'Til Cancelled and non-Good 'Til Cancelled items. If the item is a non-Good 'Til Cancelled item, the item's end date will be returned in the itemEndDate column. /item_group?feed_scope=NEWLY_LISTED&amp;category_id=15032&amp;date=20170925 ALL_ACTIVE - Returns the weekly Item Group Bootstrap file containing the item group variation information for items in the weekly Item Bootstrap feed file that were associated with an item group. The items are 'Good 'Til Cancelled' items in the category specified by the category_id parameter. /item_group?feed_scope=ALL_ACTIVE&amp;category_id=15032
* `category_id` - _required_ - An eBay top-level category ID of the items to be returned in the feed file. The list of eBay category IDs changes over time and category IDs are not the same across all the eBay marketplaces. To get a list of the top-level categories for a marketplaces, you can use the Taxonomy API getCategoryTree method. This method retrieves the complete category tree for the marketplace. The top-level categories are identified by the categoryTreeNodeLevel field. For example: &nbsp;&nbsp;&quot;categoryTreeNodeLevel&quot;: 1 For details see Get Categories for Buy APIs. Restriction: Must be a top-level category
* `date` - _optional_ - The date of the daily Item Group feed file (feed_scope=NEWLY_LISTED) you want. The date is required only for the daily Item Group feed file. If you specify a date for the Item Group Bootstrap file (feed_scope=ALL_ACTIVE), the date is ignored and the latest file is returned. The date the Item Group Bootstrap feed file was generated is returned in the Last-Modified response header. The Item Group feed files are generated every day and there are always 14 files available. There is a 48 hour latency when generating the files. This means on July 10, the latest feed file you can download is July 8. Note: The generated files are stored using MST (US Mountain Standard Time), which is -7 hours UTC time. Format: yyyyMMdd Requirement: Requirements: Required only when feed_scope=NEWLY_LISTED Must be within 3-14 days in the past

### The Hourly Snapshot feed file is generated each hour every day for all categories. This method lets you download an Hourly Snapshot TSV_GZIP (tab separated value gzip) feed file containing the details of all the items that have changed within the specified day and hour for a specific category. This means to generate the 8AM file of items that have changed from 8AM and 8:59AM, the service starts at 9AM. You can retrieve the 8AM snapshot file at 10AM. Note: Filters are applied to the feed files. For details, see Feed File Filters. When curating the items returned, be sure to code as if these filters are not applied as they can be changed or removed in the future. You can use the response from this method to update the item details of items stored in your database. By comparing the value of itemSnapshotDate for the same item you will be able to tell which information is the latest. Important: When the value of the availability column is UNAVAILABLE, only the itemId and availability columns are populated. URLs for this method Production URL: https://api.ebay.com/buy/feed/v1_beta/ Sandbox URL: https://api.sandbox.ebay.com/buy/feed/v1_beta/ Downloading feed files Hourly snapshot feed files are binary gzip files. If the file is larger than 100 MB, the download must be streamed in chunks. You specify the size of the chunks in bytes using the Range request header. The Content-range response header indicates where in the full resource this partial chunk of data belongs and the total number of bytes in the file. For more information about using these headers, see Retrieving a gzip feed file. Note: The response is always a TSV_GZIP file. However for documentation purposes, the response is shown as JSON fields so that the value returned in each column can be explained. The order of the response fields, shows you the order of the columns in the feed file. Restrictions For a list of supported sites and other restrictions, see API Restrictions.

*Tags:* `item_snapshot`

#### Input Parameters
* `X-EBAY-C-MARKETPLACE-ID` - _required_ - The ID of the eBay marketplace where the item is hosted. Note: This value is case sensitive. For example: &nbsp;&nbsp;X-EBAY-C-MARKETPLACE-ID = EBAY_US For a list of supported sites see, API Restrictions.
* `Range` - _required_ - This header specifies the range in bytes of the chunks of the gzip file being returned. Format: bytes=startpos-endpos For example, the following retrieves the first 10 MBs of the feed file. &nbsp;&nbsp;Range bytes=0-10485760 For more information about using this headers, see Retrieving a gzip feed file. Maximum: 100 MB
* `category_id` - _required_ - An eBay top-level category ID of the items to be returned in the feed file. The list of eBay category IDs changes over time and category IDs are not the same across all the eBay marketplaces. To get a list of the top-level categories for a marketplace, you can use the Taxonomy API getCategoryTree method. This method retrieves the complete category tree for the marketplace. The top-level categories are identified by the categoryTreeNodeLevel field. For example: &nbsp;&nbsp;&quot;categoryTreeNodeLevel&quot;: 1 For details see Get Categories for Buy APIs. Restriction: Must be a top-level category
* `snapshot_date` - _required_ - The hour of the incremental feed file you want, for a particular day. There are always 14 days of Hourly Snapshot feed files available. If you specify that you want the 9AM file for July 15, 2017 (2017-07-15T09:00:00.000Z), the data in the feed file will be items that changed after 9AM on July 15, 2017. Restrictions: Files are generated on the hour, so minutes and seconds are always zeros. &nbsp;&nbsp;&nbsp;(2017-07-12T09:00:00.000Z) Format: UTC format (yyyy-MM-ddThh:00:00.000Z)

## License

**flow**ground :- Telekom iPaaS / ebay-com-buy-feed-connector<br/>
Copyright © 2019, [Deutsche Telekom AG](https://www.telekom.de)<br/>
contact: flowground@telekom.de

All files of this connector are licensed under the Apache 2.0 License. For details
see the file LICENSE on the toplevel directory.
