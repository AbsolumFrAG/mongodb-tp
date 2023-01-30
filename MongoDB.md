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
db.salles.find({"adresse.numero": { $exists: false }}, { _id: 1 }) // Affiche l'id des salles qui n'ont pas de numéro d'adresse
db.salles.find({ avis: { $size: 1 }}, { _id: 1, nom: 1 }) // Affiche les salles qui ont strictement un seul avis
```