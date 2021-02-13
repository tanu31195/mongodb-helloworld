# Assignment 1: Time to Practice - The Basics & CRUD Operations

`use hospital`

    db.patientData.insertMany(
    [
      {
        "firstName": "Sam",
        "lastName": "Cook",
        "age": 41,
        "history": [
          {
            "disease": "Cold",
            "treatment": "Vitamin C"
          },
          {
            "disease": "Head ache",
            "treatment": "Panadol"
          }
        ]
      },
      {
        "firstName": "Tim",
        "lastName": "Warner",
        "age": 28,
        "history": [
          {
            "disease": "Cold",
            "treatment": "Vitamin C"
          }
        ]
      },
      {
        "firstName": "",
        "lastName": "",
        "age": 30,
        "history": [
          {
            "disease": "Head ache",
            "treatment": "Panadol"
          },
          {
            "disease": "Tooth ache",
            "treatment": "Tooth clean up"
          }
        ]
      }
    ]
    )

`db.patientData.updateOne({age: 30}, {$set: {firstName: 'Brain', lastName: 'Williams'}})`

`db.patientData.updateOne({lastName: 'Cook'}, {$set: {firstName: 'Samson', age: 43, history:[{disease: 'Cough', treatment: 'Cough Syrup'}] }})`

`db.patientData.find({age: {$gt: 30} })`

`db.patientData.deleteMany({'history.disease': 'Cold'})`
