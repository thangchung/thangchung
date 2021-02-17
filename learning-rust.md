# Learning resources
- https://doc.rust-lang.org/stable/book/
- https://rust-lang.github.io/async-book/
- https://github.com/rust-unofficial/patterns
- https://aml3.github.io/RustTutorial/html/toc.html
- https://github.com/inancgumus/learnrust
- https://nnethercote.github.io/perf-book/title-page.html
- https://github.com/RustBeginners/awesome-rust-mentors

# Style guide
- https://pingcap.github.io/style-guide/rust/

# Articles
- https://www.possiblerust.com/
- https://github.com/sidkshatriya/me/blob/master/001-rd-intro.md
- What is &~#@&lt;!? in Rust -> https://www.slideshare.net/DavidEvansUVa/what-the-lt-pointers-in-rust
- https://chrismorgan.info/blog/rust-ownership-the-hard-way/

# Presentation:
- The first presentation about Rust by Graydon -> http://venge.net/graydon/talks/intro-talk-2.pdf

# Cheatsheets
- https://github.com/rust-unofficial/awesome-rust
- https://github.com/ctjhoa/rust-learning
- https://cheats.rs/

# Project structure and samples
- https://github.com/LukeMathWalker/build-your-own-jira-with-rust

# Microservices
- http://www.goldsborough.me/rust/web/tutorial/2018/01/20/17-01-11-writing_a_microservice_in_rust/

```rust
use hyper::service::{make_service_fn, service_fn};
use hyper::{Body, Request, Response, Server};
use std::{convert::Infallible, net::SocketAddr};

#[derive(Clone, Copy)]
struct ApiServer {}

impl ApiServer {
    pub async fn route(&mut self, req: Request<Body>) -> Result<Response<Body>, Infallible> {
        let mut response = Response::new(Body::empty());
        *response.body_mut() = req.into_body();

        Ok(response)
    }
}

#[tokio::main]
async fn main() {
    let mut api = ApiServer {};

    let make_svc = make_service_fn(move |_conn| async move {
        Ok::<_, Infallible>(service_fn(move |req| async move { api.route(req).await }))
    });

    let addr = SocketAddr::from(([127, 0, 0, 1], 3000));

    let server = Server::bind(&addr).serve(make_svc);

    if let Err(e) = server.await {
        eprintln!("server error: {}", e);
    }
}
```
