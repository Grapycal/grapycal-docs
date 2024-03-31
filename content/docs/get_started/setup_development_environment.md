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
    git submodule update --init --recursive
    git checkout dev
    ```

3. Install grapycal backend and grapycal-builtin as editable packages:

    ```bash
    pip install -e backend -e extensions/grapycal_builtin
    ```

4. Install the frontend:

    ```bash
    cd frontend
    npm install
    cd ..
    ```

5. Start the backend:

    By running grapycal in `extensions` directory, you can import every extensions there locally.

    ```bash
    cd extensions/
    grapycal --no-http
    ```

6. Start the frontend server in another terminal:

    ```bash
    cd frontend
    npm run app
    ```