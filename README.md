# ReadMe

**Apple News API Client Utility**

This ReadMe explains how to install, configure, and use the Apple News API client to post an article.

## 1.0 System Requirements

- OS X 10.9.0 or later 
- Xcode 6 or later (Or, optionally, you can just install the Command Line Tools for Xcode. See [developer.apple.com](https://developer.apple.com).)

## 2.0 Installation and Setup Information

To use the Apple News API, do the following:

**Step 1:** Make sure you have downloaded the Ruby CLI for the Apple News API Client Utility. Using Finder, check the contents of the Apple-News-API-CLI folder in your Downloads directory. You should see the papi-client-{VERSION}.gem file.

Where `{VERSION}` is the current version found in `/lib/papi-client/version.rb`.

**Step 2:** Run the following command to install the gem:

	sudo gem install ~/Downloads/Apple-News-API-CLI/papi-client-{VERSION}.gem

If the command is not successful, check that you have Xcode installed. If you don't, you can download it from the App Store.

**Step 4:** Use any text editor to create a configuration file called .papi in the current user's home directory (at ~/.papi).

**Step 5:** Add the following information to the .papi configuration file:

	#<Your Channel>
	endpoint: https://news-api.apple.com
	channel_id: <Your Channel ID>
	key: <Your API Key ID>
	secret: <Your API Key Secret>

where angle brackets (<>), enter your own unique information.

**Note:** Use the same publisher credentials you used when you set up your channel.

**Step 6:** Run the following command to get information about your test channel:

	papi-client channel get

You should see the following type of information about the channel:

	{
		"data": {
		"createdAt": "2016-03-19T22:50:50Z",
		"modifiedAt": "2016-03-26T04:07:11Z",
		"id":"42424242-4242-4242-4242-424242424242",
		"type": "channel",
		"links": {
			"defaultSection":"https://news-api.apple.com/sections/43434343-4343-4343-4343-434343434343",
			"self": "http://news-api.apple.com/channels/49f6a7d3-eb20-3ab2-be3b-8399e7f28abf"
		 },
		 "name":"Example Channel",
		 "website": "http://www.example.com/"
		}
	}

**Step 7:** Run the following command to publish an article:

	papi-client article publish <Article Directory>

You should receive a response containing a JSON representation of your article bundle.

Congratulations, you published an article to your channel!

A successful response will look like the following and contain both a shareUrl (near the bottom) and ID (near the top) from Apple's servers.

	{
	  "data": {
	    "createdAt": "2015-09-28T21:08:42Z",
	    "modifiedAt": "2015-09-28T21:08:42Z",
	    "id": "baef2d66-567c-4a80-ad2d-0d74638dd9a3",
	    "type": "article",
	    "links": {
	      "channel": "https://news-api.apple.com/channels/49f6a7d3-eb20-3ab2-be3b-8399e7f28abf",
	      "self": "https://news-api.apple.com/articles/baef2d66-567c-4a80-ad2d-0d74638dd9a3",
	      "sections": [
	        "https://news-api.apple.com/sections/ce80da93-b06c-3a6b-9c72-e83250e18c3d"
	      ]
	    },
	    "document": {
	      "version": "0.10",
	      "identifier": "testArticle",
	      "language": "en",
	      "title": "Test Article",
	      "layout": {
	        "columns": 7,
	        "width": 1024,
	        "margin": 75,
	        "gutter": 50
	      },
	      "components": [
	        {
	          "role": "title",
	          "text": "Test Set",
	          "layout": "heading"
	        },
	        {
	          "role": "heading1",
	          "text": "Test 1",
	          "layout": "heading"
	        },
	        {
	          "role": "photo",
	          "URL": "bundle://image.jpg",
	          "layout": {
	            "columnStart": 0,
	            "columnSpan": 5
	          }
	        },
	        {
	          "role": "heading1",
	          "text": "Test 2",
	          "layout": "heading"
	        },
	        {
	          "role": "body",
	          "text": "Lorem ipsum doporttitor. Morbi lectus risus, iaculis vel, suscipit quis, luctus non, massa.",
	          "layout": {
	            "columnStart": 2,
	            "columnSpan": 5
	          }
	        }
	      ],
	      "componentLayouts": {
	        "heading": {
	          "margin": {
	            "top": 12,
	            "bottom": 12
	          }
	        }
	      },
	      "componentTextStyles": {
	        "default": {
	          "fontName": "Helvetica",
	          "fontSize": 18,
	          "linkStyle": {
	            "textColor": "#428bca"
	          }
	        },
	        "default-title": {
	          "fontName": "Helvetica-Bold",
	          "fontSize": 36,
	          "hyphenation": false,
	          "textAlignment": "center"
	        },
	        "default-heading": {
	          "fontName": "Helvetica-Bold",
	          "fontSize": 24,
	          "hyphenation": false,
	          "textAlignment": "center"
	        }
	      }
	    },
	    "shareUrl": "https://apple.news/Auu8tZlZ8SoCtLQ10Y43Zow",
	    "revision": "AAAAAAAAAAD//////////w==",
	    "state": "PROCESSING",
	    "accessoryText": null,
	    "title": "Test Article",
	    "isSponsored": false,
	    "isPreview": false
	  }
	}

If you did not receive confirmation, see the Troubleshooting section below for a possible explanation.

## 3.0 Command Reference

This section contains commands for commonly performed tasks:

**Note**: The `--verbose` flag can be used for `channel get`, to show the full list of fonts within the response object. Or with `article publish`, and `article update` commands to show the document JSON you publishing/updating in addition to other information.

### Get channel information.

	papi-client channel get

### Get article information.

	papi-client article get <articleId>

### Get list of channel fonts.

	papi-client channel fonts

### Get channel quota information.

	papi-client channel quota

### Publish a new article (without metadata).

	papi-client article publish <dir of article>

(This article will display in the Topic Feeds.)

### Publish a new article (with metadata).

	papi-client article publish <dir of article>
		--is_sponsored=(true|false)
		--is_preview=(true|false)
		--maturity_rating="(GENERAL|KIDS|MATURE)"
		--is_hidden=(true|false)
		--accessory_text="{string}"
		--target_territories="{comma delimited string of territories}"

### Publish a new article to a specific section.

	papi-client article publish <dir of article> --section_ids=section_id1,section_id2,section_id3

### Update an article.

	papi-client article update <articleId> <revision> --article_dir=<dir of article>

### Delete an article.

	papi-client article delete <articleId>

### Search an article.

	papi-client article search <search option - see the Apple News API Reference documentation for details>

### Promote articles in a section

	papi-client section promote <sectionId> --article_ids=article_id1,article_id2,article_id3

**Note:** You can find the revision in the response from the Article Publish or Article Get command.

## 4.0 Troubleshooting

If you are unable to successfully install the gem, check that you have Xcode installed.

If you receive an error message while publishing an article, see the Apple News API Reference documentation.

If you receive an INVALID_DOCUMENT error, check for the following scenarios:

- The document does not render in the News Preview tool. Check the tool for errors and warnings regarding expected JSON formatting.
- The document contains fonts that do not exist in the channel.
- Images were referenced in the article that were not included in the article bundle.
- The document contains deprecated or unsupported objects and properties. Use the Apple New Format Reference to make sure that your article conforms to the expected structure.

If you receive a “No value provided for required option '--endpoint'” error message, check if you’ve created the configuration file called .papi in the correct location (~/.papi). For more information see 2.0 Installation and Setup Information, Step 4 and 5.


Copyright (c) 2020 Apple Inc. All rights reserved.
