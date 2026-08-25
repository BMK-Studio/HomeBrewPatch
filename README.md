# HomebrewPatch

**HomebrewPatch** is a taiHEN user plugin for PlayStation Vita that extends native functionality for homebrew applications.

It currently provides two main features:

- **Update History** support for homebrew applications across supported PSVita firmware versions.
- Custom **Intellectual Property** content.

HomebrewPatch uses its own dedicated patch location:

```text
ux0:PatchHomebrew/
```

Each application is identified by its **Title ID**.

---

## Why HomebrewPatch exists

On the PS Vita, application patch resources normally use the official Sony patch directory:

```text
ux0:patch/<TITLE_ID>/
```

For Update History, the corresponding files are normally stored in:

```text
ux0:patch/<TITLE_ID>/sce_sys/changeinfo/
```

This mechanism works for homebrew applications on firmware 3.60.

However, on 3.60+ this Update History mechanism is no longer functional for homebrew applications.

As a result, a homebrew changelog that works correctly on firmware 3.60 may no longer appear on 3.60+ firmware.

A large part of the PSVita homebrew community uses firmware versions newer than 3.60, so relying exclusively on the original `ux0:patch/` mechanism would leave this functionality unavailable to many users.

**HomebrewPatch was created to solve this compatibility problem.**

Instead of relying on the original patch mechanism, HomebrewPatch provides its own dedicated location:

```text
ux0:PatchHomebrew/<TITLE_ID>/
```

HomebrewPatch then handles the SceShell side using firmware-specific profiles, allowing homebrew applications to provide Update History consistently across supported firmware versions.

HomebrewPatch also extends SceShell with support for custom **Intellectual Property** content, a native Sony feature that applications cannot normally customize themselves.

---

## Features

- Restores **Update History** functionality for homebrew applications on 3.60 firmware versions and newer.
- Provides consistent Update History support across supported PSVita firmware versions.
- Uses a dedicated `ux0:PatchHomebrew/` directory.
- Keeps homebrew patch data separate from the official `ux0:patch/` directory.
- Supports custom **Intellectual Property** content.
- Hooks the native SceShell Intellectual Property interface.
- Uses per-application directories based on the application's Title ID.
- Automatically detects the system firmware.
- Uses firmware-specific SceShell profiles.
- Supports multiple homebrew applications with a single HomebrewPatch installation.
- Designed for automatic integration by homebrew applications.

---

## Update History

On firmware 3.60, homebrew applications can use the standard PSVita patch location:

```text
ux0:patch/<TITLE_ID>/sce_sys/changeinfo/
```

For example:

```text
ux0:patch/VUSC00001/sce_sys/changeinfo/changeinfo.xml
ux0:patch/VUSC00001/sce_sys/changeinfo/changeinfo_02.xml
```

On later firmware versions, this mechanism no longer works correctly for homebrew applications.

HomebrewPatch replaces this dependency with its own patch location:

```text
ux0:PatchHomebrew/<TITLE_ID>/sce_sys/changeinfo/
```

For example:

```text
ux0:PatchHomebrew/VUSC00001/sce_sys/changeinfo/changeinfo.xml
ux0:PatchHomebrew/VUSC00001/sce_sys/changeinfo/changeinfo_02.xml
```

HomebrewPatch handles the SceShell integration itself using the appropriate firmware profile.

This allows the same homebrew application to provide its Update History on firmware 3.60 as well as later supported firmware versions.

---

## Intellectual Property

The **Intellectual Property** screen works differently from Update History.

It is a native Sony/SceShell feature built into the system.

Unlike Update History files, it is not normally supplied by the application through `app0:` or `ux0:patch/`, and homebrew applications cannot normally replace or customize its content.

HomebrewPatch hooks this native SceShell functionality and allows a homebrew application to provide its own title and content.

The custom information is stored in:

```text
ux0:PatchHomebrew/<TITLE_ID>/sce_sys/intellectual/property.xml
```

For example:

```text
ux0:PatchHomebrew/VUSC00001/sce_sys/intellectual/property.xml
```

HomebrewPatch reads this data and supplies it to the native SceShell Intellectual Property interface for the corresponding application.

This does not replace or move an existing application resource.

Instead, HomebrewPatch intercepts the native SceShell functionality and allows custom homebrew content to be displayed through the original system interface.

---

## Directory structure

HomebrewPatch uses the following root directory:

```text
ux0:PatchHomebrew/
```

Each application has its own directory based on its Title ID:

```text
ux0:PatchHomebrew/
└── TITLEID/
    └── sce_sys/
        ├── changeinfo/
        └── intellectual/
```

Example:

```text
ux0:PatchHomebrew/
└── VUSC00001/
    └── sce_sys/
        ├── changeinfo/
        │   ├── changeinfo.xml
        │   └── changeinfo_02.xml
        └── intellectual/
            └── property.xml
```

The two directories serve different purposes:

- `changeinfo/` provides Update History data.
- `intellectual/` provides custom content for the native SceShell Intellectual Property screen.

---

## Update History files

Update History files are stored in:

```text
ux0:PatchHomebrew/<TITLE_ID>/sce_sys/changeinfo/
```

Example:

```text
ux0:PatchHomebrew/VUSC00001/sce_sys/changeinfo/changeinfo.xml
ux0:PatchHomebrew/VUSC00001/sce_sys/changeinfo/changeinfo_02.xml
```

Applications may create or update these files whenever necessary.

---

## Intellectual Property file

Custom Intellectual Property content is stored in:

```text
ux0:PatchHomebrew/<TITLE_ID>/sce_sys/intellectual/property.xml
```

Example:

```text
ux0:PatchHomebrew/VUSC00001/sce_sys/intellectual/property.xml
```

The `property.xml` file contains the title and content that HomebrewPatch supplies to the native SceShell Intellectual Property interface.

Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<property>
    <title>BMK-Studio History</title>
    <content>
BenMitnicK is a self-taught developer who has been learning and experimenting with software development for many years.

He primarily works on small projects in his free time, driven by curiosity and a desire to learn.

BMK-Studio is simply the organization that structures this work, bringing together projects created with passion, curiosity, and a lot of programming in his spare time.
    </content>
</property>
```

---

## Installation

Copy:

```text
HomebrewPatch.suprx
```

to your taiHEN plugin directory.

For example:

```text
ur0:tai/PluginsU/HomebrewPatch.suprx
```

Then add HomebrewPatch to the `*main` section of your `config.txt`:

```text
*main
ur0:tai/PluginsU/HomebrewPatch.suprx
```

The exact plugin location does not matter as long as the path in `config.txt` points to the correct file.

Reboot the PS Vita after installation.

---

## Application integration

Applications do not need to communicate directly with HomebrewPatch.

They only need to create the appropriate files under their own Title ID.

For an application with the Title ID:

```text
VUSC00001
```

the complete structure must be:

```text
ux0:PatchHomebrew/VUSC00001/sce_sys/changeinfo/changeinfo.xml
ux0:PatchHomebrew/VUSC00001/sce_sys/changeinfo/changeinfo_02.xml
ux0:PatchHomebrew/VUSC00001/sce_sys/intellectual/property.xml
```

The application can create or update these files whenever necessary.

---

## How it works

The general workflow is:

For **Update History**, HomebrewPatch replaces the dependency on the standard `ux0:patch/` behavior that is no longer functional for homebrew applications on firmware versions newer than 3.60.

For **Intellectual Property**, HomebrewPatch hooks the native functionality and supplies custom content for the selected application.

---

## Multiple applications

HomebrewPatch is designed as a shared system component.

A single HomebrewPatch installation can support multiple homebrew applications:

```text
ux0:PatchHomebrew/
├── APP000001/
│   └── sce_sys/
│       ├── changeinfo/
│       └── intellectual/
├── APP000002/
│   └── sce_sys/
│       ├── changeinfo/
│       └── intellectual/
└── APP000003/
    └── sce_sys/
        ├── changeinfo/
        └── intellectual/
```

Each application is isolated by its own Title ID.

If no HomebrewPatch data exists for an application, HomebrewPatch leaves that application unaffected.

---

## Removing patch data

To remove HomebrewPatch data for a specific application, remove its directory:

```text
ux0:PatchHomebrew/<TITLE_ID>/
```

This does not uninstall or modify the application itself.

To completely disable HomebrewPatch, remove or comment out its entry from `config.txt` and reboot the PS Vita.

---

## Compatibility

| Firmware | Retail | Test Kit | Dev Kit |
|:--------:|:------:|:--------:|:-------:|
| **3.60** | YES | YES | — |
| **3.61** | — | — | — |
| **3.63** | — | — | — |
| **3.65** | YES | YES | — |
| **3.67** | — | — | — |
| **3.68** | YES | — | — |
| **3.69** | — | — | — |
| **3.70** | YES | — | — |
| **3.71** | — | — | — |
| **3.72** | — | — | — |
| **3.73** | — | — | — |
| **3.74** | YES | — | — |

---
