---
title: Transferring 941 pages on content to Framer
description:
published: false
date: 2024-05-02 16:11:05 +0800
categories: [web archiving, web scraping]
tags: [archivebox]
pin: false
math: false
mermaid: false
image:
  path:
  lqip:
  alt:
---

I have been using [ArchiveBox](https://github.com/ArchiveBox/ArchiveBox/wiki) to archive webpages for a client who runs a CPA firm. He wanted to move his website on over to Framer. In all the original site included a collection of 941 pages of service pages, tax resources, and guides.

I used **wget** to download the pages and **archivebox** to archive them. I then used the `archivebox export` command to export the archive to a folder. The folder contains an `index.html` file that lists all the pages in the archive. I wanted to transfer the archive to Framer so that I can view the pages in a more interactive way.

## Keypoints

- Use `wget` to crawl the website and generate a list of URLs.
- Use `archivebox` to archive the pages.
-
