+++

title = "How to programmatically get google search results"
slug = "how-to-programmatically-get-google-search-results"
date = 2020-04-22
[taxonomies]
tags = ["Google", "API", "REST", "Node.JS"]
+++

Recently, I needed a way how to programmatically search on google, while developing a linux applet - [rofi-search](https://github.com/fogine/rofi-search) which allows for interactively searching for top results across multiple search engines.

It's not known much that there is an official `Google API` you can use for getting the data right now!
<!-- more -->

_____________

## Data scrapers
Common techniques of solving the problem nowadays involve scraping the `DOM` for data from Google's website.  

There are multiple scraping libraries for many languages available, notably [GoogleScraper](https://github.com/NikolaiT/GoogleScraper) package for `python` or [google-it](https://github.com/PatNeedham/google-it) package for `NodeJS`.  

#### `google-it` code example in NodeJS

```javascript
const  search = require('google-it')

const response = await search({
    'query': 'How to programmatically get google search results'
});

console.log(response);
```

Disadvantage of data scrapers are that they need to be actively maintained and patched every time upstream website is updated, not mentioning that it is relatively heavy solution with noticeable processing overhead.  

However, often enough, it is the only way how to get the data from proprietary data sources. This time fortunately, we are in luck though as Google made it's public `Custom Search Engine API` available for everyone for free.

## Google Custom Search Engine ([CSE](https://cse.google.com/cse/))

Google's [CSE](https://cse.google.com/cse/) allows us to create a custom search engine specifically configured to our requirements whether it is prioritizing search results from specific websites or searching our web only.  

Important thing to know is if configured correctly, it can search for results on the entire web!

The following are few simple steps you will need to take, to setup your search API:  

- Create your own custom search engine [https://cse.google.com/cse/all](https://cse.google.com/cse/all) and get `Search engine ID` from the settings panel  
    {{ figure(src="/images/google_cse_id.png" alt="Google CSE ID" caption="Google CSE ID") }}

- Under `Basics` settings section where you got your `CSE ID`, find and enable `Search the entire web` option.

- [Get API key](https://developers.google.com/custom-search/v1/introduction#identify_your_application_to_google_with_api_key) for the created search engine.
    {{ figure(src="/images/google_cse_api_key.png" alt="Google CSE API key" caption="Google CSE API key") }}

- Now we can use our `CSE ID` and `API key` to make a GET API request to `https://www.googleapis.com/customsearch/v1?key=yourkey&cx=yourid&q=query`  

  Here is `NodeJS` code example which does not require any dependencies that you can copy paste:

```javascript
const https = require('https');

/**
 * @param {Object} options
 * @param {String} options.key - Google API key
 * @param {String} options.cx - Search Engine ID
 * @param {String} options.q - search query
 *
 * ...for list of all supported options, see
 * https://developers.google.com/custom-search/v1/reference/rest/v1/cse/list
 *
 * @returns {Promise<Object>}
 */
function search(options) {
    return new Promise(function(resolve, reject) {
        let query = '';

        //serialize url query parameters
        Object.keys(options || {}).forEach(function(prop) {
            query += prop+'='+encodeURIComponent(options[prop])+'&';
        });

        //api request options
        const opt = {
            hostname: 'www.googleapis.com',
            port: 443,
            path: `/customsearch/v1?${query}`,
            method: 'GET',
            headers: { Accept: 'application/json' }
        }

        //response data
        let data = '';

        const req = https.request(opt, res => {

            //process incomming response data stream
            res.on('data', chunk => {
                data += chunk;
            });

            //when end of stream is reached and there is no more data to read
            //return resolve or rejected promise,
            //depending on response http status code
            res.on('end', () => {
                if (res.statusCode >= 200 && res.statusCode < 300) {
                    resolve(JSON.parse(data));
                } else {
                    reject(data);
                }
            });
        });

        req.on('error', error => {
            reject(error);
        });

        req.end();
    });
};
```

You can use the `search` function as follows:

```javascript
const response = await search({
    cx: '001111568431131411111:aa33rweuabc',
    key: 'AKDYksKYicBgvX6k7G8mFWCABo0HqelXM7dOsA0',
    q: 'How to programmatically get google search results'
});

//dumps search results
console.log(response.items);
```

And there you have it, a lightweight solution which does not require you to regularly update scraping code nor install a ton of dependencies. More importantly, it uses official API supported by Google.  

There are few considerations that are helpful to make though.  
`CSE` doesn't include features such as personalized results, oneboxes etc.. and sometimes it might give slightly different results than when scraping the data from google's website.  

However, from my own experience with using `CSE`, the differences in search results have been insignificant.
