# Twitter API

## Realtime

*free/premium* [statuses/filter](https://developer.twitter.com/en/docs/tweets/filter-realtime/api-reference/post-statuses-filter)
*enterprise* [PowerTrack API](https://developer.twitter.com/en/docs/tweets/filter-realtime/overview/powertrack-api)

[Premium operator matching](https://developer.twitter.com/en/docs/tweets/rules-and-filtering/overview/all-operators)

### Filtering the stream

* Resource URL: `https://stream.twitter.com/1.1/statuses/filter.json`
* Response formats: `JSON`
* Requires authentication? Yes (user context only)
* Rate limited?	Yes

*Parameters*
* `follow`. Optional. Comma separated list of user IDs.
* `track`. Optional.  Phrases to track.
* and also `locations`, `delimited`, and `stall_warnings`

#### Tracking People
You need to make use of `follow` parameter. See [Basic Stream Parameters, follow](https://developer.twitter.com/en/docs/tweets/filter-realtime/guides/basic-stream-parameters#follow)

For each user specified, the stream will contain:
* Tweets created by the user.
* Tweets which are retweeted by the user.
* Replies to any Tweet created by the user.
* Retweets of any Tweet created by the user.
* Manual replies, created without pressing a reply button (e.g. “@twitterapi I agree”).

The stream will not contain:
* Tweets mentioning the user (e.g. “Hello @twitterapi!”).
* Manual Retweets created without pressing a Retweet button (e.g. “RT @twitterapi The API is great”).
* Tweets by protected users.

#### Tracking words
You need to make use of `track` parameter.  See [Basic Stream Parameters, track](https://developer.twitter.com/en/docs/tweets/filter-realtime/guides/basic-stream-parameters#track)

> A phrase may be one or more terms separated by spaces, and a phrase will match if all of the terms in the phrase are present in the Tweet *regardless of order and ignoring case*. […] By this model, you can think of commas as logical ORs, while spaces are equivalent to logical ANDs

> The text of the Tweet and some entity fields are considered for matches. Specifically, the *text attribute of the Tweet, expanded_url and display_url for links and media, text for hashtags, and screen_name for user mentions are checked for matches*.

> Each phrase must be between 1 and 60 bytes, inclusive.

> UTF-8 characters will match exactly

> URLs are considered words for the purposes of matches which means that the *entire domain and path must be included* in the track query for a Tweet containing an URL to match. Note that display_url does not contain a protocol, so this is not required to perform a match.

> Twitter currently canonicalizes the domain “www.example.com” to “example.com” before the match is performed

> Finally, to address a common use case where you may want to track all mentions of a particular domain name (i.e., regardless of subdomain or path), you should use “example com” as the track parameter for “example.com” (notice the lack of period between “example” and “com” in the track parameter). This will be over-inclusive, so make sure to do additional pattern-matching in your code

#### Tracking URL

[Standard stream parameters — Twitter      Developers](https://developer.twitter.com/en/docs/tweets/filter-realtime/guides/basic-stream-parameters)

### Premium Enrichment
*premium/enterprise*
[Premium enrichments, Overview](https://developer.twitter.com/en/docs/tweets/enrichments/overview)

> Premium enrichments are additive metadata included in the response payload of some of the data APIs. They are available in paid subscription plans only.

*???* What does /some/ mean?

* /Expanded and Enhanced URLs/:  Automatically expands shortened URLs (e.g., bitly) that are included in the body of a Tweet and provides HTML Title and Description metadata from the destination page.
* /Matching rules object/:	Indicates which rule or rules matched the Tweets received. The object returns rule tag and rule ID in the response object.
* /Klout/	Provides valuable information about a user’s influence (score) and influencer/interest areas (topics).
* /Poll metadata/: Notes the presence of the poll in a Tweet, includes the list of poll choices, and includes both the poll duration and expiration time.
* /Profile geo/: Derived user profile location data including the [longitude, latitude] coordinates (where possible) and related place metadata.

## Appendix
### Terms

*Twitter entities*

> The entities object encompasses common Tweet elements such hashtags, urls, mentions, and even polls.
See [Entities object](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/entities-object)

*Twitter extended entities*

> The extended entities is the go-to object for working with *native Twitter media*. Native media includes the types of media you can 'attach' while composing a Tweet. This includes up to four photos, or a single video or single animated GIF.

See [Extended entities object](https://developer.twitter.com/en/docs/tweets/data-dictionary/overview/extended-entities-object)

### Pricing
[Apply for Enterprise access](https://developer.twitter.com/en/enterprise-application)
