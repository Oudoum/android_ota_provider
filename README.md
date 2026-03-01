# Simple Automated LineageOS OTA Provider

A minimal OTA backend for LineageOS that uses **GitHub Releases** for hosting and requires **no external server**.  
ROM uploads and JSON generation are handled automatically through a small shell script.

---

## Requirements

To generate OTA metadata and upload releases, the following tools must be installed:

- `jq`
- `gh`
- `unzip`
- `git`

---

## Usage

To upload a ROM and automatically generate the corresponding OTA JSON file, run:

```bash
source release.sh ../path/to/lineageos.zip
```
This will:
- extract metadata from the ROM ZIP
- generate the correct JSON structure
- write the JSON to the appropriate version folder
- commit and push the update
- create a GitHub Release containing the ROM

---

## Customizing the OTA URL

If you want to host this backend on your own GitHub repository, edit the ```URL= value``` inside ```release.sh```.

```bash
URL="https://github.com/<yourname>/<yourrepo>/releases/download/${RELASE_TAG}/${FILENAME}"
```

---

## Device-Side Configuration (Overlay)

To make your ROM use this OTA provider, add an overlay defining the updater URL:

```xml
<string name="updater_server_url" translatable="false">
    https://raw.githubusercontent.com/<user>/<repo>/master/20.0/{device}.json
</string>
```

You may either hardcode the device name (e.g. ```herolte.json```) or leave ```{device}``` dynamic.
If left dynamic, users can break OTA updates by modifying device properties.
