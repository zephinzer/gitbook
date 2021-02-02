---
description: My choice of poison for my IDE
---

# Visual Studio Code/Codium

## Known Codium Issues

### Extensions not loading in VSCodium

1. Find your binary location: `which codium`
2. Navigate to that directory and search for a file called `product.json` which should be in `./resources/app/` relative to the root of the application directory \(the binary should be in `./bin`\)
3. Search for `"extensionsGallery"` and replace the URLs as follows:

```text
"extensionsGallery": {
    "serviceUrl": "https://marketplace.visualstudio.com/_apis/public/gallery",
    "cacheUrl": "https://vscode.blob.core.windows.net/gallery/index",
    "itemUrl": "https://marketplace.visualstudio.com/items"
}
```

### 429 Too Many Requests on Ubuntu/Debian-based systems in VSCodium

Add the GPG key:

```text
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg | gpg --dearmor | sudo dd of=/etc/apt/trusted.gpg.d/vscodium-archive-keyring.gpg;
```

Change `/etc/apt/sources.list.d/vscodium.list` to:

```text
deb [signed-by=/etc/apt/trusted.gpg.d/vscodium-archive-keyring.gpg] https://paulcarroty.gitlab.io/vscodium-deb-rpm-repo/debs/ vscodium main
```

Update and upgrade:

```text
sudo apt update;
sudo apt upgrade;
```

