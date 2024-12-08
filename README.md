# pre-commit-template

1. Install packages
    ```bash
    pip install pre-commit detect-secrets
    ```
1. Initialize global git template directory
    ```bash
    git config --global init.templateDir ~/.git-template
    ```
1. Install pre-commit into the git template
    ```bash
    pre-commit init-templatedir ~/.git-template
    ```
1. Create initial secret scan with custom plugins
    ```bash
    detect-secrets scan --plugin hf-token-plugin.py > .secrets.baseline
    ```
    * `detect-secrets` may expand an absolute file path for the custom plugin, you can edit it to be relative (see [our example](.secrets.baseline))
    * Update the baseline as needed
        ```bash
        detect scan --baseline .secrets.baseline
        ```
1. Use git as normal, hooks should run when you get to
    ```bash
    git commit [...]
    ```
    * If you see the below warning, the file permissions from the git template did not copy correctly (I think this is system dependent and may be a `umask` issue? not exactly sure):
        ```
        hint: The '.git/hooks/pre-commit' hook was ignored because it's not set as executable.
        ```
    * Fix this by changing the file permissions of the hooks
        ```bash
        chmod +x .git/hooks/pre-commit
        ```
