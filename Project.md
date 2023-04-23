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
    dataFromUrl "https://github.com/ZahirUddin13/Individual-Project/blob/main/Data%20and%20Resources/AnnualTicketSales.csv"
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
        , bar [ maColor "crimson" ]
        ]
```

###
