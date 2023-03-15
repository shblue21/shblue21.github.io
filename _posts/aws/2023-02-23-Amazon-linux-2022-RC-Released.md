---
title: Amazon Linux 2022 RC Released
date: 2023-02-23T00:00:00
last_modified_at: 2023-03-14T00:00:00
categories:
  - aws
tags:
  - AWS
  - Amazon Linux
toc: true
lang: en
---

> ❗ Translations provided by machine translation.  
> Amazon Linux 2022 has been renamed.
> changed to AL2022 -> AL2023 on March 2. The release cycle also changed to every two years starting in 2023.

If you use AWS compute services, you're probably using Amazon Linux optimized for AWS.  
Development on Amazon Linux 2022 began in 2021, and it's now up to RC3.1, with GA coming soon.

There are many changes, including **support policies**, **Base OS**, and more, and in this post, I'll introduce you to the major changes in Amazon Linux 2022.  

## Key comparisons

The main differences include a change to the base OS, the addition of SELinux, and a more detailed support policy.  

| Changes | Amazon Linux 2 | Amazon Linux 2022 | 
| Release Date | June 2018 | In 2023 (expected) |
| EOL | June 30, 2025 | 2026
| Support Policy | 2 Years | 5 Years
| Kernel Version | 5.10 | 5.15 ~ |
| Underlying OS | Centos and several upstreams including | Fedora
| gcc | 7.3 | 11.2
| SELinux | X | O
| Package Manager | Yum | DNF  

## Support policies and versioning

Previously, Amazon Linux did not have a clear policy of extending support for two years as it approached EOL (AL1 extended to June 2023, AL2 extended to June 2025).  
However, with the announcement of Amazon Linux 2022, they announced that they will release a major update every two years, and each major update will be supported for five years.  

This is well documented in the official Guide documentation.  
![AL2022 Support](/img/230223_al2022_1.png){: width="70%" height="70%"}  
 
In the table above, Standard Support refers to support including minor updates, while Maintenance Support refers to support including security updates, critical bug fixes, and more. 

## SELinux
In AL2022, SELinux is enabled by default.  
SELinux, short for Security-Enhanced Linux, is an extension of the security features of the Linux kernel that complements the existing security features of the Linux kernel to enhance security.  
It provides context-based access control to enhance security, logs security violations, and more.

## In-Place Upgrade ?
In-Place Upgrade from Amazon Linux 2 -> Amazon Linux 2022 is most likely not supported. 

1. Amazon Linux 2022 is based on Fedora, so it is likely to be less compatible with Amazon Linux 2.
2. Since we did not support in-place upgrades from Amazon Linux 1 to Amazon Linux 2, it is unlikely that we will support in-place upgrades from Amazon Linux 2 to Amazon Linux 2022.

They did provide a tool to help us upgrade from Amazon Linux 1 to 2 [URL](https://github.com/amazonlinux/upgrade-modules), 
it wasn't very helpful and didn't support in-place upgrades.

--------
With RC deployments starting in July 2022 and the release just around the corner, it's probably a good idea to wait until GA if you need to re-provision your AWS infrastructure soon.