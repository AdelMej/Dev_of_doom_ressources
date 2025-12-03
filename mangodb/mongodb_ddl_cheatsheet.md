# MongoDB DDL Cheat Sheet

## Databases

-   **Create database**

    ``` js
    use mydb
    ```

-   **Show current database**

    ``` js
    db
    ```

-   **List databases**

    ``` js
    show dbs
    ```

-   **Drop database**

    ``` js
    db.dropDatabase()
    ```

## Collections

-   **Create collection**

    ``` js
    db.createCollection("users")
    ```

-   **List collections**

    ``` js
    show collections
    ```

-   **Drop collection**

    ``` js
    db.users.drop()
    ```

## Indexes

-   **Create index**

    ``` js
    db.users.createIndex({ email: 1 })
    ```

-   **List indexes**

    ``` js
    db.users.getIndexes()
    ```

-   **Drop index**

    ``` js
    db.users.dropIndex("email_1")
    ```

## Validation Rules

-   **Create collection with validation**

    ``` js
    db.createCollection("products", {
      validator: {
        $jsonSchema: {
          bsonType: "object",
          required: ["name", "price"],
          properties: {
            name: { bsonType: "string" },
            price: { bsonType: "number" }
          }
        }
      }
    })
    ```

## Views

-   **Create view**

    ``` js
    db.createView("cheapProducts", "products", [
      { $match: { price: { $lt: 10 } } }
    ])
    ```

-   **Drop view**

    ``` js
    db.cheapProducts.drop()
    ```
