

resolve-flatpak
===============

This repo allows you to package DaVinci Resolve as a Flatpak for use on Linux Flatpak
based systems, especially e.g. Fedora Silverblue where there aren't easier installation
options. 

It's still a work-in-progress; but "works-for-me[tm]" right now.

Usage
-----

1. Clone this repo with: `git clone -b unified-patches https://github.com/RobotRoss/resolve-flatpak.git --recursive`
By default, com.blackmagic.Resolve.Free.yaml is configured to package the latest version of Resolve (18.5 Beta 3 at the time of writing).

2. Build your package, and export to a distributable single file installer:

#### Free
```
flatpak-builder --force-clean --repo=repo build-dir com.blackmagic.Resolve.Free.yaml
flatpak build-bundle repo ResolveFree.flatpak com.blackmagic.Resolve.Free
```
#### Studio
```
flatpak-builder --force-clean --repo=repo build-dir com.blackmagic.Resolve.Studio.yaml
flatpak build-bundle repo ResolveStudio.flatpak com.blackmagic.Resolve.Studio
```

4. Enjoy.

   IF YOU ENCOUNTER ISSUES: If you have appimagelauncherd (the AppImage Launcher daemon) installed and enabled, you NEED to temporarely disable it (either through systemctl or through the AppImage Launcher GUI) as it conflicts with flatpak-builder during the .run file repackaging process.

## Finding explicit Download IDs (for download_resolve.sh)
#### Studio:

```
curl -o- https://www.blackmagicdesign.com/api/support/nz/downloads.json |
    jq -r '.downloads[]
            | select(.urls["Linux"] != null)
            | select(.urls["Linux"][0]["product"] == "davinci-resolve-studio")
            | [.urls["Linux"][0].downloadTitle, .urls["Linux"][0].downloadId]
            | @tsv'
```

#### Free:

```
curl -o- https://www.blackmagicdesign.com/api/support/nz/downloads.json |
    jq -r '.downloads[]
            | select(.urls["Linux"] != null)
            | select(.urls["Linux"][0]["product"] == "davinci-resolve")
            | [.urls["Linux"][0].downloadTitle, .urls["Linux"][0].downloadId]
            | @tsv'
```

## Licensing
The icon in logo.png is licensed under the Creative [Commons Attribution-Share Alike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/deed.en) and fetched from [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:DaVinci_Resolve_Studio.png). It was only cropped afterwards.
