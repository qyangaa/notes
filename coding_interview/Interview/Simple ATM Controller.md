### Testing

+ All testing can be executed by running main.py

+ Following tests are implemented in main.py, and can be called by calling the function directly:

```
successfulTest(bankAPI)
noCardTest(bankAPI)
wrongPinTest(bankAPI)
wrongAccountTest(bankAPI)
outOfBalanceTest(bankAPI)
```

+ To see the behavior of individual test, please uncomment the tests in main section

+ Inputs to bankAPI (`card, pin, accounts`) is set in main section, they can be changed to do different tests.

+ You can also write custom tests following API provided by the ATMController:

  ```python
  atmController = ATMController(bankAPI) # initialize controller
  atmController.insertCard(int)
  atmController.typePinNumber(int)
  accounts = atmController.getAccounts()  # get a list of account numbers
  account = accounts[0]
  atmController.selectAccount(account)
  atmController.deposite(int)
  atmController.withdraw(int)
  atmController.checkBalance()
  atmController.removeCard()
  ```

  





### Requirements

#### Required Flow:

+ Insert Card
+ Pin number
+ Select Account
+ See Balance/ Deposit/ Withdraw

#### Data

+ integer balance and amount
+ balance represented in integer

#### Testing

+ Testing different parts of controller part

### Components

+ ATMController:

  + login: 
    + insert card (card identification)
    + type in pin number: authorization with bankAPI
    + user loaded
  + select account:
    + show accounts
    + select account
  + CheckBalance
  + Deposit
    + amount
  + Withdraw
    + amount
    + if amount exceeds current balance, show error message
  + logout

+ User:

  + addCard
  + addAccount
  + getAccount

+ Account:

  + deposite
  + checkBalance
  + withdraw

+ bankAPI

  + addUser
  + getUser
  + createUser
  + createAccount

  