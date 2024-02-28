MongoDB

Create database and insert documents in collection.

use productDatabase

db.products.insertMany()


1. Find all the information about each products

db.products.find().pretty();


2. Find the product price which are between 400 to 800

db.products.find({product_price:{$gte:400,$lte:800}}).pretty();


3. Find the product price which are not between 400 to 600
 db.products.find({product_price:{$not:{$gte:400,$lte:600}}}).pretty();


4. List the four product which are greater than 500 in price 
 db.products.find({product_price:{$gt:500}}).limit(4).pretty();

Only three products are available with price greater than 500.

If we give greater than or equal to 500 then we can get four products.
 db.products.find({product_price:{$gte:500}}).limit(4).pretty();


5. Find the product name and product material of each products


db.products.find({},{product_name:1,product_material:1}).pretty();



6. Find the product with a row id of 10

 db.products.findOne({id:"10"});


7. Find only the product name and product material


 db.products.find({},{_id:0,product_name:1,product_material:1}).pretty();


8. Find all products which contain the value of soft in product material 

 db.products.find({product_material:{$eq:"Soft"}}).pretty();



9. Find products which contain product color indigo  and product price 492.00

None. No products have product color “indigo” and product price “492.00”.

But if we can retrieve products which have either product color “indigo” or product price “492.00”. 

db.products.find({$or:[{product_color:{$eq:"indigo"}},{product_price:{$eq:492.00}}]}).pretty();



10. Delete the products which product price value are same



 var pricesToDelete=db.products.aggregate([{$group:{_id:"$product_price",count:{$sum:1},docs:{$push:"$_id"}}},{$match:{count:{$gt:1}}}]).toArray();
 pricesToDelete.forEach(function(priceGroup){db.products.deleteMany({_id:{$in:priceGroup.docs}});});


Initially we have 25 products, in that product_prices 47 and 36 occur twice.
After deleting those now we have 21 products.



