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
![bg left:40%](https://raw.githubusercontent.com/dmikusa/container-deep-dive-slides/refs/heads/main/slides/img/nikko-osaka-WzZjyThDoR8-unsplash.jpg)

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

![drop-shadow height:12em](https://raw.githubusercontent.com/dmikusa/container-deep-dive-slides/refs/heads/main/slides/img/qr-code.png)

</div>
<div style="text-align: center; padding-top: 0.5em;">

[https://github.com/dmikusa/container-deep-dive-slides]()

</div>

<!--
Slides are available at the link above.
-->

---

#

---

# 

---

# Slides

<div class="center-img">

![drop-shadow height:12em](https://raw.githubusercontent.com/dmikusa/buildpacks-for-ops/refs/heads/main/slides/img/qr-code.png)

</div>
<div style="text-align: center; padding-top: 0.5em;">

[https://github.com/dmikusa/buildpacks-for-ops]()

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