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
ticketSales : Spec
ticketSales =
    let
        enc =
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
        , enc []

        ---, circle [ maColor "crimson" ]---
        , line [ maTooltip ttEncoding, maColor "crimson", maWidth 10 ]
        ]
```

###
