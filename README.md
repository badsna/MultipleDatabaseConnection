# MultipleDatabaseConnection
Connecting multiple databases in spring boot 

__1. Create two different database:__
## config_db

| Table name    | Columns              |
|-------------- |----------------------|
| file_details  | id                   |
|               | file_path            |
|               |  status(PENEDING,PROCESSING,COMPLETE)|
|               | success_count        |
|               | failure_count        |

## inventory_db

| Table name | Columns      |
|------------|--------------|
| products   | id           |
|            | name         |
|            | code         |
|            | status(ACTIVE,DELETED)|
|            | qty          |

__2. Create RestApi for file_details:__<br>
 REST API -----> Save in file_details<br>
 example:

| ID | File      | Status   | Success Count | Failure Count |
|----|-----------|----------|---------------|---------------|
| 1  | abc.csv      | PENDING  | 0             | 0             |
| 2  | xyz.csv      | PENDING  | 0             | 0             |

__3. Use cronjob:__<br>
CornJob------>Every 10 mins
  - read and fetch csv file one at a time from file_details whose status is PENDING
  - set status to PROCESSING

__4. Sample of CSV file:__<br >
products.csv

| Product    | Code | Status | Quantity | Price |
|------------|------|--------|----------|-------|
| potato     | PJA  | ACTIVE | 10       | 1000  |
| BANANA     | BNA  | ACTIVE | 20       | 100   |


__5. Convert csv to entity and save in database table__
- inventory (DB) >products (Table)
- status of product should be ACTIVE at first

__6. When all the data of file is inserted into product table then set file_details >status to COMPLETE__

__7. success_count ,fail_count -> status.ACTIVE__
  - If status is ACTIVE then the product with same code should not be added and the failure_count is to be incremented
  - else if the status is DELETED then product should be added and success_count is to be incremented

__8. AOP__
  - Point no. 6 is to be done using AOP

__9. While inserting the product, the code of the product should be encrypted and should be decrypted while fetching__

__10. logging and testing__

## Note:
- change the filepath to data/products/xyz.csv in postman while inserting file details
