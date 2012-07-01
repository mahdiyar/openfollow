openrealtime-net
================

API server to find people on the federated social web by email
address, Twitter handle, or Facebook ID.

License
-------

Copyright 2012, StatusNet Inc. and others.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

API
---

There are two main API endpoints.

* /v1/pubsub
  
  Takes a JSON document with a bunch of identifiers.
 
  Returns a JSON document that maps those identifiers to accounts with
  associated PubSubHubbub-enabled feeds.
  
  Identifiers can be:
  
  * Twitter handle like "@evanpro"
  * Email address like "evan@status.net"
  * Facebook profile URL like "http://facebook.com/evan.prodromou"
  * Google+ profile URL like "https://plus.google.com/104323674441008487802/posts"
  * Plain text string like "Evan Prodromou"
  
  Example:
  
  > [
  >    "@t",
  >    "evan@status.net",
  >    "http://facebook.com/faddah",
  >    "https://plus.google.com/113651174506128852447/posts",
  >    "Jan-Christoph Borchardt"
  > ]
  
  returns
  
  > {
  >    "@t": ["http://tantek.com/"],
  >    "evan@status.net": ["http://identi.ca/evan", "http://evanpro.tumblr.com/"],
  >    "http://facebook.com/faddah": [],
  >    "https://plus.google.com/113651174506128852447/posts": ["http://blog.romeda.org/"],
  >    "Jan-Christoph Borchardt": ["https://joindiaspora.com/u/jancborchardt"]
  > }

  Note: every ID is returned, even if we don't have info on it.
  
  Every result would return as an array, even if there's only one element.
  
* /v1/search

  Takes a single string JSON document, and returns a list of known accounts.
  
  Single string will be Facebook, Twitter, or Google+ ID, email or plain-text name.
  
  Results will be a JSON document that's an array of strings, one string for each known related ID.
  
  So this request:
  
  >   "Jan-Christoph Borchardt"
     
  ...would return this result:
  
  >    [
  >      "https://joindiaspora.com/u/jancborchardt",
  >      "https://twitter.com/jancborchardt",
  >      "http://jancborchardt.net/",
  >      "http://janinamerica.tumblr.com/"
  >    ]
