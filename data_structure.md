# Relations entre les personnes (physiques, morales...)
## personne émettrice -[lien]> personne cible
```json
relation_personne = {
    "type_lien":String,
    "emetteur":{
        "nom":"Nom+Prénom",
        "adresse":{"lat":00000.00000, "lon":00000.00000},
        "profil":info,
    },
    "cible":{
        "nom":"Nom+Prénom",
        "adresse":{"lat":00000.00000, "lon":00000.00000},
        "profil":info,
    },
    "affaire":nomAffaire
}
```

# Relations entre les pays 
## pays émetteur -[lien]> pays récepteur
```json
relation_pays = {
    "emetteur":{
        "code":codePays,
        "nom":nomPays,
        "profil":profilPays,
    },
    "récepteur":{
        "code":codePays,
        "nom":nomPays,
        "profil":profilPays,
    },
    "affaires":["nomAffaire1",...],
    "leaks":["nomLeak1", ...]
}
```