On commence par importer les données comme cela :
```
mongoimport --db dbName --collection meteo --file fileName.json --jsonArray connectionString
```

Une fois les données importées, on crée un index sur le champ date.
```js
db.meteo.createIndex({date: 1})
```

On vérifie que l'index a bien été créé via la commande :
```js
db.runCommand({listIndexes: "meteo"})
```
![[Pasted image 20230202134024.png]]

On veut chercher les jours où la température a dépassé 25°C entre juin & août :
```js
db.meteo.find({ $or: [ { "tc": { $gt: 25 }, "mois_de_l_annee": 6 }, { "tc": { $gt: 25 }, "mois_de_l_annee": 7 }, { "tc": { $gt: 25 }, "mois_de_l_annee": 8 } ] })
```

Partie de la réponse :

![[Pasted image 20230202134126.png]]

Afin de trier les jour par pression atmosphérique dans l'ordre décroissant :
```js
db.meteo.find().sort({"pres": -1})
```

Pour calculer la température moyenne par mois :
```js
db.meteo.aggregate([ { $group: { _id: { _id: "$nom", mois: "$mois_de_l_annee" }, averageTemperature: { $avg: "$tc" } } } ])
```

Pour trouver le jour le plus chaud en été :
```js
db.meteo.aggregate([ {$match: {mois_de_l_annee: {$in: [6, 7, 8]}}}, {$group: {_id: "$date", max_temp: {$max: "$tc"}}}, {$sort: {max_temp: -1}}, {$limit: 1} ])
```

Pour exporter la base de données en CSV :
```
mongoexport --uri connectionString --db eval --collection meteo --type=csv --fields date,tc,pres,mois_de_l_annee --out export.csv
```