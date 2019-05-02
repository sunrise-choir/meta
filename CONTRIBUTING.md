# Contributing to the Sunrise Choir

Thanks for your interest in contributing to the Sunrise Choir! :sunrise: :pray: :notes:

This project is open commons that anyone can improve, stewarded by the Sunrise Choir team. :smiley_cat:

We welcome all contributions, such as but not limited to:

- designs
- specifications
- documentation
- tests
- websites
- artwork
- code

#### Table Of Contents

- [Code of Conduct](#code-of-conduct)
- [What should I know before I get started?](#what-should-i-know-before-i-get-started)
  - [Sunrise Choir ecosystem](#sunrise-choir-ecosystem)
  - [Sunrise Choir design decisions](#design-decisions)
- [How can I contribute?](#how-can-i-contribute)
  - [Bug reports](#bug-reports)
  - [Feature requests](#feature-requests)
  - [Code contributions](#code-contributions)
- [Style guides](#style-guides)
  - [Git Commit Messages](#git-commit-messages)
  - [Rust style guide](#rust-style-guide)
  - [JavaScript style guide](#javascript-style-guide)
  - [Specs style guide](#specs-style-guide)
  - [Documentation style guide](#documentation-style-guide)
- [Additional Notes](#additional-notes)

## Code of Coundect

 Anyone who interacts with the Sunrise Choir in any space, including but not limited to our GitHub repositories, must follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## What should I know before I get started?

TODO

### Sunrise Choir ecosystem

TODO

### Sunrise Choir design decisions

TODO

## How can I contribute?

### Bug reports

Have a look at the issue tracker for the relevant repository. If you can't find an issue (open or closed) describing your problem (or a very similar one) there, please open a new issue with the following details:

- Which versions are you using?
- What are you trying to accomplish?
- What is the full error you are seeing?
- How can we reproduce this?
  - Please quote as much of your code as needed to reproduce (best link to a public repository or [Gist](https://gist.github.com/)
  - Please post as much of your database schema as is relevant to your error 

Thank you! We'll try to respond as quickly as possible.

### Feature requests

If you can't find an issue (open or closed) describing your idea, open an issue. Adding answers to the following questions in your description is helpful:

- What do you want to do, and how do you expect the Sunrise Choir to support you with that?
- How might this be added to the Sunrise Choir?
- What are possible alternatives?
- Are there any disadvantages?

Thank you! We'll try to respond as quickly as possible.

### Code contributions

**Before you are able to submit code changes to the Sunrise Choir, you must first sign a [Contributor License Agreement](https://github.com/sunrise-choir/meta/blob/master/processes/cla.md)!** :heart:

Please be sure to follow the relevant [Style Guide(s)](#style guide).

### Style Guide

### Git commit messages

TODO

### Rust style guide

We follow the [Rust Style Guide](https://github.com/rust-lang-nursery/fmt-rfcs/blob/master/guide/guide.md), enforced using [rustfmt](https://github.com/rust-lang-nursery/rustfmt).

To run rustfmt tests locally:

1. Use rustup to set rust toolchain to the version specified in the repository's `rust-toolchain` file.

2. Install the rustfmt and clippy by running

```
rustup component add rustfmt
rustup component add clippy
```

3. Run clippy using cargo from the root of your repo.

```
cargo clippy
```

Each Pull Request needs to compile without warning.

4. Run rustfmt using cargo from the root of your repo

To see changes that need to be made, run

```
cargo fmt --all -- --check
```

If all code is properly formatted (e.g. if you have not made any changes), this should run without error or output.

If your code needs to be reformatted, you will see a diff between your code and properly formatted code.  If you see code here that you didn't make any changes to then you are probably running the wrong version of rustfmt.

Once you are ready to apply the formatting changes, run

```
cargo fmt --all
```

You won't see any output, but all your files will be corrected.

### JavaScript style guide

We follow the [JavaScript Standard Style](https://standardjs.com/), enforced using [`standard`](https://www.npmjs.com/package/standard).

### Specs style guide

We use [Markdown](https://daringfireball.net/projects/markdown) for our specifications.

TODO

### Documentation style guide

We use [Markdown](https://daringfireball.net/projects/markdown) for our documentation.

TODO

## Additional Notes

TODO
