# Async

```admonish warning
**This page is imcomplete.**
```

## `async`, `.await` & `impl Future`

In Rust, asynchronous functions are defined by proceeding a synchronous function
definition with `async` keyword which turns the return value into an
`impl Future<Output = SyncReturnType>`, and are consumed by `.await`-ing it.

```rust
use trpl::Html;

//                             -> impl Future<Output = Option<String>>
async fn page_title(url: &str) -> Option<String> {
    let response = trpl::get(url).await;  // awaiting for an async function
    let response_text = response.text().await;
    Html::parse(&response_text)
        .select_first("title")
        .map(|title_element| title_element.inner_html())
}
```
