# MongoDB_Hard_Assignment

Now our first task is to classify each category if total value > 10000, it's "High Value"; if > 5000, it's "Medium Value"; otherwise, it's "Standard Value".For each category, calculate its total inventory value.
The query goes as:
```sh
db.products.aggregate([
  {$group:{ _id: "$category",totalInventoryValue:{$sum:{$multiply: ["$price","$quantity"]}}}},  
  {$project:{ _id:0,categoryName:"$_id",totalInventoryValue: 1,valueClassification:
  {$switch:{branches:[{case:{$gt:["$totalInventoryValue",10000] },then: "High Value"}, 
  {case:{$gt: ["$totalInventoryValue", 5000] },then: "Medium Value"}],default: "Standard Value" }}}} 
])
```
as we can see in the image we have marked the prices based on High value and Medium Value:
![image](https://github.com/user-attachments/assets/9cc739b6-75ed-4978-89fc-7b0466a10a64)

Now our next task is for each supplier.name, find the name and price of the most expensive product they supply.
The query goes as:
```sh
db.products.aggregate([
  {$sort:{ "supplier.name":1,price: -1}},
  {$group:{ _id:"$supplier.name",mostExpensiveProductName:{$first:"$name"},maxPrice:{$first:"$price"}}},
  {$project:{ _id:0,supplierName:"$_id",mostExpensiveProductName:1,maxPrice:1}}
])
```

![image](https://github.com/user-attachments/assets/747ecc1c-6654-4d23-a504-9ac6a0a420c9)

![image](https://github.com/user-attachments/assets/02130100-79b7-474a-9ea8-225b1a42e026)

Now our next task is to find products that have the "portable" tag but DO NOT have the "computer" tag. Display their name and tags.
The query goes as:
```sh
db.products.aggregate([
  {$match:{"tags":{$in:["portable"],$nin: ["computer"]}}},
  {$project:{ _id: 0,name: 1,tags:1}}
])
```
as we can see in the image we have retrieved keywords such as 'Portable' but it does not have "computer" as a keyword as we have done it using $in and $nin:
![image](https://github.com/user-attachments/assets/5f7f061d-4264-42c4-b5c5-f946a5551184)


### **End of Hard Assignment** 




