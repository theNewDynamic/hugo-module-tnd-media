This is a template repo. To start.

search `media` through the project and replace it with the module identifier (ex: `socials` for `hugo-module-tnd-socials`)

# media Hugo Module

(intro)

## Requirements

Requirements:
- Go 1.14
- Hugo 0.61.0


## Installation

If not already, [init](https://gohugo.io/hugo-modules/use-modules/#initialize-a-new-module) your project as Hugo Module:

```
$: hugo mod init {repo_url}
```

Configure your project's module to import this module:

```yaml
# config.yaml
module:
  imports:
  - path: github.com/theNewDynamic/hugo-module-tnd-media
```

## Usage

### API

The data returned by the following map is:

- .Name
- .Path
- .Permalink
- .RelPermalink
- .Width ?
- .Height ?

### Get

The only available public returning partial is `Get`.

It returns the above structured data of the potentially transformed image.

This takes two types of arguments.
- A file path | String (.)
  With the above, the function simply returns some data on the image.
- A map | Map (.)
  path: A file path | String
  ...any other key will match a transformation from the hugo or other service API and its value the property to be passed.
  With the above, the function returns the data of the transformed media.

#### Examples
```
{{ $path := "/uploads/an-image.jpg" }}
{{ with partial "tnd-media/Get" $path }}
  {{ $url = .RelPermalink }}
{{ end }}
```

```
{{ $args := dict 
  "path" "/uploads/an-image.jpg" 
  "width" 1024 
  "height" 100 
}}
{{ with partial "tnd-imgix/Get" $args }}
  {{ $url = .RelPermalink }}
{{ end }}
```

### Settings

Settings are added to the project's parameter under the `tnd_media` map as shown below.

```yaml
# config.yaml
params:
  tnd_media:
    storage: bundle
```

#### storage

When an image is referenced as `/uploads/image.jpg` the module needs ot know if it leaves in the project's assets under `/assets/uploads/image.jpg` or in a headless bundle at `content/uploads/image.jpg`

- bundle
- assets
- static

Note that no transformation is available if the storage is static.

## theNewDynamic

This project is maintained and loved by [thenewDynamic](https://www.thenewdynamic.com).
