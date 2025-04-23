# dynpr

**dynpr** is a lightweight Ruby CLI tool for rendering dynamic text templates with double curly braces (`{{ }}`) used as placeholders. It was originally designed to streamline the process of generating prompts for large language models (LLMs), making it easy to build and manage a local library of reusable prompt templates.

With dynpr, you can define your LLM prompts as templates and inject arguments on the fly from the command line — perfect for quickly generating personalized or context-specific prompts from a central directory.

Templates can include required variables like `{{user}}` or optional ones with defaults like `{{env:development}}`. The script expects a single positional argument (assigned to `INPUT`), while all other variables are passed as keyword arguments using the `-key value` format.

Before using dynpr, make sure to set the `DYNPR_TEMPLATE_DIR` environment variable to point to your template folder. Template files are resolved relative to this directory.

The tool checks for missing required variables and reports redundant ones, ensuring your input always matches the structure of the template.
## Example

```bash
dynpr welcome.txt "John Doe" -user Alice -env production
```

This will render the `welcome.txt` template using the provided values, substituting the variables accordingly.

## Installation

Use the included `deploy.sh` script to install `dynpr` into your local bin directory:

```bash
./deploy.sh
```

Make sure to define your template directory in your shell configuration:

```bash
export DYNPR_TEMPLATE_DIR=~/my-templates
```

## Template Example

A simple template file might look like this:

```
Hello, {{user}}!
Running in {{env:development}} environment.
Input provided: {{INPUT}}
```

If no value is passed for `env`, the default `"development"` will be used.

## Zsh Tab Completion

Also add this to your `.zshrc` to enable TAB-completion of files from both the current working directory and the template directory (`DYNPR_TEMPLATE_DIR`):

```zsh
export DYNPR_TEMPLATE_DIR='<path_to_prompts_dir>'

_dynpr_completion() {
    local template_dir="${DYNPR_TEMPLATE_DIR}"
    local expl

    # Add current dir
    _files

    # Add template dir as alternate search path
    if [[ -d "$template_dir" ]]; then
        _path_files -W "$template_dir"
    fi
}
# Completion from both template dir and current dir for dynpr tool
compdef _dynpr_completion dynpr
```

After adding this, restart your terminal or run `source ~/.zshrc` to activate it.

## Requirements

dynpr uses only built-in Ruby libraries and requires no external gems or dependencies.