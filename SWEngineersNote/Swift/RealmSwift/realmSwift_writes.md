# RxSwift Writes

## Note
Realm data models are defined using regular Swift classes with properties. Simply subclass Object or an existing model class to create your Realm data model objects. Realm model objects mostly function like any other Swift objects - you can add your own methods and protocols to them and use them like you would any other object. The main restriction is that you can only use an object on the thread which it was created.

## Creating Objects

1. Define a model

	```
	class Dog: Object {
	    dynamic var name = ""
	    dynamic var age = 0
	}
	```
	
2. Create a new object using the model in several ways:
	
	```
	// (1) Create a Dog object and then set its properties
	var myDog = Dog()
	myDog.name = "Rex"
	myDog.age = 10
	
	// (2) Create a Dog object from a dictionary
	let myOtherDog = Dog(value: ["name" : "Pluto", "age": 3])
	
	// (3) Create a Dog object from an array
	let myThirdDog = Dog(value: ["Fido", 5])
	```

**!!!** Since Realm parses all models defined in your code at launch, they must all be valid, even if they are never used.

## Adding Objects

```
// Create a Person object
let author = Person()
author.name = "David Foster Wallace"

// Get the default Realm
let realm = try! Realm()
// You only need to do this once (per thread)

// Add to the Realm inside a transaction
try! realm.write {
  realm.add(author)
  // Might wanna put completion handler if applicable here
}
```

## Updating Objects
### Typed Updates
```
// Update an object with a transaction
try! realm.write {
  author.name = "Thomas Pynchon"
}
```

### Creating and Updating Objects With Primary Keys
```
// Creating a book with the same primary key as a previously saved book
let cheeseBook = Book()
cheeseBook.title = "Cheese recipes"
cheeseBook.price = 9000
cheeseBook.id = 1

// Updating book with id = 1
try! realm.write {
  realm.add(cheeseBook, update: true)
}
```

If a `Book` object with a primary key value of ‘1’ already existed in the database, then that object would simply be updated. If it did not exist, then a completely new `Book` object would be created and added to the database.

You can also partially update objects with primary keys by passing just a subset of the values you wish to update, along with the primary key:

```
// Assuming a "Book" with a primary key of `1` already exists.
try! realm.write {
  realm.create(Book.self, value: ["id": 1, "price": 9000.0], update: true)
  // the book's `title` property will remain unchanged.
}
```

## Removing Objects
### Removing Realm database file
```swift
NSFileManager.defaultManager().removeItemAtURL(Realm.Configuration.defaultConfiguration.fileURL!)
```

### Preserving Realm, but remove objects
```swift
let realm = try! Realm()
try! realm.write {
  realm.deleteAll()
}
```

## Limitations
Realm supports the following property types: `Bool`, `Int8`, `Int16`, `Int32`, `Int64`, `Double`, `Float`, `String`, `NSDate`, and `NSData`.


## Reference
- [Realm Official Documentation](https://realm.io/docs/swift/latest)