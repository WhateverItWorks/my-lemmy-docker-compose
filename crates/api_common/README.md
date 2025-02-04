lemmy_api_common
===

This crate provides all the data types which are necessary to build a client for [Lemmy](https://join-lemmy.org/). You can use them with the HTTP client of your choice.

Here is an example using [reqwest](https://crates.io/crates/reqwest):

```rust
    let params = GetPosts {
        community_name: Some("asklemmy".to_string()),
        ..Default::default()
    };
    let client = Client::new();
    let response = client
        .get("https://lemmy.ml/api/v3/post/list")
        .query(&params)
        .send()
        .await?;
    let json = response.json::<GetPostsResponse>().await.unwrap();
    print!("{:?}", &json);
```

As you can see, each API endpoint needs a parameter type ( GetPosts), path (/post/list) and response type (GetPostsResponse). You can find the paths and parameter types from [this file](https://github.com/LemmyNet/lemmy/blob/main/src/api_routes_http.rs). For the response types you need to look through the crates [lemmy_api](https://github.com/LemmyNet/lemmy/tree/main/crates/api/src) and [lemmy_api_crud](https://github.com/LemmyNet/lemmy/tree/main/crates/api_crud/src) for the place where Perform/PerformCrud is implemented for the parameter type. The response type is specified as a type parameter on the trait.

For a real example of a Lemmy API client, look at [lemmyBB](https://github.com/LemmyNet/lemmyBB/tree/main/src/api).

Lemmy also provides a websocket API. You can find the full websocket code in [this file](https://github.com/LemmyNet/lemmy/blob/main/src/api_routes_websocket.rs).
