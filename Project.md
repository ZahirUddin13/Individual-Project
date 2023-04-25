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
    dataFromUrl "https://zahiruddin13.github.io/webData/TopGenres1.csv" []
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
                << dataColumn "MOVIES" (nums [ 1102, 1098, 5479, 2418, 1186, 716, 630, 201, 2415, 213 ])
                << dataColumn "TOTAL GROSS" (strs [ "$64,529,536,530", "$49,339,974,493", "$35,586,177,269", "$33,687,992,318", "$19,810,201,102", "$13,430,378,699", "$10,480,124,374", "$4,293,988,317", "$2,519,513,142", "$2,185,433,323" ])
                << dataColumn "AVERAGE GROSS" (strs [ "$58,556,748", "$44,936,224", "$6,495,013", "$13,932,172", "$16,703,374", "$18,757,512", "$16,635,118", "$21,363,126", "$1,043,277", "$10,260,250" ])

        enc2 =
            encoding
                << position Theta [ pName "MARKET SHARE", pQuant ]
                << color [ mName "GENRES" ]
                << tooltips [ [ tName "GENRES", tTitle "Genre" ], [ tName "MARKET SHARE", tTitle "Market Share" ], [ tName "MOVIES", tTitle "Number of Movies" ], [ tName "TOTAL GROSS", tTitle "Total Gross" ] ]
    in
    toVegaLite [ config [], data [], enc2 [], arc [ maOuterRadius 400, maInnerRadius 230 ] ]
```

### Movie Genres and Gross Earnings

```elm {v interactive highlight = 13}
genreGross1 : Spec
genreGross1 =
    let
        enc3 =
            encoding
                << position X [ pName "GENRES", pNominal ]
                -- Takes data from source in the GENRES column --
                << position Y [ pName "TOTAL GROSS", pQuant ]
                -- Takes data from source in the TOTAL GROSS column --
                << tooltips [ [ tName "GENRES", tTitle "Genre" ], [ tName "MARKET SHARE", tTitle "Market Share (%)" ], [ tName "MOVIES", tTitle "Number of Movies" ], [ tName "TOTAL GROSS", tTitle "Total Gross ($)" ], [ tName "AVERAGE GROSS", tTitle "Average Gross ($)" ] ]
    in
    toVegaLite
        [ width 600
        , genreData
        , enc3 []
        , bar [ maTooltip ttEncoding, maColor "green", maWidth 30 ]
        ]
```
