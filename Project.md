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



---Imported Libraries---
```

# The use of Data Visualisation to analyse Hollywood ticket sales and movie gentres to determine trends and movie popularity

#### Overview

The following interactive document contains visualisations on Hollywood market statistics from 1995 to 2021. It focuses on three different aspects of the Hollywood Market in order to fulfill the objectives of this project.

##### Objectives:

1. Show the trend in growth of the Hollywood industry over the last 25 years.
2. Show which genres are the most popular created movies.
3. Display which genres generate the highest revenue and the most success.
4. The visualizations should include user interaction allowing them to change what statistics are shown, for example: sales, genre, revenue.
5. New users should be able to easily use the visualisations to make inferences and gain valuable insights from the information.

## The Visualisations

```elm {l=hidden}
ticketData : Data
ticketData =
    dataFromUrl "https://zahiruddin13.github.io/webData/AnnualTicketSales1.csv" []



--- This block of code pulls the ticket sales raw data from the provided url where it is stored.---
```

### Ticket sales per year

This line graph represents the relationship between time and ticket sales, presenting yearly ticket sales statistics. Hovering over the points on the graph allow you to see details of the exact number of tickets and the average prices of tickets in that year.

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

        --- Encodes interactive tooltips for the graph---
    in
    toVegaLite
        --- toVegaLite converts the elm-vegalite specifications into a single JSON object that can be passed to Vega-lite for graphics generation.
        [ width 640
        , ticketData
        , enc1 []
        , line [ maTooltip ttEncoding, maColor "crimson", maWidth 30, maPoint (pmMarker [ maColor "red" ]) ] --- This line formats data into a line graph with a Crimson color and red points. It also enables the tooltips.
        ]
```

```elm {l=hidden}
genreData : Data
genreData =
    dataFromUrl "https://zahiruddin13.github.io/webData/TopGenres1.csv" []



--- This block of code pulls the Top Genre raw data from the provided url where it is stored.---
```

### Movie Genres and Market Share

This Donut chart shows the top ten genres within the industry since 1995. The areas covered by each genre shows their representative Market Shares.
Hovering over each of the segments allows you to see details such as the exact Market Share, Number of Movies in each genre and Total Gross revenue.

```elm {v interactive highlight = 13}
genreShare : Spec
genreShare =
    let
        config =
            configure
                << configuration (coView [ vicoStroke Nothing ])

        --- Creates a configuration for this specific visualisation. coView configures the default style and vicoStroke defines the default color.
        data =
            dataFromColumns []
                << dataColumn "GENRES" (strs [ "Adventure", "Action", "Drama", "Comedy", "Thriller/Suspense", "Horror", "Romantic Comedy", "Musical", "Documentary", "Black Comedy" ])
                << dataColumn "MARKET SHARE" (nums [ 27.14, 20.75, 14.97, 14.17, 8.33, 5.65, 4.41, 1.81, 1.06, 0.92 ])
                << dataColumn "MOVIES" (nums [ 1102, 1098, 5479, 2418, 1186, 716, 630, 201, 2415, 213 ])
                << dataColumn "TOTAL GROSS" (strs [ "$64,529,536,530", "$49,339,974,493", "$35,586,177,269", "$33,687,992,318", "$19,810,201,102", "$13,430,378,699", "$10,480,124,374", "$4,293,988,317", "$2,519,513,142", "$2,185,433,323" ])
                << dataColumn "AVERAGE GROSS" (strs [ "$58,556,748", "$44,936,224", "$6,495,013", "$13,932,172", "$16,703,374", "$18,757,512", "$16,635,118", "$21,363,126", "$1,043,277", "$10,260,250" ])

        --- This code allows data to be added manually using dataColumn. This creates lists of specified data. "nums" specifies number data and "strs" specifies strings. The decision to manually include the data for this graph was due to format issues with the data provided in the csv files.
        enc2 =
            encoding
                << position Theta [ pName "MARKET SHARE", pQuant ]
                --- Defines the sizes of each segment with the data from MARKET SHARE.
                << color [ mName "GENRES" ]
                --- This line ensures that each Genre has a different color.
                << tooltips [ [ tName "GENRES", tTitle "Genre" ], [ tName "MARKET SHARE", tTitle "Market Share" ], [ tName "MOVIES", tTitle "Number of Movies" ], [ tName "TOTAL GROSS", tTitle "Total Gross" ] ]

        --- Encodes interactive tooltips for the graph---
    in
    toVegaLite
        [ --- toVegaLite converts the elm-vegalite specifications into a single JSON object that can be passed to Vega-lite for graphics generation.
          config []
        , data []
        , enc2 []
        , arc [ maOuterRadius 400, maInnerRadius 230 ]
        ]



--- Lines 117-121 formats data into a radial graph with a set Outer Radius size and Inner Radius size. The inner radius defines the hole in the middle, making this a donut chart. This line also enables the tooltips.
```

### Movie Genres and Gross Earnings

This Bar chart once again shows the top ten genres within the industry. It details the relationship between each genre and their Total Gross revenue.
Hovering over each of the segments allows you to see details such as the exact Market Share, Number of Movies in each genre, Total Gross revenue and Average Gross revenue.

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

        --- Encodes interactive tooltips for the graph---
    in
    toVegaLite
        --- toVegaLite converts the elm-vegalite specifications into a single JSON object that can be passed to Vega-lite for graphics generation.
        [ width 640
        , genreData
        , enc3 []
        , bar [ maTooltip ttEncoding, maColor "green", maWidth 50 ]
        ]



--- This line formats data into a bar graph with a green color and a set width. It also enables the tooltips.
```
