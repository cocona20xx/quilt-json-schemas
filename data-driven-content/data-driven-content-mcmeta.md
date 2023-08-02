# Summary

This file acts as a format definition for the `ddc.mcmeta` file used in QSL Static Packs to declare that files dependent on the DDC API exist within it, and which Content Deserializers and Content Owners the pack depends on. It can be seen as an equivalent to both `pack.mcmeta` *and* `quilt.mod.json` for static packs containing Data-Driven Content.

Many of the concepts in this spec are copied in part from the similar `quilt.mod.json` schema spec file.  

# Nomenclature

The term '*this pack*' refers to the Static Pack a specific instance of `ddc.mcmeta` is found in. Only one instance of `ddc.mcmeta` can exist in a Static Pack.

The acronyms '*CD*' and '*CO*' refer to 'Content Deserializers' and 'Content Owners' respectively. See their respective representation objects for more detail: [Content Deserializer Dependency Objects](#content-deserializer-dependency-objects) and [Content Owner Dependency Objects](#content-owner-dependency-objects).

The acronym '*DDC*' stands for Data-Driven Content. 

# Top-Level Keys and Values

* [schema-version](#the-schema_version-field) — The schema version to be used when reading this file
* [pack_id](#the-pack_id-field) — The ID of this pack
* [version](#the-version-field) — The version of this Static Pack
* [metadata](#the-metadata-field) — Information about this Static Pack
    * [name](#the-name-field) — The pack name
    * [description](#the-description-field) — The pack description
    * [contributors](#the-contributors-field) — Collection of contributors to this Static Pack
    * [icon](#the-icon-field) — The pack icon
    * [contact](#the-contact-field) — Collection of contact information
    * [license](#the-license-field) — One or more licenses this static pack is under
* [content_deserializers](#the-content_deserializers-field) - Information about Content Deserializers pertaining to this Static Pack 
    * [depends](#the-depends-field) — The Content Deserializers depended on by this Static Pack
    * [incompatible](#the-incompatible-field) — Content Deserializers this Static Pack is incompatible with
* [content_owners](#the-content_owners-field) — Information about Content Owners pertaining to this Static Pack
    * [depends](#the-depends-field-1) - Mods or other Static Packs this Static Pack depends on for reasons *other* than CDs, such as items, blocks, or entities added by said mods or packs
    * [breaks](#the-breaks-field) — Mods or other Static Packs this Static Pack is incompatible with for reasons *other* than CDs

 
        
## The `schema_version` field

| Type   | Required |
|--------|----------|
| Number | True     |

The `ddc.mcmeta` schema to be used loading this file. Currently, the only valid version is 1.

## The `pack_id` field

| Type   | Required |
|--------|----------|
| Number | True*    |

The ID of this pack, which much like the ID of a mod, acts as a unique identifier for it, matching the `^[a-z][a-z0-9-_]{1,63}$` regular expression.  Any piece of content defined within this pack will use this ID as its namespace. As with mod IDs, best practice is for pack IDs to be in snake_case. Can also be declared in the more generalized `static_pack.mcmeta`, in which case it is becomes optional within this file. 

## The `version` field
| Type   | Required |
|--------|----------|
| Number | True*    |

The version number of this Static Pack. Can also be declared in the more generalized `static_pack.mcmeta`, in which case it is becomes optional within this file. 

## The `metadata` field
| Type   | Required |
|--------|----------|
| Object | False    |

Optional metadata that can be used by mods to display information about this static pack. Much of this can be also be declared in `static_pack.mcmeta`.

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

## The `content_deserializers` field
| Type   | Required |
|--------|----------|
| Object | True     |

Contains the arrays holding various [Content Deserializer dependency objects](#content-deserializer-dependency-objects) that define the relationship this pack has with said CDs.

### The `depends` field

| Type   | Required |
|--------|----------|
| Array  | True     |

An array of [Content Deserializer dependency objects](#content-deserializer-dependency-objects) representing the CDs this static pack cannot load without.

### The `incompatible` field

| Type   | Required |
|--------|----------|
| Array  | False    |

An array of [Content Deserializer dependency objects](#content-deserializer-dependency-objects) that represent the CDs this static pack is incompatible with, and therefore will not load if they are present.

## The `content_owners` field
| Type   | Required |
|--------|----------|
| Object | False    |

Contains the arrays holding various [Content Owner dependency objects](#content-owner-dependency-objects) that define the relationship this pack has with said COs. Note that, like with mod dependencies, vanilla Minecraft is considered a content owner.

### The `depends` field

| Type   | Required |
|--------|----------|
| Array  | False    |

An array of [Content Owner dependency objects](#content-owner-dependency-objects), representing COs this static pack cannot load without.

### The `breaks` field

| Type   | Required |
|--------|----------|
| Array  | False    |

An array of [Content Owner dependency objects](#content-owner-dependency-objects) that represent the COs this static pack is incompatible with, and therefore will not load if they are present.

# Object Types
Below are the definitions for various special object types used throughout `ddc.mcmeta`.

## Content Owner Dependency Objects

A CO dependency object represents a static pack or mod this pack either depends on or breaks. It can be represented as either an object containing at least the [`id`](#the-id-field) field, a string representing a mod in the form `mod_id`, a string representing a Static Pack in either the form `pack_id` or `mod_id:pack_id` if the pack is defined by a mod, or an array of CO dependency objects. If an array of CO dependency objects is provided, the dependency matches if it matches ANY of the CD dependency objects for the [`depends`](#the-depends-field-1) field, and ALL for the [`breaks`](#the-breaks-field) field.

## Content Deserializer Dependency Objects

A CD dependency object represents a Content Deserializer provided by a mod which this pack depends on. It can be represented as either an object containing at least the [`id`](#the-id-field) field, a string mod identifier in the form `mod_id:cd_name`, or an array of CD dependency objects. If an array of CD dependency objects is provided, the dependency matches if it matches ANY of the CD dependency objects for the [`depends`](#the-depends-field) field, and ALL for the [`incompatible`](#the-incompatible-field) field.

## Shared Dependency Fields

Both types of dependency objects (COs and CDs) share many of the same fields. Said fields are listed below.

### The `id` field

| Type   | Required |
|--------|----------|
| String | True     |

A Content Deserializer identifier in the format `mod_id:cd_name` for content deserializers, or `mod_id` for mods.

 In the case of [CD dependency objects](#content-deserializer-dependency-objects), defining a mod as the ID field in a CD dependency object will allow *all* CDs added by the specified mod to be used by this pack's DDC files. Please note that `maven_name:mod_id` is NOT supported—the DDC API is unable to see the maven name of mods, and so such a field is ultimately useless.

### The `versions` field

| Type                | Required | Default |
|---------------------|----------|---------|
| Array/Object/String | False    | `"*"`   |

A version specifier, or complex set of version specifiers that control what versions match this dependency object.

It is an error to specify multiple constraints that conflict with each other (where no version would match the whole specifier).

It is an error when the resulting version specifier matches every version, and isn't the single string `"*"`, or uses the deprecated array style of definition.

#### String

Should be a single [version specifier](#version-specifier) defining the versions this dependency applies to.

#### Object

This must contain a single field, which must either be `any` or `all`.

The field value must be an array, with more constraints. Each element of the array must either be a string [version specifier](#version-specifier), or an object which is interpreted in the same way as the [versions field](#the-versions-field) itself.

### The `reason` field

| Type         | Required |
|--------------|----------|
| String       | False    |

A short, human-readable reason for the dependency object to exist.

## Version Specifier

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

## Content Owner dependency object exlusive fields

As [CO dependency objects](#content-owner-dependency-objects) are essentially a stand-in for `quilt.mod.json`'s dependency objects, they have some fields cloned from said QMJ dependency objects which [CD dependency objects](#content-deserializer-dependency-objects) do not.


### The `optional` field
| Type    | Required | Default |
|---------|----------|---------|
| Boolean | False    | `false` |

Dependencies marked as `optional` will only be checked if the mod/plugin specified by [the `id` field](#the-id-field-1) is present.

### The `unless` field
| Type                | Required |
|---------------------|----------|
| CODependencyObjects | False    |

Describes situations where this dependency can be ignored using the an object with the keys `"co"` and `"unless"`. For example:
```json
{
    "co": "incompatible_mod",
    "unless": "compat_layer_mod"
}
```
or
```json
{
    "co": {
        "id": "generic_content_mod",
        "version": "1.2.3",
        "reason": "Mod has a bug that causes incompatibility with this pack, but is no longer maintained by its author for this version of Minecraft."
    },
    "unless": {
        "id": "patch_mod",
        "reason": "Patches incompatibility in Generic Content Mod."
    }
    
}
```
Both `"co"` and `"unless"` can be fully fledged [CO dependency objects](#content-owner-dependency-objects).

# Prior Art & Rationale

As mentioned earlier, this spec is heavily based on that of the `quilt.mod.json` spec, down to reusing many of its field names. Due to Static Packs with DDC files registering content in a similar way to actual mods, said packs need to be treated in a very similar way to actual mods to account for this.
