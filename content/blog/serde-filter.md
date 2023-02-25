---
title: "serde-filter (draft)"
date: 2022-03-05T11:29:09+01:00
draft: true
summary: Generic filtering abstractions over json objects (built on top of serde_json::Value)
---

#### Idea

_serde_ is one of the most powerful tools in any Rustaceans toolbelt. The JSON abstractions built on top of serde, _serde_json_, provide an intuitive API for _ser/der_ operations targeting JSON objects, handling, and parsing JSON objects. However, in some cases, such as deeply nested objects, or, non-uniform nesting, parsing serde_json Value's can be cumbersome. From my experience, there doesn't seem an intuitive way to construct composable filters that can be utilized to parse/skim objects for values you need/want, or ignore others you don't.

#### Motivating Example

Imagine we are astrophysicists, and we are using NASA's Open APIs to collect training data for a new deep learning model we are building. The Open APIs are great, but many of them return much more data than we actually need in our training/holdout sets. Instead of saving all of the key value pairs from the server's response, we only really need the values for "activeRegionNum". Let's use serde_filter to achieve it!

```Rust
// serde_filter core traits and filters
use serde_filter::{filters::*, prelude::*};
// OpenAPI clients
use nerva::clients::donki::flr::*;
use nerva::prelude::*;

fn main() where {
    // Setup an OpenAPI client
    let flr = FLR::default();
    // Setup a query
    let query = FLRParams::default();
    // Get a JSON response from the API
    let json = flr.get(query).unwrap();
    // Use serde_filter to filter for the 'activeRegionNum' key
    // returns a Result<Vec<u64>, anyhow::Error>
    let nums = filter::<Match<u64>>(json, &Match::new("activeRegionNum"));
    // Do something with 'nums' vector
    if let Ok(values) = nums {
        println!("{:#?}", values);
    }
}
```

#### More on Match/Ignore

_serde-filter_ ships with two powerful filters out of the box, the _Ignore_ filter and the _Match_ filter. We touched on the _Match_ filter above, and used it to trim a large, nested JSON response into a u64 vector containing only the values we need. But in case it wasn't clear enough of an example, let's look at another.

```Rust
use serde_json::json;
use serde_filter::{prelude::*, filters::*};

fn main() where {
    // Construct some JSON to match against
    let json = json!({
        "Object": {
            "explanation": "test explanation",
            "activeRegionNum": 23
        },
        "2022-01-11": {
            "Object2": {
                "explanation": "none",
                "activeRegionNum": 98
            }
        }
    });
    // Match on 'activeRegionNum'
    let nums = filter::<Match<u64>>(json, &Match::new("activeRegionNum")).unwrap();
    // The resultant vector contains only 'activeRegionNum' values
    assert_eq!(vec![23 as u64, 98 as u64], nums);
}
```

The _Ignore_ filter is simply _Match_'s opposite. The Ignore filter accepts a vector of keys that it will ignore - trimming the JSON response down to only the keys that were not ignored. Imagine we are still handling the example JSON defined above, and let's see how we can ignore the 'explanation' key.

```Rust
let json = json!({
    "explanation": "test",
    "media_type": "test",
    "hdurl": "test",
    "service_version": "test",
    "code": 200,
    "msg": "test"
});
let trimmed = filter::<Ignore>(json, &Ignore::new(vec!["explanation", "media_type"]));
if let Ok(trimmed) = trimmed {
    assert!(trimmed[0].get("explanation").is_none());
    assert!(trimmed[0].get("media_type").is_none());
}
```
