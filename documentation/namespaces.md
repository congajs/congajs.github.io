---
layout: documentation
title: Namespaces - CongaJS Documentation - Congajs.com
nav: documentation
---

# Namespaces

CongaJS has a built in namespace system to handle the dynamic loading of modules, templates, etc. across
multiple bundles within a project.

Various configuration options expect namespaces for file paths and transform them accordingly.

Example:

    my-bundle:filter/my-filter

    // translates to
    /path/to/my-bundle/lib/filter/my-filter.js

