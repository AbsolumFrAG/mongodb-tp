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
db.salles.find({$expr: {$gt: [{$multiply: ["$_id", 100]}, "$capacite"]}}, {nom: 1, capacite: 1, _id: 0}) // Affiche le nom ainsi que la capacité des salles dont le produit de la valeur de l'identifiant par 100 est strictement supérieur à la capacité.
db.salles.find({ smac: true, $where: "this.styles.length > 2"}, { nom: 1, _id: 0 }) // Dans le cas où $where est compatible avec notre tier d'Atlas.
db.salles.find({"smac": true, "styles": {$exists: true, $gt: ["", 2]}}).pretty().map(function(doc) { return doc.nom; }); // En guise de remplacement dans le cas où $where est incompatible.
// Ces 2 requêtes permettent d'afficher le nom des salles de type SMAC programmant plus de deux styles de musiques différents en ayant la possibilité de faire usage de JavaScript.
db.salles.updateMany({}, {$inc: {"capacite": 100}}); // Incrémente les capacités de toutes les salles de 100.
db.salles.updateMany({styles: {$exists: false}}, {$push: {styles: "jazz"}}) // Ajoute le style Jazz à toutes les salles qui ne le proposent pas.
db.salles.updateMany({_id: {$nin: [2, 3]}}, {$pull: {"styles": "funk"}}); // Mets à jour toutes les salles qui ont le style funk puis retire le style funk et ou les id sont différents de 2 et 3. $nin = NOT IN en SQL.
db.salles.updateMany({ _id: 3 }, { $push: { styles: { $each: ["techno", "reggae"] }}}) // Mets à jour la salle qui a l'identifiant 3 en lui ajoutant les styles techno et reggae.
db.salles.updateMany({nom: {$regex: '[^aeiou]+$'}}, {$push: {avis: {date: new Date(), note: NumberInt(10)}}}) // Mets à jour les salles qui commencent par une voyelle (case-insensitive) en ajoutant un avis à la date du jour et une note de 10.
db.salles.updateMany({nom: {$regex: /^[zZ]/}}, {$set: {nom: "Pub Z", capacite: 50, smac: false}}, {upsert: true}) // Mets à jour les documents en mode upsert qui commence par z ou Z en leur affectant le nom Pub Z avec une capacité de 50 personnes ainsi qu'en positionnant SMAC sur false.
```