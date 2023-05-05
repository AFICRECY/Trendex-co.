# Trendex-co. (E-Commerce Back End)


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



## Description:

Watch Functionality Video: https://drive.google.com/file/d/1hpdERQKyUW0a53soKuRomyFVbq_KR4RP/view


Over the last 20 years, there has been a significant shift in the way customers shop, resulting in businesses needing to catch up and remain competitive. One of the most prominent trends in the market is the rise of online shopping, leading to a significant expansion of the eCommerce industry. By 2022, e-retail revenues are expected to reach an impressive $5.4 trillion USD.

The electronics industry's largest sector is internet retail, or e-commerce, which generated approximately $29 trillion in 2019. E-commerce platforms, such as Shopify and WooCommerce, offer a range of services to businesses of all sizes. Understanding the fundamental structure of these platforms is essential for full-stack web developers, given their prevalence

This application brings together multiple different kinds of software to create a ECommerce Back End application called Trendex-Co. Trendex-Co. Is a back end e-commerce database which allows the store inventory to live in a safe and dependable cloud-like database. Trendex is easily navigable and has functionalities set up to where store items, their prices, their stock amount, as well as other information can be acquired, updated, and deleted easily through some get routes. The application Insomnia was utilized for the inventory to be accessed separate from the code. Once the user, potentially a business owner starts their server they are able to access all of these features through Insomnia.

## Table of Contents:
* Installation (JavaScript, Node.js, NPM Packages, Template Literals, Arrow Functions, Objects, and Functions,SQL, Sequelize, Dotenv, Express.js)
* Usage
* Credits
* License


### Installation:

To install this project, a knowledge of JavaScript, Node.js, and Express.js, and Sequelize  were required. I had to first install Node.js to my computer and then install the Express and NPM packages. The Express package allowed me to use the express framework in Node.js. It allowed me to create a website which functions as a digital note taker and saves user input as well as deletes it. In order to create this application, HTML, CSS, Client-side JavaScript as well as Server Side JavaScript all needed to be used in order to make the page interactive and give the functionality to save/delete user input values. Methods used ranged from, Template Literals, Arrow Functions, Objects, and Functions, Variables, If/Else Statements, and the server side JavaScript. The web application is intended for the user to be able to visit the deployed URL and have the ability to enter whatever Title and Text they would like. They would also have the ability to save their notes to the left column, where they can read them later, or delete them if they want. The code below makes this happen. 


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



### Usage: 

Offline retailers are limited in their capacity to provide detailed product information, resulting in the hassle of repeating the same data to each customer. Online marketplaces, on the other hand, offer ample space for sellers to describe products thoroughly, providing interested buyers with access to all relevant information in one place. This is exactly why I have created Trendex. This back-end application includes additional information such as item specific information, pricing, stock availability, and other information. 
With Trendex, product listings are a crucial aspect of ecommerce as they display visible information to customers during item searches, offering sellers a significant advantage. The added benefit for online businesses is the ability to personalize their listings quickly and easily using only a product name, stock amount, id, or the price.

A seller can add multiple images, product category, description, pricing, shipping fees, and delivery date to their listing. This way, customers can learn about many aspects of the item in one go. Creating a listing is an effective way for sellers to showcase their products to potential buyers.

Overall, this application enhancesECommerce information availability and streamlines the process and can save any business owner the time and energy of traveling store to store to update item and stock information.  

