## Pre-requisites

1. Install Hugo-extended v0.124 or later
2. Clone the repository
3. Run `git submodule update --init --recursive` to clone the theme submodule

## Directory Structure
Every section is a directory with an `_index.md` file that defines the section's title.

Pages within a section are markdown files in the section's directory.

```
content
└── docs
    ├── documentation
    │   ├── _index.md
    │   └── how_to.md
    └── some_other_section
        ├── _index.md
        ├── some_page.md
        └── some_other_page.md
```
    
## Add a Page

1. Create a .md file with the command
    ```
    hugo new docs/section/page.md
    ```

2. Set draft to false

## Serve the Site Locally
    hugo serve

## Build the Site
    hugo

The command generates the static site in the `public` directory.