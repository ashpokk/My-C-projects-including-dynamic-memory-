# My-C-projects-including-dynamic-memory-
#include <iostream>
#include <vector>
#include <string>
#include <map>
#include <algorithm>
#include <fstream>
using namespace std;

class User {
private:
    const int size = 5;
    string* user_name;
    string* user_password;
    string current_user;

public:
    User() {
        user_name = new string[size]{"Ayesha", "Amna", "Fatima", "Hamza", "Usman"};
        user_password = new string[size]{"78UI", "79UI", "80UI", "81UI", "82UI"};
    }

    ~User() {
        delete[] user_name;
        delete[] user_password;
    }

    bool login() {
        string enteredname, enteredpass;
        cout << "Enter Username: ";
        cin >> enteredname;
        cout << "Enter Password: ";
        cin >> enteredpass;

        for (int i = 0; i < size; ++i) {
            if (enteredname == user_name[i]) {
                if (enteredpass == user_password[i]) {
                    cout << "Welcome, " << user_name[i] << endl;
                    current_user = user_name[i];
                    return true;
                } else {
                    cout << "Incorrect password for user: " << user_name[i] << endl;
                    char choice;
                    cout << "Do you want to change your password? (y/n): ";
                    cin >> choice;

                    if (choice == 'y' || choice == 'Y') {
                        cout << "Enter New Password: ";
                        cin >> user_password[i];
                        cout << "Password updated successfully!" << endl;
                    } else {
                        cout << "Exiting without changing password." << endl;
                    }
                    return false;
                }
            }
        }

        cout << "Username not found!" << endl;
        return false;
    }

    string getCurrentUser() const {
        return current_user;
    }
};

class Transaction {
protected:
    double amount_spent = 0;
    double amount_earned = 0;
    string date;
    string category;

public:
    virtual void input() = 0;
    virtual void display() = 0;
    virtual string getType() = 0;
    virtual double getAmount() const = 0;
    virtual string getDate() const = 0;
    virtual string getCategory() const = 0;
    virtual ~Transaction() {}
};

class Income : public Transaction {
public:
    void input() override {
        cout << "Enter the amount earned: ";
        cin >> amount_earned;
        cout << "Enter the date of the transaction (YYYY-MM-DD): ";
        cin >> date;
        cout << "Enter the category of transaction (monthly, weekly, yearly): ";
        cin >> category;
    }

    void display() override {
        cout << "[Income] Amount: $" << amount_earned
             << ", Date: " << date
             << ", Category: " << category << endl;
    }

    string getType() override {
        return "Income";
    }

    double getAmount() const override {
        return amount_earned;
    }

    string getDate() const override {
        return date;
    }

    string getCategory() const override {
        return category;
    }
};

class Expense : public Transaction {
public:
    void input() override {
        cout << "Enter the amount that was spent: ";
        cin >> amount_spent;
        cout << "Enter the date (YYYY-MM-DD): ";
        cin >> date;
        cout << "Enter the category on which it was spent (e.g., gym, grocery, shopping, rent): ";
        cin >> category;
    }

    void display() override {
        cout << "[Expense] Amount: $" << amount_spent
             << ", Date: " << date
             << ", Category: " << category << endl;
    }

    string getType() override {
        return "Expense";
    }

    double getAmount() const override {
        return amount_spent;
    }

    string getDate() const override {
        return date;
    }

    string getCategory() const override {
        return category;
    }
};

class Finance_Manager {
private:
    vector<Transaction*> transactions;
    double balance = 0;

public:
    void addIncome() {
        Income* inc = new Income();
        inc->input();
        transactions.push_back(inc);
        balance += inc->getAmount();
        cout << "Income added successfully! Current balance: $" << balance << endl;
        
            ofstream outFile("transactions.txt", ios::app); 
    if (outFile.is_open()) {
        outFile << "[Income] Amount: $" << inc->getAmount()
                << ", Date: " << inc->getDate()
                << ", Category: " << inc->getCategory() << endl;
        outFile.close();
    } else {
        cout << "Error: Could not open transactions.txt to write income." << endl;
    }
    }

    void addExpense() {
        Expense* exp = new Expense();
        exp->input();
        transactions.push_back(exp);
        balance -= exp->getAmount();
        cout << "Expense added successfully! Current balance: $" << balance << endl;
        
            ofstream outFile("transactions.txt", ios::app); 
    if (outFile.is_open()) {
        outFile << "[Expense] Amount: $" << exp->getAmount()
                << ", Date: " << exp->getDate()
                << ", Category: " << exp->getCategory() << endl;
        outFile.close();
    } else {
        cout << "Error: Could not open transactions.txt to write expense." << endl;
    }
    }

    void MonthlyReport() {
        string month;
        cout << "Enter month (YYYY-MM): ";
        cin >> month;

        double income = 0, expense = 0;

        for (Transaction* t : transactions) {
            string date = t->getDate();
            if (date.substr(0, 7) == month) {
                if (t->getType() == "Income") {
                    income += t->getAmount();
                } else {
                    expense += t->getAmount();
                }
            }
        }

        double savings = income - expense;

        cout << "Monthly Report for: " << month << endl;
        cout << "Total Income  : $" << income << endl;
        cout << "Total Expense : $" << expense << endl;
        cout << "Net Savings   : $" << savings << endl;
    }

    void YearlyReport() {
        string year;
        cout << "Enter year (YYYY): ";
        cin >> year;

        double income = 0, expense = 0;

        for (Transaction* t : transactions) {
            string date = t->getDate();
            if (date.substr(0, 4) == year) {
                if (t->getType() == "Income") {
                    income += t->getAmount();
                } else {
                    expense += t->getAmount();
                }
            }
        }

        double savings = income - expense;

        cout << "Yearly Report for " << year << endl;
        cout << "Total Income  : $" << income << endl;
        cout << "Total Expense : $" << expense << endl;
        cout << "Net Savings   : $" << savings << endl;
    }

    void showAllTransactions() {
        if (transactions.empty()) {
            cout << "No transactions found." << endl;
            return;
        }

        cout << "All Transactions:\n";
        for (Transaction* t : transactions) {
            t->display();
        }
    }

    vector<Transaction*> getTransactions() const {
        return transactions;
    }

    ~Finance_Manager() {
        for (Transaction* t : transactions) {
            delete t;
        }
    }
};

class SavingManager {
private:
    double savings;
    double fixed_expenses;
    double discretionary_expenses;
    double total_available;
    double total_needed;
    double shortfall;

public:
    void SavingsGoal(const vector<Transaction*>& transactions) {
        cout << "Enter your saving goal: ";
        cin >> savings;

        cout << "Enter fixed expenses (e.g., rent, bills): ";
        cin >> fixed_expenses;

        cout << "Enter discretionary expenses (approximate total): ";
        cin >> discretionary_expenses;

        total_available = fixed_expenses + discretionary_expenses;
        total_needed = savings + fixed_expenses;
        shortfall = total_needed - total_available;

        if (total_needed <= total_available) {
            cout << "You can meet your savings goal with your current budget!" << endl;
        } else {
            cout << "You need to reduce spending by $" << shortfall << " to hit your goal."<<endl;

            map<string, double> category_expense;
            for (Transaction* t : transactions) {
                if (t->getType() == "Expense") {
                    category_expense[t->getCategory()] += t->getAmount();  
                }
            }

            vector<pair<string, double>> sorted_categories(category_expense.begin(), category_expense.end());

            sort(sorted_categories.begin(), sorted_categories.end(),
                [](const pair<string, double>& a, const pair<string, double>& b) {
                    return a.second > b.second;
                });

            double remaining = shortfall;
            cout << "Suggested cuts:"<<endl;
            for (auto& pair : sorted_categories) {
                if (remaining <= 0) break;
                double cut = min(pair.second / 2, remaining);
                cout << "- Cut $" << cut << " from " << pair.first << endl;
                remaining -= cut;
            }

            if (remaining > 0)
                cout << "Still need to cut $" << remaining << ". Consider reviewing other expenses." << endl;
        }
    }
};


int main() {
    User u;
    Finance_Manager manager;

    char again = 'y';
    bool loginSuccess = false;

    while (again == 'y' || again == 'Y') {
        loginSuccess = u.login();
        if (loginSuccess)
            break;

        cout << "Do you want to try again? (y/n): ";
        cin >> again;
    }

    if (loginSuccess) {
        int choice;
        do {
            cout << "Finance Manager Menu:"<<endl;
            cout << "1. Add Income"<<endl;
            cout << "2. Add Expense"<<endl;
            cout << "3. View Reports or Transactions"<<endl;
              cout << "4. Analyze Savings Goal"<<endl;
            cout << "5. Exit"<<endl;
            cout << "Enter your choice (1-5): ";
            cin >> choice;

            switch (choice) {
                case 1:
                    manager.addIncome();
                    break;
                case 2:
                    manager.addExpense();
                    break;
                case 3: {
                    char choose;
                    bool validInput = false;

                    while (!validInput) {
                        cout << "Do you want to view (m)onthly, (y)early report or (a)ll transactions? ";
                        cin >> choose;

                        if (choose == 'm' || choose == 'M') {
                            manager.MonthlyReport();
                            validInput = true;
                        } else if (choose == 'y' || choose == 'Y') {
                            manager.YearlyReport();
                            validInput = true;
                        } else if (choose == 'a' || choose == 'A') {
                            manager.showAllTransactions();
                            validInput = true;
                        } else {
                            cout << "Invalid input. Please enter 'm', 'y' or 'a' only.";
                        }
                    }
                    break;
                }
                case 4: {
                    SavingManager sm;
                    sm.SavingsGoal(manager.getTransactions());
                    break;
                }
                case 5:
                    cout << "Exiting program. Thank you!" << endl;
                    break;
                
                default:
                    cout << "Invalid choice. Try again." << endl;
            }
        } while (choice != 4);
    }

    return 0;
}
