# Battle Of The Neighbourhoods

###  IBM capstone project

This capstone project was completed as the final part of IBM's Data Science Professional Certificate, which can be found on Coursera.

### Project Goals

This project has two parts:
- Firstly, Toronto is divided based on postal code and borough information into "neighbourhoods". The neighbourhoods are then clustered based on local venues nearby. These clusters are analysed to explore the relationships and similarities of Toronto's neighbourhoods.
- Secondly, Manchester neighbourhoods are clustered and analysed, using similar techniques, to determine which areas would be best for a young professional to move into.

#### Toronto neighbourhoods 

- Neighbourhood information was scraped from [Wikipedia](https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M) in the form of Postal code information. 
- The geographical coordinates were obtained from [cocl.us](https://cocl.us/Geospatial_data).
- Neighbourhoods were visualised using Folium.
- Venues in the vicinty of the Neighbourhoods were extracted using the FourSquare API.
- Neighbourhoods were then clustered and analysed based on the type (or category) of nearby venues.

The Notebook file for the project can be found in the [Project Files/Toronto](https://github.com/stuartclothier/BattleOfTheNeighbourhoods/tree/main/Project%20Files/Toronto) folder, as can additional files including an attempt to find geographical coordinates using the GeoPy client and a function which converts Beautiful Soup objects to lists for HTML table scraping.

#### Manchester Neighbourhoods

This project seeks to cluster areas of Greater Manchester to give a sense of which areas may appeal to certain types of people, e.g. families, young professionals, working class or affluent people and so on. This project focus on finding areas that would be ideal for a young professional.

Greater Manchester's 215 wards are clustered based on nearby venue categories and percentage of ward classified as green space.

Foursquare was used for venue information and OS Data Hub was used for ward boundaries and green space locations.

Although clustering results were sub-optimal, some relationships and general trends between wards were uncovered. It was (somewhat proviosionally) suggested that West Didsbury would be ideal for a young professional given the high density of restaurants, bars and access to greenspace within the ward. 

The limiations of this project are acknowledged, when/if time permits there may be improvements to this project in the future (see [TODO](https://github.com/stuartclothier/BattleOfTheNeighbourhoods/blob/main/TODO.md)).

Notebook files can be found in the [Project Files/Manchester](https://github.com/stuartclothier/BattleOfTheNeighbourhoods/tree/main/Project%20Files/Manchester) folder. 
