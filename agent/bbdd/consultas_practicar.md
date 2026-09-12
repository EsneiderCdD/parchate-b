# Consultas para practicar en MongoDB Shell

| Consulta | Comando |
|----------|---------|
| Próximos eventos | `db.events.find({ startDate: { $gte: new Date() } }).sort({ startDate: 1 })` |
| Eventos de hoy | `db.events.find({ startDate: { $gte: new Date("2025-09-10T00:00:00"), $lt: new Date("2025-09-11T00:00:00") } })` |
| Eventos por categoría | `db.events.find({ category: "charla" })` |
| Eventos de un creador | `db.events.find({ creatorId: ObjectId("...") })` |
| Eventos ordenados por fecha | `db.events.find().sort({ startDate: 1 })` |
| Primeros 5 eventos | `db.events.find().limit(5)` |
| Eventos por categoría y ordenados | `db.events.find({ category: "charla" }).sort({ startDate: 1 })` |
| Eventos futuros de un creador | `db.events.find({ creatorId: ObjectId("..."), startDate: { $gte: new Date() } })` |
| Contar eventos por categoría | `db.events.aggregate([{ $group: { _id: "$category", total: { $sum: 1 } } }])` |
| Eventos con proyección (solo title y fecha) | `db.events.find({}, { title: 1, startDate: 1, _id: 0 })` |
