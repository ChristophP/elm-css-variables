# CSS variable support for Elm

Long story short:
Elm does not support using CSS custom properties easily -> This package makes it easy.

## Why would I use this package

Because
- you want to use CSS variables like so

```elm
import CssVariables exposing (cssVars)

htmlWithCssVars =
  div
    [ cssVars
      [ ( "--textColor", "blue" ),
      , ( "--fontSize", "1.2rem" )
      ]
    ]
    [ text "CSS custom properties rule!" ]
```
The current implementation of the `elm/html` package does not support this. It's been described in sevaral issues such as [this one](https://github.com/elm/html/issues/177) and  [that one](https://github.com/elm/html/issues/129). A [merge request](https://github.com/elm/virtual-dom/pull/127) fixing it has been provided but has not been merged since it was opened. 

This package provides a convenient workaround until Elm supports it natively.

## How to use?

Install the package

```sh
elm install ChristophP/elm-css-variables
```

Then import the module and use one of it's functions.

```elm
[ div
    [ cssVars -- for regular cases
      [ ( "--textColor", "blue" ),
      , ( "--fontSize", "1.2rem" )
      ]
    ]
    [ text "CSS custom properties rule!" ]
, div 
    [ cssVarList -- for setting variables conditionally
      [ ( ( "--textColor", "blue" ), True )
      , ( ( "--fontSize", "1.2rem" ), False)
      ]
    ]
    [ text "CSS custom properties rule!" ]
]
```

## How does it work?

The functions in this package use a little trick. Instead of using the regular `elm/html` `style` function (which uses properties) we use `attribute "style"` to the set css vars. You could do this yourself but the downside is that it does not compose and subsequent calls to `attribute "style"` will override each other. To avoid that one would have to build up one large style string, which can get ugly quickly.

```elm
  div
    [ attribute "style" ("--textColor: blue; --fontSize: 1.2rem;" ++ if darkBackground then "--background: gray;" else "") ]
    [ text "Long style string!" ]
```

## Pitfall

The functions `cssVars` or `cssVarList` do not "stack". This means that calling the function multiple times on the same element will result in only the last one being applied. 


