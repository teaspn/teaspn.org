---
layout: default
title: Home
nav_order: 1
description: "TEASPN: Framework and Protocol for Integrated Writing Assistance Environments"
permalink: /
---

![TEASPN logo](logo.png)

Framework and Protocol for Integrated Writing Assistance Environments
{: .fs-6 .fw-300 }

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [View it on GitHub](https://github.com/pmarsceill/just-the-docs){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## What is TEASPN

TEASPN (Text Editing Assistance Smartness Protocol for Natural Language; pronounced "teaspoon") is a protocol and a framework for providing integrated writing assistance environments. It defines a set of APIs that writing software (e.g., text editors) uses to communicate with language smartness providers (e.g., grammatical error correction systems).

Traditionally, it takes great effort for developers to integrate language smartness features (such as spell correction and completion) into writing software. Also, there's been great progress on writing assistance technologies such as [grammatical error correction](http://nlpprogress.com/english/grammatical_error_correction.html) and text generation/completion in academia, although real-world applications are yet to reap the benefit from these research efforts, mainly due to the high cost of implementing them in a user-facing way. By adopting Teaspn,

* users (writers) can benefit from integrated writing experience provided by text editors and language smartness providers, 
* developers of writing software can integrate state-of-the-art language smartness functionalities into their editors and word processors just by following the protocol, and 
* developers of language technologies (e.g., NLP researchers) can integrate their state-of-the-art language smartness technologies to major writing software without worrying about implementing user-facing features.

Teaspn is greatly inspired by and built upon [Language Server Protocol (LSP)](https://microsoft.github.io/language-server-protocol/), which defines the protocol used by programming software (e.g., IDEs, or integrated development environments) and servers that provide language features (e.g., completion). You can think of TEASPN as a "fork" of LSP that is specifically designed for editing natural language text.


## Specifications

Just the Docs is built for [Jekyll](https://jekyllrb.com), a static site generator. View the [quick start guide](https://jekyllrb.com/docs/) for more information. Just the Docs requires no special Jekyll plugins and can run on GitHub Pages' standard Jekyll compiler.

### Quick start: Use as a GitHub Pages remote theme

1. Add Just the Docs to your Jekyll site's `_config.yml` as a [remote theme](https://blog.github.com/2017-11-29-use-any-theme-with-github-pages/)
```yaml
remote_theme: pmarsceill/just-the-docs
```
<small>You must have GitHub Pages enabled on your repo, one or more Markdown files, and a `_config.yml` file. [See an example repository](https://github.com/pmarsceill/jtd-remote)</small>

### Local installation: Use the gem-based theme

1. Install the Ruby Gem
```bash
$ gem install just-the-docs
```
```yaml
# .. or add it to your your Jekyll site’s Gemfile
gem "just-the-docs"
```
2. Add Just the Docs to your Jekyll site’s `_config.yml`
```yaml
theme: "just-the-docs"
```
3. _Optional:_ Initialize search data (creates `search-data.json`)
```bash
$ bundle exec just-the-docs rake search:init
```
3. Run you local Jekyll server
```bash
$ jekyll serve
```
```bash
# .. or if you're using a Gemfile (bundler)
$ bundle exec jekyll serve
```
4. Point your web browser to [http://localhost:4000](http://localhost:4000)

If you're hosting your site on GitHub Pages, [set up GitHub Pages and Jekyll locally](https://help.github.com/en/articles/setting-up-your-github-pages-site-locally-with-jekyll) so that you can more easily work in your development environment.

---

## About the project

Just the Docs is &copy; 2017-2019 by [Patrick Marsceill](http://patrickmarsceill.com).

### License

Just the Docs is distributed by an [MIT license](https://github.com/pmarsceill/just-the-docs/tree/master/LICENSE.txt).

### Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change. Read more about becoming a contributor in [our GitHub repo](https://github.com/pmarsceill/just-the-docs#contributing).

### Code of Conduct

Just the Docs is committed to fostering a welcoming community.

[View our Code of Conduct](https://github.com/pmarsceill/just-the-docs/tree/master/CODE_OF_CONDUCT.md) on our GitHub repository.
