# E-Commerce API

## Submission

Add all your `javascript` files in this folder and create a pull request to submit your homework

## Task

- Create a new NodeJS application called `cyf-ecommerce-api`
- Add Express and node-postgres and make sure you can start the server with `node server.js`
- Add a new GET endpoint `/customers` to load all the customers from the database

```
app.get("/customers", function(req, res) {
    let query = `SELECT * FROM customers ORDER BY name`;
    pool
        .query(query)
        .then((result) => res.json(result.rows))
        .catch((e) => console.error(e));
});
```

- Add a new GET endpoint `/suppliers` to load all the suppliers from the database

```
app.get("/suppliers", function(req, res) {
    let query = `SELECT * FROM suppliers ORDER BY supplier_name`;
    pool
        .query(query)
        .then((result) => res.json(result.rows))
        .catch((e) => console.error(e));
});
```

- (STRETCH GOAL) Add a new GET endpoint `/products` to load all the product names along with their supplier names.
