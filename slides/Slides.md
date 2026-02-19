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
.bold-selected li:last-child {
  font-family: 'San Francisco bold';
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

- Base images: <span style="font-family: San Francisco bold">Ubuntu Jammy</span>, <span style="font-family: San Francisco bold">Ubuntu Noble</span>, or <span style="font-family: San Francisco bold">UBI 8-10</span>
- Four run image sizes: <span style="font-family: San Francisco bold">static, tiny, base & full</span>
- Languages: <span style="font-family: San Francisco bold">Java, GraalVM, Scala, Kotlin, Clojure, Go, .NET, Node.js, Python, Ruby, Rust, PHP & web servers</span>
- Builders with and without buildpacks

</div>

<div style="position: absolute; top: 280px; right: 100px; z-index: 10;">

![drop-shadow width:7em](https://paketo.io/v2/images/logo-paketo-dark.svg)

</div>

<!--
Paketo provides you with a complete set of everything you need to run Cloud Native buildpacks.

This includes:

- A choice of base images. These provide the foundation images used for your build and run environments. Paketo provides some choices so you can pick what works best for your company.

- If using the Ubuntu images, we provide four different size run images. These are the images where your application will run. The four different sizes allow you to choose the base size of your application images and what will be included in those images.

- Paketo provides support for a large number of popular languages.

- Paketo provides builders. Builders are the image that you point your build tool too. It's essentially the combination of base images + a set of buildpacks. Paketo provides builders for each of the base image sets listed above and includes the supported set of buildpacks for each builder. We also ship what are called "buildpack-less" builders, which are the same base images but without any buildpacks. There are some special cases when you're customizing buildpacks, where these buildpack-less builders can be helpful.
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

![bg left:40%](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/sean-pollock-PhYq704ffdA-unsplash.jpg)

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

&nbsp;

Spring Boot

- https://start.spring.io/
- `./mvnw spring-boot:build-image` 
or `./gradlew bootBuildImage`

</div>
<div class="column center-img" style="margin-top: 1em;">

![drop-shadow width:8em](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/choose31.jpg)

</div>
</div>

<!--
For most users, you'll install pack. That can be done using a variety of package manager across MacOS, Windows and Linux, plus with bash, fish, and zsh, you can also set up auto completions. It's also distributed as a container itself, if that's more convenient.

If you're a Spring Boot user, you're in luck! The Spring team provides Maven and Gradle plugins so you get support out-of-the-box. If you create your Spring project on https://start.spring.io/, things will be automatically set up and you can just run the appropriate command to trigger a build.
-->

---

# Let's Build!


<div class="columns">
<div class="column">

Run `./mvnw spring-boot:build-image` to start the build.

</div>
<div class="column center-img">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/build-start.png) 

</div>
</div>

<!--
Ok, so let's walk through a build! To get things started, we'll kick off the build using Maven. The process would be very similar if we used Gradle. If using `pack`, it'll be similar too but with a couple differences that I'll note during the build.

In this case, we see that Maven starts with a normal build compiling code & running your tests. Nothing new to see here.
-->

---

# Let's Build!


<div class="columns">
<div class="column" style="flex: 3">

Maven builds a JAR &
starts buildpacks

</div>
<div class="column" style="flex: 7">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/build-jar-and-container-start.png) 

</div>
</div>

<!--
After tests run, Maven will create a JAR file. The JAR file is used as input to the container build process with Paketo Buildpacks, and we can see the Cloud Native Buildpacks build start right after.

The first thing you'll see the CNB build process do is to pull the required images. We see it pull a builder, a run image, and the lifecycle. It will optionally pull cache information, if it exists.
-->

---

# Let's Build!


<div class="columns">
<div class="column" style="flex: 3">

Next creator starts. 

It analyzes &
 runs detect.

</div>
<div class="column" style="flex: 7">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/build-start-creator-and-detect.png) 

</div>
</div>

<!--
The next step is for the lifecycle to run. The lifecycle has two different modes in which it can run, and in this case we see it running creator which essentially runs all of the build steps in one container.

The lifecycle runs through the five stages of Cloud Native buildpacks. On this screen, we can see the first three.

1. Analyzing. In this stage, the lifecycle will look at the information that it has from previous runs. In this case, it's our first run so we don't see it doing anything.

2. Next we see the detection step. This is where all of the detect executable runs for each of the buildpacks. Buildpacks' detection executables all run in parallel and each buildpack outputs what is called a build plan. When all of the buildpacks are done, the lifecycle reviews the build plans and using the rules defined in the CNB specification selects a build plan to execute. Conversely, if it cannot select a build plan that satisfies all the rules, then it will fail.

3. Lastly, on this screen, we can see the restore stage. Information from analyze and detect is combined, and this is where metadata and cached layers are restored and made available to the next step: build.
-->

---

# Let's Build!


<div class="columns">
<div class="column" style="flex: 4">

Then buildpacks run!

</div>
<div class="column" style="flex: 6">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/build-runs.png) 

</div>
</div>

<!--
This is where the buildpacks actually run. It's called the "build" stage. In it you will see the selected buildpacks run in the order defined by their build plan. This is the build plan that was selected in the detect stage.

The build packs run and get access to the application code as well as any information restored from previous runs.

The buildpacks themselves do not actually create any OCI images or layers, instead, they are responsible for creating a directory structure on disk and populating it with the files and information used to create the final OCI image and all its layers. This makes authoring buildpacks much easier, as all a buildpack needs to do is create/move files around on disk.
-->

---

# Let's Build!

<div class="columns">
<div class="column" style="flex: 3">

Lastly, an image is exported.

</div>
<div class="column" style="flex: 7">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/build-exports.png) 

</div>
</div>

<!--
The fifth and final stage of the lifecycle is to export the image. This process takes the specially crafted directory structure generated by the buildpacks and turns this into an OCI image. You can see in the output the image layers that are being created, and you can also see other metadata being written to the image, like labels.

Finally, you'll see it write out the cache information that can be used by subsequent builds. This information does not end up in the application image though.
-->

---

# Demos

<!--
Run through the following demos:

- Spring Boot app build from source
- Spring Boot app build from jar and customization some settings like JVM version & vendor
- Static HTTP apps
- Python app, show how they’re all similar
- Maybe something with a custom start command
-->

---

# Adoption Plan

<div class="columns">
<div class="column bold-selected">

1. What languages do you need
to support?

</div>
<div class="column">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/language-word-cloud.png)

</div>
</div>


<!--
Now that we've seen Cloud Native buildpacks & Paketo in action, let's take a step back. When I'm preparing a new application, I have a set of a few questions I'll walk through to plan out the process.

The first question is regarding programming languages that you need to support. I love Paketo, which supports Java, GraalVM, Scala, Kotlin, Clojure, .NET, Python, Node.js, Go, Rust, Ruby, PHP, and Web Servers. Make a list of the languages you need to support.

If you need support for languages not in this, then you will want to check out the [Buildpack Registry](http://registry.buildpacks.io/), where you can look for existing buildpacks that support the languages you require. If you can't find any buildpacks that support your language, then you would need to create one to support that language or not use buildpacks for that language.
-->

---

# Adoption Plan

<div class="columns">
<div class="column" style="flex: 4">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/which-builder.png)

</div>
<div class="column bold-selected" style="flex: 3">

1. What languages do you need to support?
2. What builder should you use? 

</div>
</div>

<!--
Next, you'll want to select a builder. If you're company uses RedHat base images, then you'll want to go with one of the UBI builders, 8, 9 or 10. If you want to use Ubuntu base images, then you'll want one of our Ubuntu base images, either Jammy or Noble. From there, there are four different sized images you can choose from based on your needs. Generally, the smaller images are better, but like most things in tech, it depends.

Static is the smallest and works for Go & Rust apps. It only has CA certs & timezone data. It is around 5-8M.

Tiny is a little larger, including CA certs, timezone data, libc/libgcc, libstdc++, zlib, and openssl/libssl. This is suitable for Java, GraalVM, Scala, Kotlin, Clojure, Go, Rust, and Node.js applications. It is around 16M.

Base is larger but still quite reasonable. It includes all of time, more libraries, and is the smallest image that includes a shell. This is important to know for troubleshooting purposes. If you need to get into the container, then you need a shell. This image also includes `apt` so it works as a good base if you need to include additional libraries or packages. It supports languages Java, GraalVM, Scala, Kotlin, Clojure, .NET, Python, Node.js, Go, Rust, Ruby, and Web Servers. It is around 90M.

Full is the largest image, and has way too much stuff in it. Last I looked, it weighed in around 1G. It is not recommended that you use full unless you absolutely have to. If base is missing something you need, then using base and adding a few packages is the recommended path. That will result in a much smaller image.

For the time being, PHP apps do need full because of all their required OS level dependencies, however, the project is hoping to address this and support PHP on the base images.
-->

---

<div class="columns">
<div class="column bold-selected" style="flex: 3">

1. What languages do you need to support?
2. What builder should you use?
3. Do you need to add 
OS packages?

</div>
<div class="column" style="flex: 4">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/more-packages.png)

</div>
</div>

<!--
Another common issue is needing to install additional OS packages. This often comes in the form of needing additional dev libraries, like for a database, which are not present in the base images.

How you do this depends on your choice of base image. If you are using the Ubuntu base, then you can use the Paketo Apt buildpack. This buildpack is given an `Aptfile` with a list of packages to install. It will then install them into a buildpack layer and set env variables to make the packages accessible to your application. This is *NOT* installing them into the base layer, so this doesn't work for all packages but it does well with most things you need to install for buildpacks.

If you're using the UBI base images, then it's a little trickier. You would need to make a custom image, as we do not presently offer an RPM buildpack.
-->

---

# Adoption Plan

<div class="columns">
<div class="column" style="flex: 4">

![drop-shadow](https://raw.githubusercontent.com/dmikusa/effortless-containerization-with-cnbs/refs/heads/main/slides/img/tls-ca-certs.png)

</div>
<div class="column bold-selected" style="flex: 3">

1. What languages do you need to support?
2. What builder should you use?
3. Do you need to add 
OS packages?
4. Do you require Corporate CA Certs?

</div>
</div>

<!--

-->

---

<div class="columns">
<div class="column bold-selected" style="flex: 3">

1. What languages do you need to support?
2. What builder should you use?
3. Do you need to add 
OS packages?
4. Do you require Corporate CA Certs?
5. How to configure?

</div>
<div class="column" style="flex: 4">


</div>
</div>

<!--
Another common issue is needing to install additional OS packages. This often comes in the form of needing additional dev libraries, like for a database, which are not present in the base images.

How you do this depends on your choice of base image. If you are using the Ubuntu base, then you can use the Paketo Apt buildpack. This buildpack is given an `Aptfile` with a list of packages to install. It will then install them into a buildpack layer and set env variables to make the packages accessible to your application. This is *NOT* installing them into the base layer, so this doesn't work for all packages but it does well with most things you need to install for buildpacks.

If you're using the UBI base images, then it's a little trickier. You would need to make a custom image, as we do not presently offer an RPM buildpack.
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