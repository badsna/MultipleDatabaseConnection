# MultipleDatabaseConnection
Connecting multiple databases in spring boot


__1.Create two different database:__
  config_db----------------------------------------------------------->inventory_db
  file_details(Table name)                                              products (Table name)
  Columns:                                                               Columns:
  id                                                                     id
  file_path                                                              name
  status(PENEDING,PROCESSING,COMPLETE)                                   code
  success_count                                                          status(ACTIVE,DELETED)
  failure_count                                                          qty
                                                                         price

__2.Create RestApi for file_details:__
Rest API ----> save in file_details
  1 .csv PENDING 0 0
  2. .csv PENDING 0 0

__3.Use cronjob:__
CornJob------>Every 10 mins
  -read and fetch csv file one at a time from file_details whose status is PENDING
  - set status to PROCESSING

__4.Sample of csv file:__
products.csv
    "potato" PJA ACTIVE 10 1000
"BANANA" BNA ACTIVE 20 100

__5.Convert csv to entity and save in database table__
  inventory (DB) >products (Table)
  status of product should be ACTIVE at first

__6.When all the data of file is inserted into product table then set file_details >status to COMPLETE__

__7.success_count ,fail_count -> status.ACTIVE__
  -If status is ACTIVE then the product with same code should not be added and the failure_count is to be incremented
  -else if the status is DELETED then product should be added and success_count is to be incremented

__8.AOP__
  -Point no. 6 is to be done using AOP

__9.While inserting the product, the code of the product should be encrypted and should be decrypted while fetching__
__10.logging and testing__

