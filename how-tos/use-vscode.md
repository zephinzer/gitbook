# Use VSCode

## Set integrated terminal to read from `.bash_profile`

Open `settings.json` and ensure the following setting exists:

```bash
# ...
  "terminal.integrated.profiles.linux": {
    "bash": {
        "path": "bash",
        "args": ["-l"], # make sure this exists
        "icon": "terminal-bash"
    },
  },
# ...
```

