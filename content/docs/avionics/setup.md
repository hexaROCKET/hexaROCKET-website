+++
title = "Setup Toolchain"
description = "How to setup the software build toolchain for the avionics system."
date = 2023-01-16T08:00:00+00:00
updated = 2023-01-16T08:00:00+00:00
draft = false
weight = 202
sort_by = "weight"
template = "docs/page.html"

[extra]
lead = 'How to setup the software build toolchain for the avionics system.'
toc = true
top = false
+++

## Prerequisites

Given that we have decided to use the Rust programming language, we should probably 
[install rust](https://www.rust-lang.org/tools/install).

With rust installed, we will need to include the rp-2040 HAL crate in our `cargo.toml` file:

```toml
rp2040-hal = "0.7.0"
```

Additionally, we will need drivers for each of our hardware components outlined above:

```toml
stuff = "0.1.0"
stuff = "0.1.0"
stuff = "0.1.0"
stuff = "0.1.0"
stuff = "0.1.0"
```

It's this simplicity shown above that outlines the massive benefit the embedded rust HAL 
toolchain provides.




