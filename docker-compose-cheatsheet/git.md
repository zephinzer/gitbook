# Git

## Initialisation/Starting

### Init a repository

```bash
git init
```

### Adding a Git remote

```bash
# add using ssh
git remote add origin git@github.com:path/to/repo.git

# add using https
git remote add origin https://github.com/paht/to/repo.git
```

### Change URL of Git remote

```bash
git remote set-url origin git@github.com:/path/to/repo.git
```

### Clone a repository

```bash
# this will clone into a new directory named 'repo'
git clone github.com/path/to/repo.git

# this will clone into the current directory
git clone github.com/path/to/repo.git .
```

## Development

### Remove an item from the Git cache

```bash
# remove a file
git rm --cache ./path/to/file 

# remove a directory
git rm -r --cache ./path/to/directory
```

## Release

### Listing all Git tags

```bash
git tag --list
```

### Adding Git tag to current commit

```bash
git tag v1.2.3
```

