---
weight: 1
title: "Setup Development Environment"
description: ""
icon: "article"
date: "2024-03-31T10:35:55+08:00"
lastmod: "2024-03-31T10:35:55+08:00"
draft: false
toc: true
---

1. Install Python 3.11.*

2. Clone the repository:

    ```bash
    git clone git@github.com:Grapycal/GrapycalPrivate.git
    cd GrapycalPrivate
    git checkout dev
    ```

3. Install dependencies:

    ```bash
    scripts/dev_setup.sh
    ```

4. Run the development server:

    ```bash
    scripts/dev.sh
    ```
