Also add this to `.zshrc` to be able to do TAB-complete files from both current directory and the prompts (`DYNPR_TEMPLATE_DIR`) directory:
```
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