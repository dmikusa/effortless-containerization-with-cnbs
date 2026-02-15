---
title       : Effortless Containerization with Cloud Native Buildpacks
author      : Daniel Mikusa
description : 'Ready to reduce your container build complexity? Cloud Native Buildpacks automatically transform your source code into production ready container images with very little configuration. This session introduces CNB fundamentals through practical examples: local builds with Pack CLI, CI pipeline integration, and proven strategies to overcome common adoption challenges and streamline your containerization workflow.'
keywords    : cloud native, buildpacks, paketo
marp        : true
theme       : jobs
paginate    : true
---

<style>
.columns {
    display: flex;
}
.column {
    flex: 1;
}
.center-img img {
    display: block;
    margin-left: auto;
    margin-right: auto;
}
.only-img img {
    margin-top: 1.5em;
    display: block;
    margin-left: auto;
    margin-right: auto;
}
</style>

<!-- 
_class: titlepage
_footer: Photo by <a href="https://unsplash.com/@nosaka?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">nikko osaka</a> on <a href="https://unsplash.com/photos/brown-and-red-shipping-containers-WzZjyThDoR8?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a>
_paginate: false
-->
![bg left:40%](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/nikko-osaka-WzZjyThDoR8-unsplash.jpg)

# Effortless Containerization with Cloud Native Buildpacks

<!-- 
Welcome! This is my session. Let's get started!
-->

---

<div class="columns">
<div class="column">

## Daniel Mikusa

- Staff Software Engineer @ 7SIGNAL, Inc
- Paketo Steering Committee Member
- Cloud-Native Buildpacks Maintainer

### Contact Me

- <small>dan@mikusa.com</small>
- <small>https://github.com/dmikusa</small>
- <small>https://www.mikusa.com</small>

</div>
<div class="column center-img">

![drop-shadow width:10em](https://raw.githubusercontent.com/dmikusa/mcp-a-systems-integration-perspective/refs/heads/main/slides/img/7SIGNAL-LOGO-RGB-CLR-LGHT-BG.svg)
![drop-shadow width:10em](https://paketo.io/v2/images/logo-paketo-dark.svg)
![drop-shadow width:10em](https://buildpacks.io/images/buildpacks-logo.svg)

</div>
</div>

<!--
Who am I? I work at 7SIGNAL, a leader in WiFi optimization. In a nutshell, we write performance monitoring software that helps make your WiFi awesome.

I also help with a couple of OSS projects, Cloud-Native Buildpacks and Paketo Buildpacks. Both, great tools for building your container images and the subject of today's discussion.

While I'm not covering WiFi in this talk, if you're curious feel free to come chat after the session.
-->

---

# Slides

<div class="center-img">

![drop-shadow height:12em](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/qr-code.png)

</div>
<div style="text-align: center; padding-top: 0.5em;">

[https://github.com/dmikusa/effortless-containerization-with-cnbs]()

</div>

<!--
Slides are available at the link above.
-->

---

# Why are we here today?

<div class="columns margin3">
<div class="column">

1. A Quick primer
2. Getting Started
3. How to build containers
4. Adoption Plan

</div>
<div class="column">

![drop-shadow width:10em](https://buildpacks.io/images/buildpacks-logo.svg)
![drop-shadow width:10em](https://paketo.io/v2/images/logo-paketo-dark.svg)

</div>

---

# What are Cloud Native Buildpacks?

<div class="columns">
<div class="column">

<h2>What they are?</h2>
<ul>
    <li>Source Code => OCI</li>
    <li>Standard & specification</li>
    <li>Support Multiple Languages</li>
    <li>Integrated into Platforms</li>
</ul>

</div>
<div class="column">

<h2>What they're not?</h2>
<ul>
    <li>Total replacement for Dockerfiles</li>
    <li>An implementation</li>
    <li>Replacement for CI/CD</li>
    <li>Dev-only Tool</li>
</ul>

</div>
</div>

<!--
What they are:

- CNBs have a focused purpose, to turn source code into OCI images.
- They are a general standard and support many different languages.
- They are suited well for integration into platforms and are in many already (CF, Heroku, Fly.io, AWS AppRunner, Google Cloud Run)
- They are a specification (platform + buildpacks). It defines how buildpacks work and how you integrate them into your platforms.

What they are not:

- They do not attempt to completely replace Dockerfiles. There are still some good use cases for Dockerfiles. They do replace Dockerfiles for use when it comes to building application containers.
- They're not replacing CI/CD, although there are some common parts to both. Both build your app, both create artifacts. CNBs are usually integrated into your existing CI/CD pipelines some point after tests have run.
- They're not a dev-only tool. They're great for Ops folks too, and that's what we're talking about today.
- They're not an implementation. CNB does not provide a working set of buildpacks, which is where Paketo buildpacks come in. Paketo is an implementation of the CNB specification for most popular languages: Java, .NET, Python, Node.js, Ruby, Go, Rust, PHP, and Web Servers.
-->

---

# What are Cloud Native Buildpacks?

<div class="columns" style="margin-top: 1.5em">
<div class="column">

Technically:

- A build + detect executable 
- A metadata config file called `buildpack.toml`
- Bundled as an OCI image

</div>
<div class="column">

<div style="margin-top: 1.5em">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/buildpack-contents.png)

</div>

</div>

---

# What are Paketo Buildpacks?


<div class="columns">
<div class="column" style="margin-top: 3em; margin-left: 2em">

![drop-shadow width:7em](https://paketo.io/v2/images/logo-paketo-dark.svg)

</div>
<div class="column" style="margin-top: 1em; margin-left: -3em">

- Cloud Native Buildpacks 
Implementation
- Written in Go
- Open Source Apache v2
- Part of Cloud Foundry Foundation

</div>
</div>

<!--
Paketo Buildpacks are an implementation of the Cloud Native Buildpacks specification.
// TODO: finish
-->

---

# Paketo Provides

<div style="margin-top: 2em;">

## None

- Base images: <span style="font-family: San Francisco bold">Ubuntu Jammy</span>, <span style="font-family: San Francisco bold">Ubuntu Noble</span>, or <span style="font-family: San Francisco bold">UBI 8-10</span>
- Four run image sizes: <span style="font-family: San Francisco bold">static, tiny, base & full</span>
- Languages: <span style="font-family: San Francisco bold">Java, Java Native Image, Go, .NET, Node.js, Python, Ruby, Rust, & web servers</span>
- Buildpack-less builders too

</div>

<div style="position: absolute; top: 250px; right: 100px; z-index: 10;">

![drop-shadow width:7em](https://paketo.io/v2/images/logo-paketo-dark.svg)

</div>

<!--
Buildpacks can be customized in a number of ways to fit your needs, but let's start by covering what you get out-of-the-box without any customization, simply by using the Paketo Buildpacks implementation.
-->

---
<!-- 
_footer: Photo by <a href="https://unsplash.com/@punttim?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Tim Gouw</a> on <a href="https://unsplash.com/photos/man-wearing-white-top-using-macbook-1K9T5YiZ2WU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
-->

![bg right:40%](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/tim-gouw-1K9T5YiZ2WU-unsplash.jpg)

# Problems Solved: Dockerfiles

- Dockerfile copy & paste
- Dockerfile sprawl

<!--
You need to containerize an app. Where do you get the Dockerfile?
a.) Copy & Paste from another repo
b.) Copy & Paste from Stack Overflow or a blog post
c.) Let AI tools generate it

Most people don't have time to sit and generate one from scratch, check that it's following all the best practices (or even know of them), so any issues or problems get copied and pasted with the Dockerfile.

If you have enough applications at your company, then you hit a new issue. What happens when you want to update all of these unique Dockerfiles? You need to analyze them, because there may be differences in the files, and then buckle in to submit a whole bunch of pull requests. It's not a fun process.

Cloud Native buildpacks have no Dockerfiles, so this problem is gone. The knowledge and commands in the Dockerfiles are codified in the buildpacks & shared that way. 
-->

---

# Problems Solved: Best Practices

<div class="columns">
<div class="column">

<ul>
    <li>Base image too big</li>
    <li>Base image too small</li>
    <li>Base image not maintained</li>
    <li>Not your golden base image</li>
    <li>Forgets to use multi-stage builds</li>
    <li>Implements multi-stage incorrectly</li>
</ul>

</div>
<div class="column">

<ul>
    <li>Bad layer ordering (cache invalidation)</li>
    <li>Bad layer breakdown (decreases sharing)</li>
    <li>Runs the app as root</li>
    <li>No PID1 handler</li>
    <li>Forget company CA certs</li>
    <li>exec vs shell to start process</li>
</ul>

</div>
</div>

<!--
Dockerfiles are easy to write, but hard to get correct. A quick search will show you tons of articles talking about the "best practices" you should follow when writing a Dockerfile. There are dozens of them, some are obvious and some can be pretty subtle.

As a busy developer, you may be unaware of these best practices or you may not have the time to go through them all. As mentioned earlier, copy & paste is the most common way to create a Dockerfile, so you're just hoping someone else before you put in the time to do this right.

The great thing about Buildpacks is that you don't have to hope. The buildpack authors put in the time to get it right, and codify that knowledge so every time you run the buildpack, things are done correctly and you get the best possible image.
-->

---

<!-- 
_footer: Photo by <a href="https://unsplash.com/@seanpollock?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Sean Pollock</a> on <a href="https://unsplash.com/photos/low-angle-photo-of-city-high-rise-buildings-during-daytime-PhYq704ffdA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
-->

![bg right:40%](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/sean-pollock-PhYq704ffdA-unsplash.jpg)

# Enterprise Ready

Paketo Buildpacks Support

- Custom CA certificates
- Proxies for Internet access
- Local mirrors
- Air Gapped Environments
- SBOM Support

<!--
Buildpacks are Enterprise ready. Many enterprises need things like proxy support for access to the Internet, or possibly to support a custom CA cert chain because their network intercepts TLS traffic to decrypt it. Paketo Buildpacks supports all of this.

Some companies may not allow internet access at all, or certain parts of the company network may be air gapped. In these cases, Paketo has two options for you. You may run a local mirror for dependencies and a local registry for images. Another option that Paketo supports is to bundle dependencies into your buildpack images and use a local registry for images.

As supply chain issue increase, companies are investing more into provenance and SBOM support. Fortunately, for Paketo buildpacks you get that out-of-the-box. We'll see this more in the demos.
-->

---

# Getting Started

<div class="columns">
<div class="column">

Most Users
- [Install pack](https://buildpacks.io/docs/for-platform-operators/how-to/integrate-ci/pack/)
- Run `pack build`

</div>
<div class="column">

Spring Boot

- https://start.spring.io/
- `./mvnw spring-boot:build-image` or `./gradlew bootBuildImage`


</div>
</div>

<div style="position: absolute; top: 250px; right: 100px; z-index: 10;">

![drop-shadow width:7em](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/choose31.jpg)

</div>

<!--
For most users, you'll install pack. That can be done using a variety of package manager across MacOS, Windows and Linux, plus with bash, fish, and zsh, you can also set up auto completions. It's also distributed as a container itself, if that's more convenient.

If you're a Spring Boot user, you're in luck! The Spring team provides Maven and Gradle plugins so you get support out-of-the-box. If you create your Spring project on https://start.spring.io/, things will be automatically set up and you can just run the appropriate command to trigger a build.
-->

---

# Let's Build!

// TODO: insert image or output of a typical build


---
# 

---
# 

---
# 

---
# 

---
# 

---

# Slides

<div class="center-img">

![drop-shadow height:12em](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/qr-code.png)

</div>
<div style="text-align: center; padding-top: 0.5em;">

[https://github.com/dmikusa/effortless-containerization-with-cnbs]()

</div>

<!--
Slides are available at the link above.
-->

---

<style scoped>
img {
    padding-top: 1.5em;
    height: 300px;
    width: 300px;
}
</style>

# Questions?

<div class="only-img">

:thinking:

</div>