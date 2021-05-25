# Week 7 - Building something from scratch (2)

## 11 May 2021 - 17 May 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## Considerations for designing a database in MongoDB

### One to N relations

MongoDB has three basic ways to model one-to-n relationships:

#### One to Few

Basically, the information is embedded into the object. There is only one object:

```` json
{
  name: 'Kate Monster',
  ssn: '123-456-7890',
  addresses : [
     { street: '123 Sesame St', city: 'Anytown', cc: 'USA' },
     { street: '123 Avenue Q', city: 'New York', cc: 'USA' }
  ]
}
````

- You don't have to perform a join to retrieve the information
- You can't access only the embedded information

#### One to Many

In this case, the 'parent' object would have an array referencing all the ID's of the many 'children'. Like replacement parts for a motor, the motor would have an array for all the ID's of all the replacement parts of that motor.

```` json
{   // This is the part
    _id : ObjectID('AAAA'),
    partno : '123-aff-456',
    name : '#4 grommet',
    qty: 94,
    cost: 0.94,
    price: 3.99
}

{   // This is the motor
    name : 'brushless-motor',
    manufacturer : 'Acme Corp',
    catalog_number: 1234,
    parts : [     // array of references to Part documents
        ObjectID('AAAA'),    // reference to the #4 grommet above
        ObjectID('F17C'),    // reference to a different Part
        ObjectID('D2AA'),
        // etc
    ]
}
````

#### One to Squillions (idk, this is what the mongodb doc calls it)

Let's take the motor example above. In this case, each 'part' would have the 'motor' object ID.

```` json
{   // This is the part
    partno : '123-aff-456',
    name : '#4 grommet',
    qty: 94,
    cost: 0.94,
    price: 3.99,
    motor: ObjectID('BR-MOTOR')
}

{   // This is the motor
    _id: ObjectID('BR-MOTOR')
    name : 'brushless-motor',
    manufacturer : 'Acme Corp',
    catalog_number: 1234,
}
````

### Fancier combinations

You can combine this three basic models to fit your application. For example, let's say that my question is 'Which motor does this part belongs to?'. Using the One-to-many model, you would need to query for each motor to find the part. But what if the question is 'Which parts belong to this motor?'; if you have the One-to-Squillions, you would query each part.

You may solve this by combining those two; you set an array of ID's in the motor, and an ID of the motor in each of the parts.

### Wait, this is no SQL

Yes, and this is the advantage of mongodb, and any no sql database. The data is denormalized (repeated multiple times), but each document answers each query fast.

Denormalizing only makes sense when you are reading much more than updating, since when you update, you need to modify each field in each document of that value.

---

## All blogs

| Blog | Info |
| --- | --- |
| [Week 1 - Innovation and hard\smart work](/Week_Pages/Week1_April.md) | 05 Apr 2021 - 12 Apr 2021 |
| [Week 2 - Polyglot Programming](/Week_Pages/Week2_April.md) | 13 Apr 2021 - 19 Apr 2021 |
| [Week 3 - Fancy Topics](/Week_Pages/Week3_April.md) | 20 Apr 2021 - 26 Apr 2021 |
| [Week 4 - It's all about science](/Week_Pages/Week4_April.md) | 27 Apr 2021 - 03 May 2021 |
| [Week 4 - Pretotypes](/Pretotypes/Pretotypes_April2021.md) | My pretotypes for week 4 |
| [Month 1 - Reset Phase](/Month_Pages/Month1_April.md) | 27 Apr 2021 - 03 May 2021 |
| [Week 6 - Building something from scratch (1)](/Week_Pages/Week6_May.md) | 11 May 2021 - 17 May 2021 |
