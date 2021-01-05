# Homework

## Submission

You can continue working on the code from last week for this weeks task.

To submit you should open a pull request with all of your code in this folder.

## Task

In the following homework, you will create new API endpoints in the NodeJS application `cyf-ecommerce-api` that you created for last week homework for the Database 2 class.
```
const express = require("express");
const app = express();
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

const bodyParser = require('body-parser');
app.use(bodyParser.json());

const port = 3000;

const secrets = require('./secrets.json');

const {Pool} = require("pg")
const pool = new Pool({
    user: "postgres",
    host: "localhost",
    database: "cyf_ecommerce-api",
    password: secrets.password,
    port: 5432,
})

```

1. If you don't have it already, add a new GET endpoint `/products` to load all the product names along with their supplier names.

```
app.get('/products', (req,res) => {
  pool.query(`select p.product_name, s.supplier_name from products p
    join suppliers s on s.id = p.supplier_id;`)
    .then(result => res.send(result.rows))
    .catch(error => console.log(error))
})
```



2. Update the previous GET endpoint `/products` to filter the list of products by name using a query parameter, for example `/products?name=Cup`. This endpoint should still work even if you don't use the `name` query parameter!

```
app.get("/products/:productName", function (req, res) {
    const productName = req.params.productName;
    console.log(req.params.productName)
    pool
      .query('select product_name,supplier_name from products join suppliers on products.supplier_id = suppliers.id  where products.product_name like $1', [`%${productName}%`])
      .then((result) => res.json(result.rows))
      .catch((e) => console.error(e));
});
```


3. Add a new GET endpoint `/customers/:customerId` to load a single customer by ID.

```
app.get("/customers/:customerId", function (req,res){
    const customerId = req.params.id;

    pool
        .query('select * from customers where id=$1',[customerId])
        .then(response=>res.json(response.rows[0]))
        .catch((e)=> console.error(e));
})
```

4. Add a new POST endpoint `/customers` to create a new customer.

```
app.post("/customers", (req,res)=>{
    const {name,address,city,country} = req.body;
    console.log(name)

    pool
      .query('INSERT INTO customers(name,address,city,country) values ($1,$2,$3,$4)',[name,address,city,country])
      .then(response=>res.status(201).json("Customer created"))
      .catch((e)=> console.error(e));
})
```

5. Add a new POST endpoint `/products` to create a new product (with a product name, a price and a supplier id). Check that the price is a positive integer and that the supplier ID exists in the database, otherwise return an error.

```
app.post("/products",(req,res)=>{
    let {product_name,unit_price,supplier_id} = req.body;
  
    pool
      .query("SELECT * FROM suppliers WHERE id=$1",[supplier_id])
      .then(response=>{
        if (response.rowCount >0){
          pool 
          .query('INSERT INTO products (product_name,unit_price,supplier_id) VALUES ($1,$2,$3)',[product_name,unit_price,supplier_id])
          .then(response=> res.status(201).json("Product added"))
          .catch((e)=> console.error(e));
        } else {
            res.json("The suppliers does not exists")
        }
      })
      .catch((e)=>console.error(e));
  })
```

6. Add a new POST endpoint `/customers/:customerId/orders` to create a new order (including an order date, and an order reference) for a customer. Check that the customerId corresponds to an existing customer or return an error.


```
app.post('/customers/:customerId/orders', (req, res) => {
  // Getting data from request
  const { orderReference } = req.body;
  const customerId = req.params.customerId;
  // Validating data
  if (!customerId) {
      res.send('There\'s no customerId')
  } else {
      const customerIdNumber = Number(customerId);
      if (customerIdNumber > 0) {
          const validId = `select c.id from customers c where c.id = $1`;
          pool.query(validId, [customerIdNumber])
              .then((result) => {
                  // ! Esto dice el # d filas del resultado
                  if (result.rowCount > 0) {
                      const query = `INSERT INTO orders
                      (order_date, order_reference, customer_id)
                  VALUES
                      ($1, $2, $3)`
                      pool.query(query, [new Date(), orderReference, customerIdNumber])
                          .then((result) => res.send('You have inserted a new order in the orders table'))
                          .catch((err) => res.send('There was an error during the creation of the order. Please try again'))
                  } else {
                      res.send('The customer does not exist')
                  }
              })
      } else {
          res.send('The customerId is not a positive number')
      }
  }
});
```

7. Add a new PUT endpoint `/customers/:customerId` to update an existing customer (name, address, city and country).

```
/* app.put("/customers/:customersId",(req,res)=>{
    const {name,address,city,country} = req.body;
}) */
```

8. Add a new DELETE endpoint `/orders/:orderId` to delete an existing order along all the associated order items.

```
/* app.delete("/orders/:ordersId", (req,res)=>{
  const ordersId=req.params.ordersId;
  pool  
  .query("DELETE FROM orders where id=$1",[ordersId])
  .then(response=>res.status(200).json("orders deleted"))
  .catch((e=>console.error(e)))
}) */
```

9. Add a new DELETE endpoint `/customers/:customerId` to delete an existing customer only if this customer doesn't have orders.

app.delete("/customers/:customerId", (req,res)=>{
    const customerId=req.params.customerId;
    pool  
    .query("DELETE FROM customers where id=$1",[customerId])
    .then(response=>res.status(200).json("Customer deleted"))
    .catch((e=>console.error(e)))
})
```



10. Add a new GET endpoint `/customers/:customerId/orders` to load all the orders along the items in the orders of a specific customer. Especially, the following information should be returned: order references, order dates, product names, unit prices, suppliers and quantities.

```

```


```
app.listen(port, () => console.log(`Server is listening on port ${port}.`))
```