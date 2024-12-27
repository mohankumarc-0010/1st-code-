# 1st-code-
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_ACCOUNTS 100
#define MAX_NAME_LENGTH 50
#define MAX_ACCOUNTS 100
#define MAX_NAME_LENGTH 50
#define MAX_PASSWORD_LENGTH 20
#define MAX_HISTORY 100
#define MAX_TRANSACTION_LENGTH 200
// Structure to hold account information
typedef struct {
    int accountNumber;
    char accountHolderName[MAX_NAME_LENGTH];
    float balance;
} Account;

// Global array to hold accounts
Account accounts[MAX_ACCOUNTS];
int accountCount = 0;

// Function prototypes
void createAccount();
void deposit();
void withdraw();
void checkBalance();
void displayAccountDetails();
void displayAllAccounts();
int findAccount(int accountNumber);

int main() {
    int choice;
    
    // Menu loop
    do {
        printf("\n\n=== Banking System ===\n");
        printf("1. Create Account\n");
        printf("2. Deposit\n");
        printf("3. Withdraw\n");
        printf("4. Check Balance\n");
        printf("5. Display Account Details\n");
        printf("6. Display All Accounts\n");
        printf("7. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                createAccount();
                break;
            case 2:
                deposit();
                break;
            case 3:
                withdraw();
                break;
            case 4:
                checkBalance();
                break;
            case 5:
                displayAccountDetails();
                break;
            case 6:
                displayAllAccounts();
                break;
            case 7:
                printf("Exiting the system. Goodbye!\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 7);

    return 0;
}

// Function to create a new account
void createAccount() {
    if (accountCount >= MAX_ACCOUNTS) {
        printf("Account limit reached. Cannot create more accounts.\n");
        return;
    }

    Account newAccount;
    newAccount.accountNumber = accountCount + 1;  // Assign unique account number
    printf("Enter account holder name: ");
    getchar();  // To clear the newline character left by previous input
    fgets(newAccount.accountHolderName, MAX_NAME_LENGTH, stdin);
    newAccount.accountHolderName[strcspn(newAccount.accountHolderName, "\n")] = 0; // Remove the newline character

    newAccount.balance = 0.0f;  // Initial balance is 0

    accounts[accountCount] = newAccount;
    accountCount++;

    printf("Account created successfully! Account number: %d\n", newAccount.accountNumber);
}

// Function to deposit money into an account
void deposit() {
    int accountNumber;
    float amount;

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    int accountIndex = findAccount(accountNumber);
    if (accountIndex == -1) {
        printf("Account not found!\n");
        return;
    }

    printf("Enter amount to deposit: ");
    scanf("%f", &amount);

    if (amount <= 0) {
        printf("Invalid amount. Deposit must be positive.\n");
        return;
    }

    accounts[accountIndex].balance += amount;
    printf("Deposited %.2f to account number %d. New balance: %.2f\n",
            amount, accountNumber, accounts[accountIndex].balance);
}

// Function to withdraw money from an account
void withdraw() {
    int accountNumber;
    float amount;

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    int accountIndex = findAccount(accountNumber);
    if (accountIndex == -1) {
        printf("Account not found!\n");
        return;
    }

    printf("Enter amount to withdraw: ");
    scanf("%f", &amount);

    if (amount <= 0) {
        printf("Invalid amount. Withdrawal must be positive.\n");
        return;
    }

    if (accounts[accountIndex].balance < amount) {
        printf("Insufficient balance!\n");
        return;
    }

    accounts[accountIndex].balance -= amount;
    printf("Withdrew %.2f from account number %d. New balance: %.2f\n",
            amount, accountNumber, accounts[accountIndex].balance);
}

// Function to check balance of an account
void checkBalance() {
    int accountNumber;

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    int accountIndex = findAccount(accountNumber);
    if (accountIndex == -1) {
        printf("Account not found!\n");
        return;
    }

    printf("Account number: %d\n", accountNumber);
    printf("Account holder: %s\n", accounts[accountIndex].accountHolderName);
    printf("Balance: %.2f\n", accounts[accountIndex].balance);
}

// Function to display account details
void displayAccountDetails() {
    int accountNumber;

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    int accountIndex = findAccount(accountNumber);
    if (accountIndex == -1) {
        printf("Account not found!\n");
        return;
    }

    printf("\nAccount Details\n");
    printf("=================\n");
    printf("Account number: %d\n", accountNumber);
    printf("Account holder: %s\n", accounts[accountIndex].accountHolderName);
    printf("Balance: %.2f\n", accounts[accountIndex].balance);
}

// Function to display all accounts
void displayAllAccounts() {
    if (accountCount == 0) {
        printf("No accounts available.\n");
        return;
    }

    printf("\nAll Accounts\n");
    printf("=================\n");
    for (int i = 0; i < accountCount; i++) {
        printf("Account number: %d\n", accounts[i].accountNumber);
        printf("Account holder: %s\n", accounts[i].accountHolderName);
        printf("Balance: %.2f\n", accounts[i].balance);
        printf("----------------------\n");
    }
}

// Function to find an account by its account number
int findAccount(int accountNumber) {
    for (int i = 0; i < accountCount; i++) {
        if (accounts[i].accountNumber == accountNumber) {
            return i;
        }
    }
    return -1;  // Account not found
// Structure to hold account information
typedef struct {
    int accountNumber;
    char accountHolderName[MAX_NAME_LENGTH];
    char password[MAX_PASSWORD_LENGTH];
    float balance;
    int isLocked; // Lock account after too many failed login attempts
    char transactionHistory[MAX_HISTORY][MAX_TRANSACTION_LENGTH];
    int historyCount;
} Account;

// Global array to hold accounts
Account accounts[MAX_ACCOUNTS];
int accountCount = 0;

// Max allowed invalid login attempts
#define MAX_INVALID_ATTEMPTS 3

// Function prototypes
void createAccount();
int login(int *accountIndex);
void deposit(int accountIndex);
void withdraw(int accountIndex);
void checkBalance(int accountIndex);
void displayAccountDetails(int accountIndex);
void displayAllAccounts();
void recordTransaction(int accountIndex, const char *transactionDetails);
int findAccount(int accountNumber);
void viewTransactionHistory(int accountIndex);

// Main Menu
int main() {
    int choice;
    int accountIndex;
    
    // Menu loop
    do {
        printf("\n\n=== Banking System ===\n");
        printf("1. Create Account\n");
        printf("2. Login\n");
        printf("3. Display All Accounts\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                createAccount();
                break;
            case 2:
                if (login(&accountIndex) == 0) {
                    int subChoice;
                    do {
                        printf("\n\n=== Account Menu ===\n");
                        printf("1. Deposit\n");
                        printf("2. Withdraw\n");
                        printf("3. Check Balance\n");
                        printf("4. View Transaction History\n");
                        printf("5. Display Account Details\n");
                        printf("6. Logout\n");
                        printf("Enter your choice: ");
                        scanf("%d", &subChoice);

                        switch (subChoice) {
                            case 1:
                                deposit(accountIndex);
                                break;
                            case 2:
                                withdraw(accountIndex);
                                break;
                            case 3:
                                checkBalance(accountIndex);
                                break;
                            case 4:
                                viewTransactionHistory(accountIndex);
                                break;
                            case 5:
                                displayAccountDetails(accountIndex);
                                break;
                            case 6:
                                printf("Logging out...\n");
                                break;
                            default:
                                printf("Invalid choice. Please try again.\n");
                        }
                    } while (subChoice != 6);
                }
                break;
            case 3:
                displayAllAccounts();
                break;
            case 4:
                printf("Exiting the system. Goodbye!\n");
                break;
            default:
                printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 4);

    return 0;
}

// Function to create a new account
void createAccount() {
    if (accountCount >= MAX_ACCOUNTS) {
        printf("Account limit reached. Cannot create more accounts.\n");
        return;
    }

    Account newAccount;
    newAccount.accountNumber = accountCount + 1;  // Assign unique account number
    printf("Enter account holder name: ");
    getchar();  // To clear the newline character left by previous input
    fgets(newAccount.accountHolderName, MAX_NAME_LENGTH, stdin);
    newAccount.accountHolderName[strcspn(newAccount.accountHolderName, "\n")] = 0; // Remove the newline character

    // Set password
    printf("Enter a password (max 20 characters): ");
    fgets(newAccount.password, MAX_PASSWORD_LENGTH, stdin);
    newAccount.password[strcspn(newAccount.password, "\n")] = 0; // Remove the newline character

    newAccount.balance = 0.0f;  // Initial balance is 0
    newAccount.isLocked = 0;  // Account is not locked by default
    newAccount.historyCount = 0;  // No transaction history initially

    accounts[accountCount] = newAccount;
    accountCount++;

    printf("Account created successfully! Account number: %d\n", newAccount.accountNumber);
}

// Function to log in to an account
int login(int *accountIndex) {
    int accountNumber;
    char password[MAX_PASSWORD_LENGTH];
    int invalidAttempts = 0;

    printf("Enter account number: ");
    scanf("%d", &accountNumber);

    *accountIndex = findAccount(accountNumber);
    if (*accountIndex == -1) {
        printf("Account not found!\n");
        return -1;
    }

    if (accounts[*accountIndex].isLocked) {
        printf("This account is locked due to too many failed login attempts.\n");
        return -1;
    }

    printf("Enter password: ");
    getchar();  // To consume the newline character left by previous input
    fgets(password, MAX_PASSWORD_LENGTH, stdin);
    password[strcspn(password, "\n")] = 0; // Remove the newline character

    // Check password
    while (strcmp(accounts[*accountIndex].password, password) != 0) {
        invalidAttempts++;
        if (invalidAttempts >= MAX_INVALID_ATTEMPTS) {
            accounts[*accountIndex].isLocked = 1;
            printf("Account locked due to too many invalid attempts.\n");
            return -1;
        }
        printf("Incorrect password. Try again: ");
        fgets(password, MAX_PASSWORD_LENGTH, stdin);
        password[strcspn(password, "\n")] = 0; // Remove the newline character
    }

    printf("Login successful!\n");
    return 0;
}

// Function to deposit money into an account
void deposit(int accountIndex) {
    float amount;

    printf("Enter amount to deposit: ");
    scanf("%f", &amount);

    if (amount <= 0) {
        printf("Invalid amount. Deposit must be positive.\n");
        return;
    }

    accounts[accountIndex].balance += amount;
    recordTransaction(accountIndex, "Deposited money");

    printf("Deposited %.2f to your account. New balance: %.2f\n", amount, accounts[accountIndex].balance);
}

// Function to withdraw money from an account
void withdraw(int accountIndex) {
    float amount;

    printf("Enter amount to withdraw: ");
    scanf("%f", &amount);

    if (amount <= 0) {
        printf("Invalid amount. Withdrawal must be positive.\n");
        return;
    }

    if (accounts[accountIndex].balance < amount) {
        printf("Insufficient balance!\n");
        return;
    }

    accounts[accountIndex].balance -= amount;
    recordTransaction(accountIndex, "Withdrew money");

    printf("Withdrew %.2f from your account. New balance: %.2f\n", amount, accounts[accountIndex].balance);
}

// Function to check balance of an account
void checkBalance(int accountIndex) {
    printf("Your current balance: %.2f\n", accounts[accountIndex].balance);
}

// Function to display account details
void displayAccountDetails(int accountIndex) {
    printf("\nAccount Details\n");
    printf("=================\n");
    printf("Account number: %d\n", accounts[accountIndex].accountNumber);
    printf("Account holder: %s\n", accounts[accountIndex].accountHolderName);
    printf("Balance: %.2f\n", accounts[accountIndex].balance);
}

// Function to view transaction history of an account
void viewTransactionHistory(int accountIndex) {
    if (accounts[accountIndex].historyCount == 0) {
        printf("No transactions yet.\n");
    } else {
        printf("\nTransaction History:\n");
        for (int i = 0; i < accounts[accountIndex].historyCount; i++) {
            printf("%d: %s\n", i + 1, accounts[accountIndex].transactionHistory[i]);
        }
    }
}

// Function to record a transaction in the account's history
void recordTransaction(int accountIndex, const char *transactionDetails) 
{
    if (accounts[accountIndex].historyCount < MAX_HISTORY) 
{
        snprintf(accounts[accountIndex].transactionHistory[accounts[accountIndex].historyCount],
                 MAX_TRANSACTION_LENGTH, "%s", transactionDetails);
        accounts[accountIndex].historyCount++;
    }
}

// Function to display all accounts
void displayAllAccounts() {
    if (accountCount == 0) {
        printf("No accounts available.\n");
        return;
    }

    printf("\nAll Accounts\n");
    printf("=================\n");
    for (int i = 0; i < accountCount; i++) {
        printf("Account number: %d\n", accounts[i].accountNumber);
        printf("Account holder: %s\n", accounts[i].accountHolderName);
        printf("Balance: %.2f\n", accounts[i].balance);
        if (accounts[i].isLocked) {
            printf("Account is locked due to too many failed login attempts.\n");
        }
        printf("----------------------\n");
    }
}

// Function to find an account by its account number
int findAccount(int accountNumber) {
    for (int i = 0; i < accountCount; i++) 
{
        if (accounts[i].accountNumber == accountNumber) 
{
            return
