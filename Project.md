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

# THE USE OF DATA VISUALISATION TO ANALYSE HOLLYWOOD TICKET SALES AND MOVIE GENRES TO DETERMINE TRENDS AND MOVIE POPULARITY

###

```elm {l=hidden}
ticketData : Data
ticketData =
    dataFromUrl "https://zahiruddin13.github.io/webData/AnnualTicketSales1.csv" []
```

```elm {v}
ticketSales : Spec
ticketSales =
    let
        enc =
            encoding
                << position X [ pName "YEAR", pTemporal ]
                << position Y [ pName "TICKETS SOLD", pQuant ]
    in
    toVegaLite
        [ width 640
        , ticketData
        , enc []
        , bar []
        ]
```

###
