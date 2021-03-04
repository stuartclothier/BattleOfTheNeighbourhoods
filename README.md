# Battle Of The Neighbourhoods

##  IBM capstone project

#### Project Goals

This project has two parts:
- Firstly, Toronto is segregated based on postal code and borough information into "neighbourhoods". The neighbourhoods are then clustered and then analysed based on local venues nearby.
- Secondly, Manchester neighbourhoods are analysed to determine which areas would be best for a young professional to move into.

#### Toronto neighbourhoods project outline

- Neighbourhood information was scraped from [Wikipedia](https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M) in the form of Postal code information. 
- The geographical coordinates were obtained from [cocl.us](https://cocl.us/Geospatial_data).
- Neighbourhoods were visualised using Folium.
- Venues in the vicinty of the Neighbourhoods were extracted using the FourSquare API.
- Neighbourhoods were then clustered and analysed based on the type (or category) of nearby venues.

The Notebook file for the project can be found in the [Project Files/Toronto](https://github.com/stuartclothier/BattleOfTheNeighbourhoods/tree/main/Project%20Files/Toronto) folder, as can additional files including an attempt to find geographical coordinates using the GeoPy client and a function which converts Beautiful Soup objects to lists for HTML table scraping.

#### Manchester Neighbourhoods

Notebook files can be found in the [Project Files/Manchester](https://github.com/stuartclothier/BattleOfTheNeighbourhoods/tree/main/Project%20Files/Manchester) folder. 

More to come.
