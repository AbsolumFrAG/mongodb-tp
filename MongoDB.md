Exo 1 :
```js
use sample_db
db.createCollection("employees")
db.employees.insertMany({ name: "John Doe", age: 35, job: "Manager", salary: 80000 }, { name: "Jane Doe", age: 32, job: "Developer", salary: 75000 }, { name: "Jim Smith", age: 40, job: "Manager", salary: 85000 })
db.employees.find()
db.employees.find({age: { $gt: 33 }})
db.employees.find().sort({ salary: -1 })
db.employees.find({}, { name: 1, job: 1, _id: 0 })
db.employees.aggregate([ { $group: {_id: "$job", count: {$sum: 1}}}, {$sort: {count: -1}}])
db.employees.updateMany({ job: "Developer" }, { $set: { salary: 80000 } })
```

Exo 2 :
```js
db.salles.find({ smac: true }, { _id: 1, nom: 1 }) // Affiche les salles de type SMAC.
db.salles.find({ capacite: { $gt: 1000 } }, { _id: 0, nom: 1 }) // Affiche le nom des salles qui ont une capacité supérieure à 1000.
db.salles.find({"adresse.numero": { $exists: false }}, { _id: 1 }) // Affiche l'id des salles qui n'ont pas de numéro d'adresse.
db.salles.find({ avis: { $size: 1 }}, { _id: 1, nom: 1 }) // Affiche les salles qui ont strictement un seul avis.
db.salles.find({ styles: "blues" }, { styles: 1, _id: 0 }) // Affiche tous les styles musicaux des salles qui proposent du blues.
db.salles.find({ "styles.0": "blues"}, { styles: 1 }) // Affiche les styles musicaux qui ont blues en première position dans le tableau styles.
db.salles.find({ "adresse.codePostal": { $regex: /^84/ }, "capacite": { $lt:
 500 } }, { "adresse.ville": 1, _id: 0 }) // Affiche la ville des salles qui ont leur code postal qui commence par 84 (via un RegEx) et qui ont pour capacité de moins de 500 places.
db.salles.find({$or: [{_id: {$mod: [2, 0]}}, {avis: {$exists: false}}]}, {_id: 1}) // Affiche les identifiants des salles qui ont un id pair et ou le champ avis est vide.
db.salles.find({avis: {$elemMatch: {note: {$gte: 8, $lte: 10}}}}, {nom: 1}) // Affiche le nom des salles qui ont un avis entre 8 et 10.
db.salles.find({avis: {$elemMatch: {date: {$gt: new Date('2019-11-15')}}}}, {nom: 1, _id: 0}) // Affiche le nom des salles qui comporte un avis avec une date supérieure au 15/11/2019.
```