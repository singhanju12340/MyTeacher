---
Creation Time: Thursday, September 12th 2024
Modified Time: Friday, September 13th 2024
---
1. easy to find
2. easy to find bugs in the system
3. easy to add new extended features
4. Design principle
5. Design pattern

Requirement:

2020- Phonepe was working with yes bank, and yes bank was banned
Phone Pe has to migrate it to other bank.
we could use Adapter design pattern for handle this scenario

1. class 
2. entity
3. design pattern
4. data base design
5. table design
6. relations between tables

LLD start:


## PayTm

Requirements: Create design (class diagram and schema design) and write working code for a command-line payment application that supports the following use cases.

  

1. Allow users to create accounts via their phone number and password.
    RegisterUser [phone_number] [password]
    
1. Allow users to update their profile details.
    UpdateUser [user_id] [name] [email] [phone_number]
    
1. Send money to another user of the application. (Send money to another phone number)
    CreateTransaction PAYTM [from_user_id] [to_user_id] [amount]
    
1. Send money to a bank account.
    CreateTransaction BANK [from_user_id] [account_number] [ifsc_code] [amount]
    
1. Allow users to make payment for the transaction via Card/ UPI/ Netbanking
    MakePayment [transaction_id] [payment_method] [... details related to payment method ...]
  
6. Allow users to refund a transaction. The refund amount will go to the original source.
	RefundTransaction [transaction_id]
	
1. Allow users to view transaction history.
	ViewTransactionsHistory [user_id]


### Create class for all the entity need to be saved in DB at Paytm payment gateway
1. user
		userId
		name
		email
		phone
		password(encrypted)- read about Bcrypt
		address
		list<bankaccount>
		
1. transactions(id generator from amazon/flipkart)
		id
		amount
		status: PENDING/CONFIRMED/CANCELED
		timestamp
		type: user-user/user-to-merchant
		source
		destination
		**list<payment> - example emi tax: 10k for 10 months will have 10 payments**  do not need to keep it in txn table. its already added in payment table
		
		
1. Payment:(id at paytm for each payment done related to transcation)
		modeOfpayment: CC/DC/NB/UPI
		id
		status
		amount
		transctionId
		
		
1. BankAccount
	1. name
	2. account
	3. ifsc
	4. account type
	


### Schema Design:

