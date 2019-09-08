---
title: Intro to SELinux 
tags: admin security SELinux Gentoo
article_header: 
  type: cover
  image:
    src: 
---

Welcome to my second blog post. I have been planning on doing a blog on SELinux
for some time now, however, I keep getting caught up with stuff. Finally I had
the time to sit down and work through this one. 

Originally, I started researching for this blog post because I was interested
in learning more about SELinux and how it works. This would satisfy portions of
the operations and security learning domains for the RHCSA/RHCE certifications. 

Just to make things more fun, I thought I would throw Gentoo into the mix.
Installing things on RHELS or CentOS is great, but installing things on Gentoo
is so much more fun. 

As a disclaimer, this is not a blog post about basic Gentoo stuff. If you need
to know how emerge works or anything else of the sort, please refer to another
tutorial. 

This article assumes some knowledge of Gentoo. 

---

First, what is SELinux? 

SELinux stands for Security Enhanced Linux. It was developed by RedHat and the
NSA in the late 90's and released to the public in 2000. 

SELinux takes advantage of a MAC (mandatory access control) system. On
a typical Unix system, the security implementation is based on a DAC
(discretionary access control) system, which means that that any owner of
a file has the ability to allow whatever access from anything else, to that
file. 

A mandatory access control system, like SELinux enforces security from a preset
premative, often called a policy. This policy will always override a DAC
policy. So if we had a user on the system named Paul, and Paul has allowed
another user, Jen to access his home directory. If the SELinux policy is set in
such that Jen is not allowed to access Pual's home directory, this policy will
override the access that Paul has given to his home directory. Making the words
work out very nicely, from discretionary to mandatory. I love it when words are
actually descriptive to operations and implementations. 

One of the features of SELinux is the type of enforcement that its capable
of. When you think about permissions, you often think of two domains, the first
domain is users, groups, and other. The second domain is read, write, and
execute (rwx) permissions on the previous three subjects. With this comes the
ability to allocate permissions accordingly depending on the user, the group
that the user is in and anyone on the system. The problem with discretionary
access control systems is that they are discretionary! For example, lets say we
make a group called "developers" and allow anyone in that group access to use
the compiler. You think that you are a smart sysadmin and this box is locked
down only to developers in the "developers" group...WRONG! Lets say that
someone in the developers group decides to copy all the compiler files into
their home directory. Now that person that has the compiler files in the home
directory, because of DAC, can now allow access to any user that is not in the
developers group. This allows very easy lateral movement and execution from
a malicious actor on the system. 

SELinux has much more granular control over the whole systems. It can specify
boundaries and interaction between files, users, processes, sockets etc. 

The way that SELinux enforces these policies is very complex, but the simple
explanation that I will cover in this article is based on context enforcing.
The context enforcing utalizes 3 and sometimes 4 parts:

- SELinux user (not the same as regular user)
- SELinux role
- SELinux type
- the fourth is sensitivity (which uses the mls domain, not covered in this
  article)

What this looks like if you were to do an ls -laZ on a root directory in RHELs:

```
-rw-------. root    root    system_u:object_r:admin_home_t:s0 anaconda-ks.cfg
```



<!--more-->
