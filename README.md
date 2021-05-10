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

### Transformations

While transformation are passed as parameter to the `Get` function, the Module can either use Hugo's built-in [Image processing solution](https://gohugo.io/content-management/image-processing/#readout) or [imgix](https://www.imgix.com/)

While Hugo's built-in solution gives more than you usually need on any given project, imgix sports more transformations options.

Available transformation options are:

#### With Hugo or Imgix
- width (int)
- height (int)
- crop (bool)
- format (jpg|png|tif|bmp|gif|webp)
- quality (int [0-100])
- rotate (int [0-360])

#### With Imgix
All of the above plus any options available through their [Image rendering API](https://docs.imgix.com/apis/rendering) not listed above.

### Settings

Settings are added to the project's parameter under the `tnd_media` map as shown below.

```yaml
# config.yaml
params:
  tnd_media:
    storage: bundle
```

#### storage

When an image is referenced as `/uploads/image.jpg` the module and its `Get` function needs to know if it lives in the project's assets under `/assets/uploads/image.jpg` or in a headless bundle at `content/uploads/image.jpg` or in the static directory `/static/uploads/image.jpg`.

- bundle
- assets
- static

Note that while `bundle` and `assets` can both do Hugo image processing, `static` images cannot and will need imgix to be setup in order to be "transformed".

#### Imgix

On top of using the built-in Hugo image transformation abilitym, the Module can also use the power of [TND Imgix](https://github.com/theNewDynamic/hugo-module-tnd-imgix).

Note that using imgix, image transformed with the `Get` function will not return any `Width` or `Height`

##### Set Up
In order to use imgix, you just need to set a imgix domain.

```yaml
params:
  tnd_media:
    imgix:
      domain: imgix.project.net
# That's it 🎉
```

For more advanced imgix configuration, head to the upstream documentation from [TND Imgix](https://github.com/theNewDynamic/hugo-module-tnd-imgix#settings) and store those keys alongside the above example under `params.tnd_media.imgix`

## theNewDynamic

This project is maintained and loved by [thenewDynamic](https://www.thenewdynamic.com).
