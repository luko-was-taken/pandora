### OBJECTIF : Obtenir la géolocalisation des adresses renseignées dans les différents fichiers
# -------------------------------------------
# DONE 
## packages 
```python
from geopy.geocoders import Nominatim
import time
from pprint import pprint
```

## instantiate a new Nominatim client
```python
app = Nominatim(user_agent="tutorial")
```

## get location
```python
def get_location_by_address(address):
    """This function returns a location as raw from an address
    will repeat until success"""
    time.sleep(1)
    try:
        return fit_address(address)
    except:
        return get_location_by_address(address)
```

## try with given address or adjust it
```python
def fit_address(address):
    """This function returns a location with a modified address if the given one is not working """
    time.sleep(1)
    try:
        return app.geocode(address).raw
    except:
        return get_location_with_modified_adress(address)
```

# TODO

## function to have a better address
```python
import re

def get_location_with_modified_adress(address,do_regex=True):
    """This function returns a location with an address that suits better to the geolocator algorithm by substracting details from the original address such as p.o box or building number"""

    # Address = [Details]+[Simple address]
    # Details = P.O. Box, building nulber, floor, building name...
    components = re.split('; |, ', address)
    if len(components) > 1 :
        if do_regex :
            for elem in components :
                if re.match(r"(P.O. BOX)", elem):
                    components.remove(elem)
        
        modified_address = ""
        modified_address_coma = ""
        for elem in components :
            modified_address += elem
            modified_address_coma = elem + ", "
        try :
            return app.geocode(modified_address).raw
        except:
            get_location_with_modified_adress(address=modified_address_coma,do_regex=False)
    else : 
        try :
            return app.geocode(address).raw
        except 
            return None

    # dico existants pour détecter la composition d'une adresse ? combien de langue ? a minima, FR, EN, ES, CH
     
```

## fonction de test pour vérifier si l'addresse trouvée est au moins bien dans le pays renseigné
```python
def test_address_in_country(coordinates,country):
    """This function returns true if the located coordinates are in the country bounding box"""
    # country --> bounding box, regarder comment l'extraire ?
    if coordinates in bounding_box:
        return True
    else :
        return False

```

## Structure des adresses selon les leaks
- BAHAMAS : 
    - "country_codes"
    - "countries"
    - "address"

```python
"BHS",
"Bahamas",
"SUITE E-2,UNION COURT BUILDING, P.O. BOX N-8188, NASSAU, BAHAMAS"

"BHS",
"Bahamas",
"LYFORD CAY HOUSE, LYFORD CAY, P.O. BOX N-7785, NASSAU, BAHAMAS"

"BHS",
"Bahamas",
"PROVIDENCE HOUSE, EAST WING EAST HILL ST, P.O. BOX CB-12399, NASSAU, BAHAMAS"
```

- OFFSHORE :
    - "name" : vide
    - "address"
    - "country_codes"
    - "countries"

```python
"",
"One Bearer Secured Debenture",
"XXX",
"Not identified"

"",
"11 Coomber Road, The Peak, Hong Kong",
"HKG",
"Hong Kong"

"",
"4 Irish Place 2nd Floor, Gibraltar.",
"GIB",
"Gibraltar"
```

- PANAMA :
    - "name" : vide
    - "address"
    - "country_codes"
    - "countries"

```python
"",
"-	27 ROSEWOOD DRIVE #16-19 SINGAPORE 737920",
"SGP",
"Singapore"

"",
"Almaly Village v.5, Almaty Kazakhstan",
"KAZ",
"Kazakhstan"

"",
"Cantonia South Road St Georges Hill Weybridge, Surrey","GBR",
"United Kingdom"
```

- PARADISE :
    - "name" : complété
    - "address"
    - "country_codes"
    - "countries"

```python
"6B Chenyu Court; 22-24 Kennedy Road; Hong Kong",
"6B Chenyu Court",
"HKG",
"Hong Kong"

"15C Suchun Industrial Square; Suzhou Industrial Park; 215126 Suzhou; People's Republic of China",
"15C Suchun Industrial Square",
"CHN",
"China"

"8F., No. 68; Minfu 13th St. Taoyuan County; 330; Taiwan",
"8F., No. 68",
"TWN",
"Taiwan"

"No. 66, Ln. 20; Dafu Rd., Shengang Township; 429 Taichung County; Taiwan",
"No. 66, Ln. 20",
"TWN",
"Taiwan"
```


## Problèmes avec le jeu de données
- adresses parfois incomplètes, le pays n'est pas toujours renseigné dans le champ mais quand meme dans la colonne countries
- adresses pas toujours structurées
- des fois, lorsque l'on applique une recherche de géolocalisation, l'adresse est trop complète ou incomplète (d'où la nécessité de compléter ou retirer certaines parties d'adresse)

## Points positifs avec le jeu de données
- pays délimitables en fonction des pays concernés dans le leak
- pays renseignés dans certains jeux de données
- il y a un minimum de structure entre adresse et complément d'adresse

## contenu de ces champs
- name:
- addresse
- country_codes
- countries

