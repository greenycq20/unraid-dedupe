# Dedupe Plugin — GitHub Setup Guide

Follow these steps to get your plugin hosted on GitHub with auto-updates working.

## 1. Create the GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Repository name: `unraid-dedupe`
3. Description: `A high-performance deduplication tool for Unraid OS`
4. Set to **Public** (required for Unraid plugins)
5. Click **Create repository**

## 2. Push Your Code

From your local machine (with Git installed):

```bash
cd "d:\AI Dev\Unraind Plugins\dedupe"
git init
git add .
git commit -m "Initial release v2026.10.30"
git branch -M main
git remote add origin https://github.com/greenyc20/unraid-dedupe.git
git push -u origin main
```

Replace `YOURUSERNAME` with your GitHub username.

## 3. Update the pluginURL in the .plg File

After pushing, update the `pluginURL` entity in `unraid_dedupe_plugin.xml`:

```xml
<!ENTITY pluginURL "https://raw.githubusercontent.com/greenyc20/unraid-dedupe/main/dedupe.plg">
```

> **Important:** This URL must point to the **raw** file content, not the GitHub HTML page.

## 4. Rename the Plugin File

For distribution, the `.plg` file should be named to match the plugin:

```bash
# Rename for consistency
mv unraid_dedupe_plugin.xml dedupe.plg
```

## 5. Submit to Community Applications (Optional)

To make your plugin discoverable in the Unraid **Apps** tab:

1. Fork the [Community Applications repository](https://github.com/Squidly271/community.applications.github)
2. Create a template XML file for your plugin
3. Submit a Pull Request

Template format (`dedupe.xml`):
```xml
<?xml version="1.0"?>
<Containers>
  <Plugin>
    <PluginURL>https://raw.githubusercontent.com/greenyc20/unraid-dedupe/main/dedupe.plg</PluginURL>
    <Name>Dedupe</Name>
    <Overview>A high-performance deduplication tool for Unraid. Scan your shares for identical files using SHA/MD5 hashing and reclaim wasted space.</Overview>
    <Category>Tools:Utilities</Category>
    <Support>https://forums.unraid.net/topic/XXXXX-dedupe/</Support>
    <Icon>https://raw.githubusercontent.com/greenyc20/unraid-dedupe/main/icon/dedupe-icon.png</Icon>
    <Description>Scan your Unraid shares for duplicate files and reclaim wasted array space.</Description>
  </Plugin>
</Containers>
```

## Repository Structure

After setup, your repo should look like:

```
unraid-dedupe/
├── dedupe.plg              # Main plugin file (renamed from .xml)
├── README.md               # Project documentation
├── CHANGELOG.md            # Version history
├── LICENSE                 # MIT License
├── icon/
│   └── dedupe-icon.png     # 96x96 plugin icon
└── docs/
    └── screenshot.png      # Optional: UI screenshot for README
```

