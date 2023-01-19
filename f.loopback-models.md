# Creating Models with the `lb4` CLI

The LoopBack documentation has a very good explanation of models. If something I write below doesn't make sense, or you just want to learn more, feel free to read them!

https://loopback.io/doc/en/lb4/Model.html

---

![The location of a model in a LoopBack application](assets/f.modelarchitecture.png)

Recall that for our workshop we are going to be making a REST API to serve our fictional bookstore, Hackathon Books. The first step for creating our REST API is to define the models that are needed to run our bookstore.

In LoopBack, a model represents a "business domain object", a structure that has a set of values that represent some _thing_ that we want to interact with using our REST API. These models are implemented in TypeScript files which define the _shape_ of the data (what properties exist in that model, and what types of values are expect for each property). When we send and receive data from our REST API, we do so with JSON strings, the shapes of which is highly dependent on the shapes of our models.

Typically, each model is backed by a table in our datasource, and each property in the model is mapped to a field in the table. When we create, read, update, or delete (CRUD) instances of our model, what we are doing under the covers is creating, reading, updating, and deleting rows in our database.

This will all make much more sense as we continue to create our REST API, but just know that the model is a very important part of our equation, and will form the backbone of how our data _looks_.

## Hackathon Books Models

Let's look at some models that we might need to run our bookstore:

* **Book**: We are a bookstore, so of course we need to keep track of our books. Each book has a title, a published date, at least one **Author**, and a price that we charge when people buy that book.

* **Author**: Each author has a name of course. And also, each **Author** might write one or more **Books**. For our purposes, that is the only information we will record for our authors.

### Relationships

You will notice that just like rows in different database tables, there are often relationships between models and their instances. For our data model, we have:

1. **`Books`** has many **`Authors`**
2. **`Authors`** has many **`Books`**

It is important that we lay out our relationships so that we know what sorts of foreign keys we have to had in our source models.

To implement our relationships that a **`Book`** has many **`Authors`** (a book can be written by one or more people), and that an **`Author`** has many **`Books`** (an author can write more than one book), we will need another model, which we can call **`AuthorsBooks`**, that will need its own `id` property, and a `authorId` and `bookId` property.

It can be difficult to modify these things after you create them, so it always worth the time to sketch out your models, just like your database tables.

When are we going to define athese relationships? Well, LoopBack won't let us do that until we have created Repositories for our Models, so we will wait until we have everything else set up with our Models before we worry about the relationships.

## Creating our Models

If we had to write our Models by hand, it would take up the entire workshop. Luckily, the CLI has an option that will help us create Models with simple prompts like we are used to seeing!

Let's get started creating our Models!

To create the `Book` and `Author` models, we run `lb4 model` for each and then pass the following options.

When you run the `lb4 model` command, you should enter the following:

---
**`Book`**
```
? Model class name: Book
? Please select the model base class Entity (A persisted model with an ID)
? Allow additional (free-form) properties? No
Model Book will be created in src/models/book.model.ts

Let's add a property to Book
Enter an empty property name when done

? Enter the property name: id
? Property type: number
? Is id the ID property? Yes
? Is id generated automatically? Yes

Let's add another property to Book
Enter an empty property name when done

? Enter the property name: title
? Property type: string
? Is it required?: Yes

Let's add another property to Book
Enter an empty property name when done

? Enter the property name: publishedDate
? Property type: date
? Is it required?: Yes

Let's add another property to Book
Enter an empty property name when done

? Enter the property name: price
? Property type: number
? Is it required?: Yes

Let's add another property to Book
Enter an empty property name when done

? Enter the property name: 
   create src/models/book.model.ts

No change to package.json was detected. No package manager install will be executed.
   update src/models/index.ts

Model Book was/were created in src/models
```
---
**`Author`**
```
? Model class name: Author
? Please select the model base class Entity (A persisted model with an ID)
? Allow additional (free-form) properties? No
Model Author will be created in src/models/author.model.ts

Let's add a property to Author
Enter an empty property name when done

? Enter the property name: id
? Property type: number
? Is id the ID property? Yes
? Is id generated automatically? Yes

Let's add another property to Author
Enter an empty property name when done

? Enter the property name: name
? Property type: string
? Is it required?: Yes

Let's add another property to Author
Enter an empty property name when done

? Enter the property name: 
   create src/models/author.model.ts

No change to package.json was detected. No package manager install will be executed.
   update src/models/index.ts

Model Author was/were created in src/models
```
---
**`AuthorsBooks`**
```
? Model class name: AuthorsBooks
? Please select the model base class Entity (A persisted model with an ID)
? Allow additional (free-form) properties? No
Model AuthorsBooks will be created in src/models/authors-books.model.ts

Let's add a property to AuthorsBooks
Enter an empty property name when done

? Enter the property name: id
? Property type: number
? Is id the ID property? Yes
? Is id generated automatically? Yes

Let's add another property to AuthorsBooks
Enter an empty property name when done

? Enter the property name: authorId
? Property type: number
? Is it required?: Yes

Let's add another property to AuthorsBooks
Enter an empty property name when done

? Enter the property name: bookId
? Property type: number
? Is it required?: Yes

Let's add another property to AuthorsBooks
Enter an empty property name when done

? Enter the property name: 
   create src/models/authors-books.model.ts

No change to package.json was detected. No package manager install will be executed.
   update src/models/index.ts

Model AuthorsBooks was/were created in src/models
```

Note that we sometimes have to create fields that track the `id` fields of other models. The models should look like the following:

**`Book`**:
```
id:                 number     (required) (ID) (auto-generated)
title:              string     (required)
publishedDate:      date       (required)
price:              number     (required)
```

**`Author`**:
```
id:                 number     (required) (ID) (auto-generated)
name:               string
```

**`AuthorsBooks`**:
```
id:                 number     (required) (ID) (auto-generated)
authorId:           number     (required)
bookId:             number     (required)
```

---

Whew! That was a lot of manual entering of the fields we wanted to track. But I think you will agree that it was a lot less work than actually creating these TypeScript files ourselves. All of those files were automatically generated for you, and should be found in `src/models` in your application's directory. These models will serve as the backbone for our REST API, so most of the work we have to do in this part of the workshop is actually done!

## A Quick Note on Database Migration

As said above, LoopBack models are usually backed by database tables so that we can persist our data after our program closes down. For most REST API frameworks, we would have to create these tables by hand, ensure that all of our fields were named correctly, that those field types could be mapped to our TypeScript types, and the relatinships between tables were preserved with mechanisms like foreign keys.

Luckily, LoopBack has the concept of "Migration" that allows us to simply define our models, point LoopBack at a database, and have it create our models for us! Unfortunately, we have to do a little more work before we can create our database tables from our models. Let's continue onto creating our application's Repositories and Controllers.

---
Next: [Creating LoopBack Repositories](g.loopback-repository.md)