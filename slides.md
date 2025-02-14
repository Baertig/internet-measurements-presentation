---
# You can also start simply with 'default'
theme: default
# some information about your slides (markdown enabled)
title: Quic server preferred address
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
hideInToc: true
colorSchema: light

fonts:
  # basically the text
  sans: Open Sans
---

## Anylayzing google's deployment of quic `server preferred address`

Georg Bär-Dumont // 18.02.2025

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: quote
hideInToc: true
---

*"So far, only [Google’s Media CDN]{v-mark.red.highlight="1"} has widely enabled advertising an alternative address, but we expect more servers to adopt it soon. Testing has shown that this migration is successful over 99% of the time in Chrome and reduces average [RTT by 40-80%]{v-mark.red.underline="2"}."*

-- [Chromium Blog Post](https://blog.chromium.org/2024/12/making-chrome-quicer.html)

---
hideInToc: true
---

# Outline


<Toc text-sm />

---
layout: two-cols-header
---

# What is the **server preferred addresse** parameter? 

::left::

- *transport parameters* are exchange during handshake
- **only** the server can send the `preferred_address` parameter
- *"the client* **SHOULD** *select one of the two addresses provided by the server..."* -- RFC 9000 9.6
- enables connection migration, that is initiated by the server

::right::

<div class="flex w-full h-full items-center justify-center">
  <img src="./assets/Wireshark_prefferred_address.png" style="width: 350px"/>
</div>

<style>
.two-cols-header {
  grid-template-rows: auto 1fr;
  gap: 8px;
}
</style>


---

# Why use server preferred address ?


<div class="flex pt-15 items-center justify-center">
  <img src="./assets/quic_handshake.png" style="width: 500px"/>
</div>

[https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhPbECfw3krb6-1DM-wQBsBpPLhcLNGwGVMzEFW_XzUpumvfQzJSvVLJfZ1iwCyowq9QRE2bwl-GsQ8eArforqyyEBadmNN2iwUP59p3Rl428qWPqaJFu2JYe9o7QsuWa20R1s_isnM7efkNIMSetkmnyhFuQOtRt1-7G_e4NNb-BSfXP-DhVB_X5c6QZ0V/s1600/Screenshot%202024-12-17%2012.07.47%20PM.png]{style="font-size: 8px; line-height: normal; margin-bottom: 85px"}

<style>
.slidev-layout p {
  opacity: 1.0
}
</style>

---

# Experiment Setup: server preferred address detection

<div class="p-10"/>

1. Send requests to Google Media CDN i.e. Watch <logos-youtube />
2. Capture TLS Handshake Traffic 
3. Analyze Quic Transport Parameters

<div class="p-10"/>

**Expected Result**: list of DNS names of CDN-Servers, that use server preferred address

(potential) **Next Step**: Measure difference in performance of using the preferred address vs. using the original address

---

# Building a Youtube Crawler

<div class="flex pt-4 items-center justify-center">
  <img src="./assets/youtube_crawler.png" style="width: 580px;"/> 
</div>

---

# Results: Videos

100 videos

<div class="flex flex-row pt-15 items-center justify-center gap-32">
  <img src="./assets/like_and_view_count_complete.png" style="width: 300px;"/> 
  <img src="./assets/like_and_view_count_zoomed.png" style="width: 300px;"/> 
</div>

---

# Results: Transport params

<div class="flex flex-row items-end gap-32">
  <img src="./assets/quic_transport_parameters_in_server_hello.png" style="width: 300px;"/> 
  <img src="./assets/request_count_per_domain.png" style="width: 300px;"/> 
</div>

<div style="position: absolute; bottom: 94px; left: 110px; height: 15px; width: 200px" v-mark="{at: 1, color: 'red', type: 'circle'}" /> 

<div style="position: absolute; top: 118px; right: 250px; height: 20px; width: 200px" v-mark="{at: 2, color: 'red', type: 'circle'}" /> 

---

# Discussion

- No service uses it
- Meaning: Google tells lies.
- Caveat: I can only test from my own network (Vodafon)


---

---

# Sources

## Images
https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhPbECfw3krb6-1DM-wQBsBpPLhcLNGwGVMzEFW_XzUpumvfQzJSvVLJfZ1iwCyowq9QRE2bwl-GsQ8eArforqyyEBadmNN2iwUP59p3Rl428qWPqaJFu2JYe9o7QsuWa20R1s_isnM7efkNIMSetkmnyhFuQOtRt1-7G_e4NNb-BSfXP-DhVB_X5c6QZ0V/s1600/Screenshot%202024-12-17%2012.07.47%20PM.png, Access at 01.27.2025


