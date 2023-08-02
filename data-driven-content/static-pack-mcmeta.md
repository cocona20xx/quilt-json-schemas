# Summary
This file acts as a format definition for the `static_pack.mcmeta` file, a file with a similar purpose to both `pack.mcmeta` from vanilla Minecraft's data/resource packs, and `quilt.mod.json` for mods.

# Nomenclature

The term '*this pack*' refers to the Static Pack a specific instance of `static_pack.mcmeta` is found in. Only one instance of `static_pack.mcmeta` 
The term '*this instance*' refers to said specific instance of `static_pack.mcmeta`.

# Implementation Note

*All but one* of the fields found within `static_pack.mcmeta` can also be defined within `ddc.mcmeta` (said missing field has an equivalency in `ddc.mcmeta` as well). While `ddc.mcmeta` can 'inherit' fields/values from `static_pack.mcmeta`, allowing them to be defined in an instance of `static_pack.mcmeta` instead of within itself, instances of `static_pack.mcmeta` **cannot** do the same for fields/values defined in `ddc.mcmeta`.

# Top-Level Keys and Values

* [schema-version](#the-schema_version-field) — The schema version to be used when reading this file
* [id](#the-id-field) — The ID of this pack
* [version](#the-version-field) — The version of this Static Pack
* [metadata](#the-metadata-field) — Information about this Static Pack
    * [name](#the-name-field) — The pack name
    * [description](#the-description-field) — The pack description
    * [contributors](#the-contributors-field) — Collection of contributors to this Static Pack
    * [icon](#the-icon-field) — The pack icon
    * [contact](#the-contact-field) — Collection of contact information
    * [license](#the-license-field) — One or more licenses this static pack is under
* [minecraft_version](#the-minecraft_version-field) — Minecraft versions this pack is compatible with. The only field not also present in `ddc.mcmeta`, as instances of `ddc.mcmeta` can define said dependency as a `content_owners/depends` object.

## The `schema_version` field

| Type   | Required |
|--------|----------|
| Number | True     |

The `static_pack.mcmeta` schema to be used loading this file. Currently, the only valid version is 1.

## The `id` field

| Type   | Required |
|--------|----------|
| Number | True     |

The ID of this pack, which much like the ID of a mod, acts as a unique identifier for it, matching the `^[a-z][a-z0-9-_]{1,63}$` regular expression.  Any piece of content defined within this pack will use this ID as its namespace. As with mod IDs, best practice is for pack IDs to be in snake_case.

## The `version` field
| Type   | Required |
|--------|----------|
| Number | False*   |

The version number of this Static Pack. If an instance `ddc.mcmeta` is present, either said instance of `ddc.mcmeta` or this instance MUST define a version number.

## The `version` field
| Type   | Required |
|--------|----------|
| Number | True*    |

The version number of this Static Pack. Can also be declared in the more generalized `static_pack.mcmeta`, in which case it is becomes optional within this file. 

## The `metadata` field
| Type   | Required |
|--------|----------|
| Object | False    |

Optional metadata that can be used by mods to display information about this static pack. 

#### The `name` field
| Type   | Required |
|--------|----------|
| String | False    |

A human-readable name for this static pack.

#### The `description` field

| Type   | Required |
|--------|----------|
| Object | False    |

A human-readable description of this static pack. This description should be plain text, with the exception of line breaks, which can be represented with the newline character `\n`.

### The `contributors` field

| Type   | Required |
|--------|----------|
| Object | False    |

A collection of `key: value` pairs denoting the persons or organizations that contributed to this project. The key should be the name of the person or organization, while the value can be either a string representing a single role or an array of strings each one representing a single role.

A role can be any valid string. The "Owner" role is defined as being the person(s) or organization in charge of the project.

#### The `contact` field

| Type   | Required |
|--------|----------|
| Object | False    |

A collection of key: value pairs denoting various contact information for the people behind this pack, with all values being strings. The following keys are officially defined, though packs can provide as many additional values as they wish:

- email — Valid e-mail address for the organization/developers
- homepage — Valid HTTP/HTTPS address for the project or the organization/developers behind it
- issues — Valid HTTP/HTTPS address for the project issue tracker
- sources — Valid HTTP/HTTPS address for a source code repository

#### The `license` field
| Type                | Required |
|---------------------|----------|
| Array/String/Object | False    |

The license or array of licenses this project operates under.

A license is defined as either an [SPDX identifier](https://spdx.org/licenses/) string or an object in the following form:
```json
{
    "name": "Perfectly Awesome License v1.0",
    "id": "PAL-1.0",
    "url": "https://theperfectlyawesomelicense.com/",
    "description": "This license does things and stuff and says that you can do things and stuff too!"
}
```
The `"description"` field is optional.

#### The `icon` field
| Type          | Required |
|---------------|----------|
| Object/String | False    |

One or more paths to a square .PNG file. If an object is provided, the keys must be the resolution of the corresponding file. For example:
```json
"icon": {
    "32": "path/to/icon32.png",
    "64": "path/to/icon64.png",
    "4096": "path/to/icon4096.png"
}
```

## The `minecraft_version` field
| Type                | Required | Default |
|---------------------|----------|---------|
| Array/Object/String | False    | `"*"`   |

A [version specifier](#version-specifier), or complex set of version specifiers that control what versions of minecraft this pack is compatible with.

It is an error to specify multiple constraints that conflict with each other (where no version would match the whole specifier).

It is an error when the resulting version specifier matches every version, and isn't the single string `"*"`, or uses the deprecated array style of definition.

### Version Specifier

A version range specifier can make use of any of the following patterns:
* `*` — Matches any version. Will fetch the latest version available if needed
* `1.0.0` — Matches the most recent version greater than or equal to version 1.0.0 and less than 2.0.0
* `=1.0.0` — Matches exactly version 1.0.0 and no other versions
* `>=1.0.0` — Matches any version greater than or equal to 1.0.0
* `>1.0.0` — Matches any version greater than version 1.0.0
* `<=1.0.0` — Matches any version less than or equal to version 1.0.0
* `<1.0.0` — Matches any version less than version 1.0.0
* `1.0.x` — Matches any version with major version 1 and minor version 0.
* `~1.0.0` — Matches the most recent version greater than or equal to version 1.0.0 and less than 1.1.0
* `^1.0.0` — Matches the most recent version greater than or equal to version 1.0.0 and less than 2.0.0

# Prior Art

Like `ddc.mcmeta`, this file is heavily based on `quilt.mod.json`. The inclusion of a specific [`minecraft_version`](#the-minecraft_version-field) field is designed to mimic the role of the `pack_format` field in `pack.mcmeta` files from vanilla's resource/data packs, while being far more versatile. 

# Rationale

Static Packs (which do not contain Data-Driven Content) sit in a nebulous in-between state, being both similar to and radically different from vanilla's resource/data packs and actual mods. Thus, an `.mcmeta` file for said static packs must be more similar to `quilt.mod.json` than either form of `pack.mcmeta`, and so the file format described above is essentially a truncated version of `quilt.mod.json`. 