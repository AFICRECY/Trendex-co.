# Trendex-co. 
## (E-Commerce Back End)

<p>&nbsp;</p>

## Technology Used: 
| Technology Used         | Resource URL           |
| ------------- |:-------------:|
| Git | [https://git-scm.com/](https://git-scm.com/)     |
| JavaScript  | [https://getbootstrap.com/docs/5.3/getting-started/introduction/](https://developer.mozilla.org/en-US/docs/Web/JavaScript)      |
| Node.js | [https://nodejs.org/en](https://nodejs.org/en)      |
| Express.js | [https://expressjs.com/](https://expressjs.com/)   |
|Sequelize   | [https://sequelize.org/docs/v6/](https://sequelize.org/docs/v6/)      | 
|  SQL   |    [https://www.mysql.com/](https://www.mysql.com/)   |
| Insomnia   | [https://insomnia.rest/](https://insomnia.rest/)     |


<p>&nbsp;</p>


## Description:

Watch Functionality Video: https://drive.google.com/file/d/1hpdERQKyUW0a53soKuRomyFVbq_KR4RP/view


Over the last 20 years, there has been a significant shift in the way customers shop, resulting in businesses needing to catch up and remain competitive. One of the most prominent trends in the market is the rise of online shopping, leading to a significant expansion of the eCommerce industry. By 2022, e-retail revenues are expected to reach an impressive $5.4 trillion USD.

The electronics industry's largest sector is internet retail, or e-commerce, which generated approximately $29 trillion in 2019. E-commerce platforms, such as Shopify and WooCommerce, offer a range of services to businesses of all sizes. Understanding the fundamental structure of these platforms is essential for full-stack web developers, given their prevalence

This application brings together multiple different kinds of software to create a ECommerce Back End application called Trendex-Co. Trendex-Co. Is a back end e-commerce database which allows the store inventory to live in a safe and dependable cloud-like database. Trendex is easily navigable and has functionalities set up to where store items, their prices, their stock amount, as well as other information can be acquired, updated, and deleted easily through some get routes. The application Insomnia was utilized for the inventory to be accessed separate from the code. Once the user, potentially a business owner starts their server they are able to access all of these features through Insomnia.

<p>&nbsp;</p>


## Table of Contents:
* Installation (JavaScript, Node.js, NPM Packages, Template Literals, Arrow Functions, Objects, and Functions,SQL, Sequelize, Dotenv, Express.js)
* Usage
* Credits
* License

<p>&nbsp;</p>


### Installation:

To install this project, a knowledge of JavaScript, Node.js, and Express.js, and Sequelize  were required. I had to first install Node.js to my computer and then install the Express and NPM packages. The Express package allowed me to use the express framework in Node.js. It allowed me to create a website which functions as a digital note taker and saves user input as well as deletes it. In order to create this application, Server Side JavaScript as well as Insomnia, SQL, and Sequelize all needed to be used in order to allow a tally of the items, stock, and price, etc information. Additionally the use of GET, POST, PUT, and DELETE Request methods were used. Methods used ranged from, Template Literals, Arrow Functions, Objects, and Functions, Variables, If/Else Statements, and the server side JavaScript. The web application is intended for the user to be able to visualize and update their store stock information. The code below makes this happen. 


Establishing links and connections
```
const express = require('express');
const routes = require('./routes');
const sequelize = require('./config/connection');


const app = express();
const PORT = process.env.PORT || 3001;


app.use(express.json());
app.use(express.urlencoded({ extended: true }));


app.use(routes);


sequelize.sync({ force: false }).then(() => {
 app.listen(PORT, () => console.log('Now listening'));
});

```
(Above:This code sets up an Express.js server and establishes a connection to a database using Sequelize. It configures the server to listen on a specific port and parse incoming requests in JSON and URL-encoded formats using middleware. It also routes incoming requests to the correct endpoints using the routes defined in the ./routes file. Finally, it syncs the Sequelize models with the database and starts the server, enabling it to receive and respond to incoming requests.)

Seeded Data Example
```
const { Product } = require('../models');


const productData = [
 {
   product_name: 'Plain T-Shirt',
   price: 14.99,
   stock: 14,
   category_id: 1,
 },
 {
   product_name: 'Running Sneakers',
   price: 90.0,
   stock: 25,
   category_id: 5,
 },
 ];


const seedProducts = () => Product.bulkCreate(productData);


module.exports = seedProducts;
```
(Above: The code defines a function called seedProducts that inserts data into the Product table in a database using the Sequelize ORM. It creates an array of product data objects and uses the bulkCreate method of the Product model to insert the data into the table. The function is exported for use in other files, typically used to add initial data to the database when setting up a new application or for testing purposes.)

API Routes
```
const router = require('express').Router();
const apiRoutes = require('./api');


router.use('/api', apiRoutes);


router.use((req, res) => {
 res.send("<h1>Wrong Route!</h1>")
});


module.exports = router;
```
(Above: This code exports a router instance created by the Express.js Router method, which defines two routes. The first route uses the router.use method to direct all requests with the '/api' prefix to the routes defined in the apiRoutes file. The second route uses the router.use method without any prefix to handle any other requests that do not match the '/api' prefix. It sends a response with the message "Wrong Route!" as an HTML heading. Finally, the router instance is exported for use in another file.)



GET/POST/PUT/DELETE Request
```
router.get('/', async (req, res) => {
 try {
   const tagData = await Tag.findAll({
     include: [{ model: Product}],
   });
   res.status(200).json(tagData);
 } catch (err) {
   res.status(500).json(err);
 }


});


router.get('/:id', async (req, res) => {
 try {
   const tagData = await Tag.findByPk( req.params.id, {
     include: [{ model: Product}],
   });
   res.status(200).json(tagData);
 } catch (err) {
   res.status(500).json(err);
 }


});


router.post('/', async (req, res) => {
 try {
   const tagData = await Tag.create(req.body);
   res.status(200).json(tagData);
 } catch (err) {
   res.status(500).json(err);
 }


});


router.put('/:id', async (req, res) => {
 try {
   const tagData = await Tag.update (req.body, {
     where: {
       id: req.params.id
     },
   })
     if (!tagData) {
       res.status(404).json({ message: 'No tag found with this id!' });
     return;
     }
   res.status(200).json(tagData);
 } catch (err) {
   res.status(500).json(err);
 }


});


router.delete('/:id', async (req, res) => {
 try {
   const tagData = await Tag.destroy({
     where: {
       id: req.params.id
     }
   });
   if (!tagData) {
     res.status(404).json({ message: 'No tag found with this id!'});
     return;
   }
   res.status(200).json(tagData);
 } catch (err) {
   res.status(500).json(err);
 }
});


```
(Above: This code sets up various routes for a server using the Express.js framework. It includes GET, POST, PUT, and DELETE routes for a Tag model, which is likely a table in a database.

The GET routes return Tag data either for all records or for a specific record identified by an ID parameter. The POST route creates a new Tag record in the database using data from the request body. The PUT route updates an existing Tag record with new data specified in the request body. The DELETE route deletes a Tag record identified by an ID parameter.

All routes use try/catch blocks to handle errors. If the operation is successful, it responds with a 200 status and the requested data in JSON format. If there is an error, it responds with a 500 status and the error message in JSON format.)



Models File
```
const { Model, DataTypes } = require('sequelize');


const sequelize = require('../config/connection');


class ProductTag extends Model {}


ProductTag.init(
 {
     id: {
       type: DataTypes.INTEGER,
       allowNull: false,
       primaryKey: true,
       autoIncrement: true
   },
     product_id: {
       type: DataTypes.STRING,
       allowNull: false,
       references: {
         model: "product",
         key: "id",
     
       }
     },
     tag_id: {
       type: DataTypes.INTEGER,
       references: {
         model: "tag",
         key: "id",
 
       }
     }
   },
 {
   sequelize,
   timestamps: false,
   freezeTableName: true,
   underscored: true,
   modelName: 'product_tag',
 }
);
```
(Above: This code defines a Sequelize model called ProductTag, which represents the join table between the Product and Tag tables in a database. The ProductTag model inherits from the Model class and defines the fields that correspond to the columns in the ProductTag table.

The ProductTag.init() method defines the columns of the table along with their data types and constraints. The id field is an auto-incrementing integer that serves as the primary key for the table. The product_id and tag_id fields are foreign keys that reference the id columns in the Product and Tag tables, respectively.

The configuration object passed to the ProductTag.init() method specifies the database connection using the sequelize object imported from ../config/connection. It also sets various options like timestamps, table name, and model name. Finally, it exports the ProductTag model to be used in other parts of the application.)


One to Many, Many to Many Relationships
```
const Product = require('./Product');
const Category = require('./Category');
const Tag = require('./Tag');
const ProductTag = require('./ProductTag');


Product.belongsTo(Category,{foreignKey: "category_id"})


Category.hasMany(Product, {foreignKey: "category_id", onDelete: "CASCADE"})


Product.belongsToMany(Tag, {
 through: {
   model: ProductTag,
 },
});


Tag.belongsToMany(Product, {
 through: {
   model: ProductTag,
 },
});


module.exports = {
 Product,
 Category,
 Tag,
 ProductTag,
};


```
(Above: This code exports an object containing the models for Product, Category, Tag, and ProductTag, which are defined in separate files.

The Product model is linked to the Category model through a foreign key constraint, which is set using the belongsTo method of Sequelize. The Category model has a one-to-many relationship with the Product model, as specified using the hasMany method.

The Product model is also linked to the Tag model through a many-to-many relationship, which is implemented using a junction table called ProductTag. This relationship is established using the belongsToMany method of Sequelize for both models.

Finally, the module exports an object containing all of these models to be used in other files of the application.)


Creating and Dropping the Database
```
DROP DATABASE IF EXISTS ecommerce_db;
CREATE DATABASE ecommerce_db;
```
(Above: This code is used to create a new database named ecommerce_db and if the database already exists, it is first dropped using DROP DATABASE IF EXISTS statement. The purpose of this code is to ensure that a fresh database is created every time it is executed, which can be useful for setting up a new instance of an application or resetting an existing database.)

Connecting to SQl Database w Sequelize
```
require('dotenv').config();
const Sequelize = require('sequelize');


const sequelize = process.env.JAWSDB_URL
 ? new Sequelize(process.env.JAWSDB_URL)
 : new Sequelize(process.env.DB_NAME, process.env.DB_USER, process.env.DB_PASSWORD, {
     host: 'localhost',
     dialect: 'mysql',
     dialectOptions: {
       decimalNumbers: true,
     },
   });
```
(Above: This code connects to a MySQL database using Sequelize and uses the dotenv library to load environment variables. It checks if a JAWSDB_URL variable exists and creates a Sequelize instance using the URL if it does, or uses DB_NAME, DB_USER, and DB_PASSWORD environment variables to create a Sequelize instance for a local database. The code specifies the host as 'localhost' and the dialect as 'mysql'. The dialectOptions object is used to pass additional options to the MySQL driver, such as setting the decimalNumbers option to true to handle decimal values properly.)


<p>&nbsp;</p>


### Usage: 

Offline retailers are limited in their capacity to provide detailed product information, resulting in the hassle of repeating the same data to each customer. Online marketplaces, on the other hand, offer ample space for sellers to describe products thoroughly, providing interested buyers with access to all relevant information in one place. This is exactly why I have created Trendex. This back-end application includes additional information such as item specific information, pricing, stock availability, and other information. 
With Trendex, product listings are a crucial aspect of ecommerce as they display visible information to customers during item searches, offering sellers a significant advantage. The added benefit for online businesses is the ability to personalize their listings quickly and easily using only a product name, stock amount, id, or the price.

A seller can add multiple images, product category, description, pricing, shipping fees, and delivery date to their listing. This way, customers can learn about many aspects of the item in one go. Creating a listing is an effective way for sellers to showcase their products to potential buyers.

Overall, this application enhancesECommerce information availability and streamlines the process and can save any business owner the time and energy of traveling store to store to update item and stock information.  



<p>&nbsp;</p>


## Credits

* Express Installation: https://expressjs.com/en/starter/installing.html
*Express API: https://expressjs.com/en/api.html
* GET & POST Requests: https://www.diffen.com/difference/GET-vs-POST-HTTP-Requests
* Error Help: https://stackoverflow.com/questions/14949118/node-js-error-cannot-find-module-express
*Insomnia: https://insomnia.rest/ 
*Express Routing Guide: https://expressjs.com/en/guide/routing.html
* Error in Sequelize: https://stackoverflow.com/questions/61515940/error-in-sequelize-name-sequelizeeagerloadingerror
* Eslint Installation: https://stackoverflow.com/questions/49227262/how-to-install-eslint-globally
* Express Middleware: https://expressjs.com/en/guide/using-middleware.html





### License:
MIT License

Copyright (c) [2023] [Afi Nkhume-Crecy]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,




