# SEED Labs – SQL Injection Attack Lab
## Semana 8 e 9

### Task 1
### Get Familiar with SQL Statements

* Login into MySQL using root as username and dees as password

![task1(1)](https://user-images.githubusercontent.com/116459746/231608600-b1dc5227-cfb2-437c-bcfe-8f888fe73bc1.png)

* Using SQL commands to specify the database used: use sqllab_users; and to show all the existent tables in the database: show tables;

![task1(2)](https://user-images.githubusercontent.com/116459746/231608617-9b331db9-4566-4c66-b0c5-687c8a26e966.png)

### Task 2
### SQL Injection Attack on SELECT Statement

#### Task 2.1 - SQL Injection Attack from webpage

* First, in order to login into page www.seed-server.com , we need to discover the login parameters

Admiting that we know the username is 'admin', we need to discover the password field. For that, we use the sqllab_users database and opperating using the following command:
 
![task2(3)](https://user-images.githubusercontent.com/116459746/232121906-82641fdf-b192-4f3a-9c9e-b626129e2cfa.png)

So, the username field is *admin' #*  and the password field is *a5bdf35a1df4ea895905f6f6618e83951a6effc0*

* We login into page www.seed-server.com

![task2(2)](https://user-images.githubusercontent.com/116459746/232121785-736d4b3a-2c04-42f4-a46e-cdca147500c4.png)

The returning of this action is:

![task2(2 1)](https://user-images.githubusercontent.com/116459746/232124650-4f91b4df-36fb-4d6b-bb43-c2f46b6f29e2.png)

#### Task 2.2 - SQL Injection Attack from command line

![task2 2(1)](https://user-images.githubusercontent.com/116459746/232129841-c73416f7-c5ab-4cdf-8097-d172fd3c1346.png)
![task2 2(2)](https://user-images.githubusercontent.com/116459746/232129638-b618e1de-0272-4d49-9a53-2e079738c962.png)

#### Task 2.3 - Append a new SQL statement

1. UPDATE credential SET Name='Mafalda' WHERE Name='Alice';

![task2 3(1)](https://user-images.githubusercontent.com/116459746/232137250-37f87d9f-5680-4fbf-9477-c3cab0b6a2dd.png)
![task2 3(2)](https://user-images.githubusercontent.com/116459746/232137604-0c26e073-2d00-4cfc-9163-0e2828e7cc30.png)

2. DELETE FROM credential WHERE Name='Boby';

![task2 3(3)](https://user-images.githubusercontent.com/116459746/232138478-218fe9fe-5e6e-4dd6-8185-527d35edd492.png)
![task2 3(4)](https://user-images.githubusercontent.com/116459746/232138492-f4f61423-cdd3-4db3-bf7c-b267b7f0f464.png)

### Task 3 
### SQL Injection Attack on UPDATE Statement

#### Task 3.1 - Modify your own salary
In the Nickname field, we write the following code:  Alice', Salary='80000' WHERE EID='10000'#.

![task3 1](https://user-images.githubusercontent.com/116459746/235370846-b249e846-70a3-4474-aa38-de406aebc9a8.png)

And, by comparing the parameter salary in the following image and the parameter salary in task 2.1, we conclude the salary has been changed to '80000' as we wished.

![task3 1(2)](https://user-images.githubusercontent.com/116459746/235370980-f095d023-c483-4d15-9720-ba9479cf7272.png)

#### Task 3.2 - Modify other people salary
We can also modify other people salary from our username. In this case, we are going to modify Boby's salary from 30000 to 60000 from Alice's profile.

We write the following message in Alice's Nickname field: Boby', Salary=60000 WHERE Name='Boby'#.

![task3 2](https://user-images.githubusercontent.com/116459746/235371286-c2f3e588-47be-4347-bbd3-26b2939d5880.png)

The result of this action may be seen in the terminal by using mysql tools or by entering to Boby's profile.

![task3 2(2)](https://user-images.githubusercontent.com/116459746/235371361-d4d32185-a60f-4e7f-9bd7-691fa96f315d.png)

#### Task 3.3 - Modify other people password
Knowing the password has been hashed using SHA 1, we get the hashed password from the credential table.
We can enter other people profile by knowing the hashed password.

![task3 3](https://user-images.githubusercontent.com/116459746/235371933-e562720e-04ed-42fe-94ef-2e881822ba1e.png)

For example, we choose Ted's row and get Ted's hashed password: 99343bff28a7bb51cb6f22cb20a618701a2c2f58.
Then we enter Ted's profile by using this password.

![task 3 3(1)](https://user-images.githubusercontent.com/116459746/235372126-ef4cca50-2546-4d39-a2eb-688b74742bbe.png)

From there, we can easily modify Ted's password by editing his profile.

![task3 3(2)](https://user-images.githubusercontent.com/116459746/235372153-546783ab-300c-41e9-9dc4-725fd076d2c0.png)
