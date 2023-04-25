---
id: litvis

elm:
  dependencies:
    gicentre/elm-vegalite: latest
    gicentre/tidy: latest
---

```elm {l=hidden}
import Tidy exposing (..)
import VegaLite exposing (..)
```

# The use of Data Visualisation to analyse Hollywood ticket sales and movie gentres to determine trends and movie popularity

```elm {l=hidden}
ticketData : Data
ticketData =
    dataFromUrl "https://zahiruddin13.github.io/webData/AnnualTicketSales1.csv" []
```

### Ticket sales per year

```elm {v interactive highlight = 13}
ticketSales1 : Spec
ticketSales1 =
    let
        enc1 =
            encoding
                << position X [ pName "YEAR", pTemporal ]
                -- Takes data from source in the YEAR column --
                << position Y [ pName "TICKETS SOLD", pQuant ]
                -- Takes data from source in the TICKETS SOLD column --
                << tooltips [ [ tName "TICKETS SOLD", tTitle "Tickets Sold" ], [ tName "AVERAGE TICKET PRICE", tTitle "Average Ticket Price" ] ]
    in
    toVegaLite
        [ width 600
        , ticketData
        , enc1 []
        , line [ maTooltip ttEncoding, maColor "crimson", maWidth 30, maPoint (pmMarker [ maColor "red" ]) ]
        ]
```

### Movie Genres and Market Share

```elm {l=hidden}
genreData : Data
genreData =
    dataFromUrl "https://zahiruddin13.github.io/webData/TopGenres.csv" []
```

```elm {v interactive highlight = 13}
genreShare : Spec
genreShare =
    let
        config =
            configure
                << configuration (coView [ vicoStroke Nothing ])

        data =
            dataFromColumns []
                << dataColumn "GENRES" (strs [ "Adventure", "Action", "Drama", "Comedy", "Thriller/Suspense", "Horror", "Romantic Comedy", "Musical", "Documentary", "Black Comedy" ])
                << dataColumn "MARKET SHARE" (nums [ 27.14, 20.75, 14.97, 14.17, 8.33, 5.65, 4.41, 1.81, 1.06, 0.92 ])

        enc2 =
            encoding
                << position Theta [ pName "MARKET SHARE", pQuant ]
                << color [ mName "GENRES" ]
                << tooltips [ [ tName "GENRES", tTitle "Genre" ], [ tName "MARKET SHARE", tTitle "Market Share" ], [ tName "MOVIES", tTitle "Number of Movies" ] ]
    in
    toVegaLite [ config [], data [], enc2 [], arc [ maOuterRadius 400, maInnerRadius 230 ] ]
```
