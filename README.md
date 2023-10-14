# TECH6301 Mini Project: Bank Account 


class BankAccount:
    
    account_counter = 0

    def __init__(self, name, accountType, balance=0):
        self.name = name
        self.accountType = accountType
        self.balance = balance
        BankAccount.account_counter += 1
        self.accountNumber = BankAccount.account_counter
        self.filename = self.generate_transaction_filename()
        self.create_transaction_file()

    def generate_transaction_filename(self):
        return f"{self.accountNumber}_{self.accountType}_{self.name}.txt"

    def create_transaction_file(self):
        with open(self.filename, 'w') as file:
            file.write(f"Account ID: {self.accountNumber}\n")
            file.write(f"Account Type: {self.accountType}\n")
            file.write(f"Account Holder: {self.name}\n")
            file.write(f"Initial Balance: ${self.balance:.2f}\n")

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            with open(self.filename, 'a') as file:
                file.write(f"Deposit: +${amount:.2f}\n")
                file.write(f"New Balance: ${self.balance:.2f}\n")

    def withdraw(self, amount):
        if amount > 0 and amount <= self.balance:
            self.balance -= amount
            with open(self.filename, 'a') as file:
                file.write(f"Withdrawal: -${amount:.2f}\n")
                file.write(f"New Balance: ${self.balance:.2f}\n")
        else:
            print("Withdrawal failed. Insufficient funds.")

    def get_balance(self):
        return self.balance

    def get_user_id(self):
        return self.accountNumber

    def get_username(self):
        return self.name

    def get_account_type(self):
        return self.accountType

    def get_transaction_history(self):
        try:
            with open(self.filename, 'r') as file:
                return file.read()
        except FileNotFoundError:
            return "Transaction history not found."

# Example usage:
account1 = BankAccount("John Doe", "checking", 1000)

print("Account 1: {} - Account Type: {} - Balance: ${}".format(account1.get_username(), account1.get_account_type(), account1.get_balance()))

# Deposit $500 into the account
account1.deposit(500)
print("Updated balance after deposit: ${}".format(account1.get_balance()))

# Withdraw $200 from the account
account1.withdraw(200)
print("Updated balance after withdrawal: ${}".format(account1.get_balance()))

# Retrieve and print transaction history
transaction_history = account1.get_transaction_history()
print("Transaction History:")
print(transaction_history)
