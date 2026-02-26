# Installation for JetBrains IDEs (IntelliJ IDEA, WebStorm, etc.)

This guide walks through installing the JSL TextMate bundle in JetBrains products to get syntax highlighting for `.jsl` files.

## Prerequisites

Clone this repository to a permanent directory on your machine (the IDE will reference it directly):

```bash
git clone https://github.com/BlackBeltTechnology/judo-jsl-tmbundle.git ~/TextMateBundles/judo-jsl-tmbundle
```

## Steps

### 1. Enable the TextMate Bundles Plugin

Open **Settings → Plugins** and verify that the **TextMate Bundles** plugin is installed and enabled. If it's not active, enable it and restart the IDE.

![Check TextMate plugin](images/check_textmate_plugin.png)

### 2. Open TextMate Bundle Settings

Navigate to **Settings → Editor → TextMate Bundles** to see the list of installed bundles.

![Open installed TextMate bundles](images/open_installed_texmate_bundles.png)

### 3. Add the JSL Bundle

Click the **+** button to add a new TextMate bundle.

![Add new tmbundle extension](images/add_new_textmate_bundles.png)

### 4. Select the Bundle Directory

Browse to the cloned `judo-jsl-tmbundle` directory and select it.

![Select checked out jsl.tmbundle directory](images/browse_jsl_tmbundle.png)

### 5. Verify Installation

Confirm that **JSL** appears in the list of installed TextMate bundles.

![Check JSL is installed](images/check_jsl_installed.png)

### 6. Associate `.jsl` File Type

Open **Settings → Editor → File Types**, find the TextMate-supported types, and associate `.jsl` files with the JSL bundle.

![Associate .jsl files to TextMate bundle](images/associate_file_type.png)

### 7. Add File Pattern

Add `*.jsl` to the file name patterns so the IDE automatically uses JSL highlighting for all `.jsl` files.

![Add *.jsl to file name pattern](images/add_jsl_file_type.png)

After completing these steps, all `.jsl` files will have syntax highlighting in your JetBrains IDE.
