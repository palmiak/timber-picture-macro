# Timber Picture Macro

## About
I really love using `<picture>` - it gives me total control over the image I want to show. It's great for RWD and for `webp` usage. The only problem is the fact that we have to write lots of repeadable code. That's why I've made this macro.

## Installation
First of all install **Timber**.

Next create a file `picture.twig` in your theme. Copy the content of `picture.twig` from this repo.

Whenever you like to use it add: `{% import 'picture.twig' as picture %}`.

## Example usage
Let's say we have something like this:
```twig
<picture>
    <source
        media="(max-width:375px)"
        type="image/webp"
        class="lazyload"
        src="{{ Image( context.transparent_img )|resize(335,315,'center') }}"
        data-srcset="{{ Image(offer.offer.background_image)|resize(335,315,'center')|towebp }}"
    />
    <source
        media="(max-width:375px)"
        class="lazyload"
        src="{{ Image( context.transparent_img )|resize(335,315,'center') }}"
        data-srcset="{{ Image(offer.offer.background_image)|resize(335,315,'center')|tojpg }}"
    />
    <source
        type="image/webp"
        media="(min-width: 376px) and (max-width:767px)"
        class="lazyload"
        src="{{ Image( context.transparent_img )|resize(770,520,'center') }}"
        data-srcset="{{ Image(offer.offer.background_image)|resize(770,520,'center')|towebp }}"
    />
    <source
        media="(min-width: 376px) and (max-width:767px)"
        class="lazyload"
        src="{{ Image( context.transparent_img )|resize(770,520,'center') }}"
        data-srcset="{{ Image(offer.offer.background_image)|resize(770,520,'center')|tojpg }}"
    />
    <source
        type="image/webp"
        media="(min-width: 768px) and (max-width:1365px)"
        class="lazyload"
        src="{{ Image( context.transparent_img )|resize(580,400,'center') }}"
        data-srcset="{{ Image(offer.offer.background_image)|resize(580,400,'center')|towebp }}"
    />
    <source
        media="(min-width: 768px) and (max-width:1365px)"
        class="lazyload"
        src="{{ Image( context.transparent_img )|resize(580,400,'center') }}"
        data-srcset="{{ Image(offer.offer.background_image)|resize(580,400,'center')|tojpg }}"
    />
    <source
        type="image/webp"
        media="(min-width: 1366px)"
        class="lazyload"
        src="{{ Image( context.transparent_img )|resize(600,380,'center') }}"
        data-srcset="{{ Image(offer.offer.background_image)|resize(600,380,'center')|towebp }}"
    />
    <img
        class="lazyload"
        src="{{ Image( context.transparent_img )|resize(600,380,'center') }}"
        data-src="{{ Image(offer.offer.background_image)|resize(600,380,'center')|tojpg }}"
    />
</picture>
```

Using this macro we can make it shorter:
```twig
{% import 'picture.twig' as picture %}
<picture>
    {{ picture.source_({
        src: Image(offer.offer.background_image)|resize(335,315,'center'),
        webp: true,
        media: '(max-width:375px)',
        class: 'lazyload',
        lazy-image: Image( context.transparent_img )|resize(335,315,'center'),
    }) }}

    {{ picture.source_({
        src: Image(offer.offer.background_image)|resize(770,520,'center'),
        webp: true,
        media: '(min-width: 376px) and (max-width:767px)',
        class: 'lazyload',
        lazy-image: Image( context.transparent_img )|resize(770,520,'center'),
    }) }}

    {{ picture.source_({
        src: Image(offer.offer.background_image)|resize(580,400,'center') }},
        webp: true,
        media: '(min-width: 768px) and (max-width:1365px)',
        class: 'lazyload',
        lazy-image: Image( context.transparent_img )|resize(580,400,'center') }},
    }) }}

    {{ picture.img({
        src: Image(offer.offer.background_image)|resize(600,380,'center') }},
        webp: true,
        media: '(min-width: 1366px)',
        class: 'lazyload',
        lazy-image: Image( context.transparent_img )|resize(600,380,'center') }},
    }) }}
</picture>
```

That's not all - using **twig** we can create a common part of the arguments and merge arrays like this:
```twig
<picture>
    {% set default_args = { 'webp': true, 'class': lazyload } %}

    {{ picture.source_({
        src: Image(offer.offer.background_image)|resize(335,315,'center'),
        media: '(max-width:375px)',
        lazy-image: Image( context.transparent_img )|resize(335,315,'center'),
    }|merge(default_args) ) }}

    {{ picture.source_({
        src: Image(offer.offer.background_image)|resize(770,520,'center'),
        media: '(min-width: 376px) and (max-width:767px)',
        lazy-image: Image( context.transparent_img )|resize(770,520,'center'),
    }|merge(default_args) ) }}

    {{ picture.source_({
        src: Image(offer.offer.background_image)|resize(580,400,'center') }},
        media: '(min-width: 768px) and (max-width:1365px)',
        lazy-image: Image( context.transparent_img )|resize(580,400,'center') }},
    }|merge(default_args) ) }}

    {{ picture.img({
        src: Image(offer.offer.background_image)|resize(600,380,'center') }},
        media: '(min-width: 1366px)',
        lazy-image: Image( context.transparent_img )|resize(600,380,'center') }},
    }|merge(default_args) ) }}
</picture>
```

Much better, isn't it? With this macro we a have much less code but we still have almost full control.

## Functions and arguments
This macro has two functions that use same argument:
- `source()` - for adding the `source` tag in `<picture>`
- `img()` - for adding the `img` tag in `<picture>`

- `src` - img url
- `webp` - false - should we also create webp - true / false
- `class` - class names
- `lazy_load` - false - should the image be loaded with data-src/data-srcset variant
- `lazy_image` - the image that should be used until normal image is loaded
- `media` - media query
- `force_jpg` - forces image to be converted to jpg. If you use webp argument than only non-webp image variant will be changed to jpg.

## Contributors
Big thanks to [Monika](https://github.com/Montette) for being main tester and bug finder.

## To Do
- [ ] wrapper function
- [ ] functionality for 2x etc compatibility
