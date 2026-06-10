# Read and Save Keywords

## Introduction

Guild provides two built-in keywords for interacting with files and resources:

* `read` — Loads a file into a Guild value.
* `save` — Persists a Guild value to disk.

These keywords are designed to eliminate much of the boilerplate traditionally associated with file handling. Instead of manually opening files, reading their contents, parsing data formats, and serializing changes, Guild allows developers to work directly with native values.

```guild
const users = read database/users.json;

users.push({
    name: "Andrew",
    age: 17
});

save users;
```

In this example, Guild automatically loads the JSON file into an object, allows it to be modified like any other Guild value, and writes the changes back when `save` is executed.

---

# Philosophy

Traditional programming languages often require several steps to work with files:

1. Open the file.
2. Read the contents.
3. Parse the contents.
4. Modify the data.
5. Serialize the data.
6. Write the file back.

Guild simplifies this process by treating files as resources that can be converted directly into Guild values.

The goal is to make resource management feel like a natural part of the language rather than an external API.

---

# The `read` Keyword

## Purpose

The `read` keyword loads the contents of a file at compile time and converts them into an appropriate Guild value.

### Syntax

```guild
const value = read path;
```

### Example

```guild
const config = read config/settings.json;
```

When the compiler encounters this statement, it:
- validates the path
- loads the resource
- determines its type and
- generates the corresponding Guild value.

---

# Resource Validation

Before a file is loaded, Guild performs several checks.

## Valid Path

The provided path must be a valid file path.

```guild
const users = read database/users.json;
```

Invalid values produce compile-time errors.

```guild
const users = read 42;
```

Error:

```text
Invalid resource path.
```

---

## Directory Restrictions

Guild only allows resources to be loaded from directories declared in the project file.

Example project configuration:

```guild
dirs: [
    "./assets",
    "./database",
    "./config"
]
```

Valid:

```guild
const users = read database/users.json;
```

But this is invalid:

```guild
const users = read secret/users.json;
```

And throws an error:

```text
Resource path is not declared in project directories.
```

This restriction helps prevent accidental access to unauthorized files.

---

## Project Files

Project files cannot be loaded using `read`.

```guild
const project = read ether_web_app;
```

Error:

```text
Project files cannot be loaded using 'read'.
```

This is to prevent clashes as a project should not read another project's data.

In Guild, a project file is a guild file with the `!doctype project` directive.

---

# Type Conversion

After validation, Guild determines how the file should be represented.

Different file contents produce different value types.

---

## Script Files → Functions

When a Guild script file is loaded, the compiler transforms its contents into a callable function.

### File

```guild
// startup.gnt

print("Application started.");
```

### Usage

```guild
const startup = read startup.gnt;

startup();
```

### Compiler Interpretation

```guild
function startup() {
    print("Application started.");
}
```

This allows scripts to be treated as reusable executable resources.

---

## Objects and JSON Files

Objects, arrays, strictObjects, and JSON files are converted directly into Guild values.

### JSON File

```json
{
    "name": "Andrew",
    "age": 17
}
```

### Usage

```guild
const user = read user.json;
```

### Result

```guild
print(user.name);
print(user.age);
```

No manual parsing is required.

---

## Multiple Object Declarations

When a file contains several object declarations, Guild groups them into a single container object.

### File

```guild
let user = {
    name: "Andrew"
};

let settings = {
    theme: "dark"
};
```

### Usage

```guild
const data = read data.gnt;
```

### Result

```guild
data.user
data.settings
```

This provides a convenient way to organize related resources.

---

## Text Resources

Files that do not match any supported structured format are returned as strings.

### Example

```guild
const stylesheet = read styles/main.css;
// stylesheet is a string.
```

This behavior allows arbitrary files to be embedded and accessed as plain text.

---

# Resource Binding

Values created through `read` maintain a hidden association with their source file.

```guild
const config = read config.json;
```

Internally:

```text
config
├── source: config.json
```

This information allows Guild to determine where the value should be written when saved.

---

# The `save` Keyword

## Purpose

The `save` keyword writes a value to disk.

Unlike some persistence systems, Guild never saves automatically.

Changes remain in memory until the developer explicitly chooses to save them.

---

## Syntax

```guild
save value;
```

### Example

```guild
const config = read config.json;

config.theme = "dark";

save config;
```

Only the `save` statement updates the file.

---

# Saving Multiple Resources

Several values may be saved simultaneously.

```guild
save config, users, cache;
```

Equivalent to:

```guild
save config;
save users;
save cache;
```

This can improve readability when multiple resources are updated together.

---

# Saving to a Different Location

A value can also be written to a new destination.

### Syntax

```guild
save value as path;
```

### Example

```guild
save users as backup/users.json;
```

This is useful for:

* Backups
* Exporting data
* Generating new resource files
* Duplicating existing resources

---

# Saving Non-Resource Values

Values that were not created through `read` have no associated source file.

```guild
const users = [];
```

Attempting to save directly produces an error.

```guild
save users;
```

Error:

```text
Cannot save value.
No source file associated with resource.
```

A destination must be specified.

```guild
save users as users.json;
```

---

# Unsupported Values

Functions cannot be saved.

```guild
const startup = read startup.gnt;

save startup;
```

Error:

```text
Functions cannot be serialized.
```

Since executable code cannot be reliably reconstructed from a generated function, Guild prevents this operation.

---

# Design Goals

The `read` and `save` keywords were created to:

* Reduce boilerplate.
* Eliminate repetitive parsing logic.
* Encourage explicit persistence.
* Provide compile-time resource validation.
* Improve developer productivity.
* Make external resources feel like native Guild values.

Together, these keywords form Guild's resource management system, allowing developers to work with files using familiar language constructs rather than low-level file APIs.
