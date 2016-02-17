# productservice
product rest service

A sample application for the Java SDK 2.0 and Couchbase Server 3.0

## What's needed?
 - The product sample bucket
 - The product/brewery_products view (built in in productsample sample)
 - An additional view product/by_name with the following map function (you should copy the product designdoc to dev in order
 to edit it and add this view):

```
    function (doc, meta) {
       if (doc.type == "product") {
         emit(doc.name, doc.brewery_id)
       }
     }
```

## Building and running
Correctly configure the application for your couchbase installation by editing **`src/main/resources/application.yml`**.

To build a self-contained jar of the application, run the following Maven command:

    mvn clean package

To run the application and expose the REST API on `localhost:8080`, run the following command:

    java -jar target/productsample2-1.0-SNAPSHOT.jar

## REST API
The REST API is deployed on port 8080 and has the following routes:

### product Routes
 * `GET /product/{id}`: retrieve the product with id {id} (one json object representing the product)
 * `POST /product`: with a jsonObject in body representing the product data, creates a new product
 * `PUT /product/{id}`: with a jsonObject in body representing the updated product data, updates a product of id {id}
 * `DELETE /product/{id}`: deletes the product of id {id}
 * `GET /product`: list all the products, just outputting the products `id` and `name` in an array of JSON objects
 * `GET /product/search/{partOfName}`: list all the products which name's contains {partOfName} (ignoring case). Each returned
 product is represented as a JSON object with the product's `id` and `name` and the whoe product details under `detail`.

```
{
    "id": "theproductId",
    "name": "The product",
    "detail": {
        "name": "The product",
        "category": "German Ale",
        ...
    }
}
```

### Brewery Routes
 * `GET /brewery/{id}`: retrieve the details of brewery {id}, along with the list of products produced by this brewery (in
 a sub-array `products`, one JSON object for each product having the product's id under `id` and the product's detail under `product`).

```
"products": [
    {
        "id": "theproductId",
        "product": {
            "name": "someproduct",
            "category": "German Ale",
            ...
        }
    },
    ...
]
```
