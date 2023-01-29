# TPT and TPH in Database Design

When designing a database, one important decision that must be made is how to structure the tables to represent the relationships between different entities. Two common approaches to structuring these relationships are Table per Type (TPT) and Table per Hierarchy (TPH).

## **Table per Type (TPT)**

In TPT, each distinct type of entity in the hierarchy is represented by its own table. For example, consider a hierarchy of classes where a "Vehicle" class is the base class and "Car" and "Truck" are subclasses. In TPT, we would have three separate tables: one for "Vehicle", one for "Car", and one for "Truck".

Each table would have its own set of columns, with the columns in the "Car" and "Truck" tables representing the properties that are unique to cars and trucks, respectively. The "Vehicle" table would have columns for properties that are common to all types of vehicles. The primary key for each table would be a foreign key in the other tables, linking them together in the hierarchy.

```yaml
Vehicle
-------
VehicleID (PK)
Make
Model

Car
---
VehicleID (PK, FK)
NumDoors

Truck
-----
VehicleID (PK, FK)
NumAxles
```

The main advantage of TPT is that it allows for a more normalized database, with fewer duplicate data. Each table only contains the columns that are relevant to the specific type of entity. This can make the database more efficient and easier to maintain.

The disadvantage of TPT is that querying the data can be more complex, as the data is spread out across multiple tables. Joining tables is often required to get all the data for a specific entity.

## **Table per Hierarchy (TPH)**

In TPH, all types of entities in the hierarchy are represented in a single table, with a column indicating the type of entity. For example, using the same vehicle hierarchy as in the TPT example, we would have a single "Vehicle" table with a "Type" column that indicates whether a row represents a "Car" or a "Truck".

```yaml
Vehicle
-------
VehicleID (PK)
Type
Make
Model
NumDoors (for cars only)
NumAxles (for trucks only)
```

The main advantage of TPH is that querying the data is simpler, as all the data is in a single table. This can make the database more efficient when querying large amounts of data.

The disadvantage of TPH is that it can lead to a less normalized database, with more duplicate data. The "NumDoors" and "NumAxles" columns, for example, will be null for the rows that represent entities of the other type. This can make the database more difficult to maintain and can lead to data inconsistencies.

## **Conclusion**

Both TPT and TPH have their own advantages and disadvantages, and the best approach will depend on the specific needs of the application. TPT is best when normalization and ease of maintenance are the primary concerns, while TPH is best when querying performance is the primary concern. It's also worth noting that, both TPT and TPH can be used together in a hybrid approach to get the best of both worlds.