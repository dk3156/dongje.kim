---
title: "Lab3"
date: Homework
mainSectionTitle: "ptc"
---

## Answers
```
/*
  rec03_start.cpp
 */

// Provided
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
using namespace std;

// Task 1
// Define an Account struct
struct Account{
    string name;
    int number;
};

// Task 2
// Define an Account class (use a different name than in Task 1)
class Account2{
friend ostream& operator<<(ostream&, const Account2&);
    
public:
    Account2(const string& the_name, const int& the_number): name(the_name), number(the_number){}
    
    const string& get_name() const {return name;}
    int get_number() const {return number;}
private:
    string name;
    int number;
};

// Task 3
// Define an Account (use a different name than in Task 1&2) and
// Transaction classes

class Transaction3{
    friend ostream& operator<<(ostream&, const Transaction3&);
    
public:
    Transaction3(int the_amount, int the_sign): amount(the_amount), sign(the_sign){}
    int get_amount() const {return amount;}
    int get_sign() const {return sign;}
private:
    //amount of withdraw or deposit
    int amount;
    //if withdraw, -1, if deposit, +1
    int sign;
};


class Account3{
    friend ostream& operator<<(ostream&, const Account3&);
    
public:
    Account3(const string& the_name, int the_number): name(the_name), number(the_number){}
    void deposit(int amount){
        trans.emplace_back(amount, 1);
        balance += amount;
    }
    void withdraw(int amount){
        trans.emplace_back(amount, -1);
        balance -= amount;
    }
    
    const string& get_name() const {return name;}
    int get_number() const {return number;}
    int get_balance() const{return balance;}
    
private:
    string name;
    int number;
    vector<Transaction3> trans;
    int balance = 0;
};


ostream& operator<< (ostream& os, const Account3& acc){
    os << "Account " << acc.name << " " << acc.number << endl;
    os << "transaction history: " << endl;
    for(size_t i = 0; i < acc.trans.size(); ++i){
        os << acc.trans[i];
    }
    os << "balance remained: " << acc.balance << endl;
    
    return os;
}

ostream& operator<< (ostream& os, const Transaction3& trans){
    if(trans.sign == -1){
        os << trans.amount << " withdrawn." << endl;
    } else {
        os << trans.amount << " deposited." << endl;
    }
    
    return os;
}


// Task 4
// Define an Account with a nested public Transaction class
// (use different names than in the previous Tasks)

class Account4{
    friend ostream& operator<<(ostream&, const Account4&);
    
public:
    class Transaction4{
        friend ostream& operator<<(ostream&, const Transaction4&);
        
        Transaction4(int the_amount, int the_sign): amount(the_amount), sign(the_sign){}
        int get_amount() const {return amount;}
        int get_sign() const {return sign;}
    private:
        //amount of withdraw or deposit
        int amount;
        //if withdraw, -1, if deposit, +1
        int sign;
    };
    
    Account4(const string& the_name, int the_number, int the_amount, int the_sign): name(the_name), number(the_number){}
    void deposit(int amount){
        trans.emplace_back(amount, 1);
        balance += amount;
    }
    void withdraw(int amount){
        if(balance - amount < 0){
            cerr << " Not enough balance" << endl;
            return;
        }
        trans.emplace_back(amount, -1);
        balance -= amount;
    }
    
    const string& get_name() const {return name;}
    int get_number() const {return number;}
    int get_balance() const{return balance;}
    
private:
    string name;
    int number;
    vector<Transaction4> trans;
    int balance = 0;
};


ostream& operator<< (ostream& os, const Account4& acc){
    os << "Account " << acc.name << " " << acc.number << endl;
    os << "transaction history: " << endl;
    for(size_t i = 0; i < acc.trans.size(); ++i){
        os << acc.trans[i];
    }
    os << "balance remained: " << acc.balance << endl;
    return os;
}

ostream& operator<< (ostream& os, const Account4::Transaction4& trans){
    if(trans.sign == -1){
        os << trans.amount << " withdrawn." << endl;
    } else {
        os << trans.amount << " deposited." << endl;
    }
    
    return os;
}




// Task 5
// Define an Account with a nested private Transaction class
// (use different names than in the previous Tasks)


size_t find_account(const vector<Account3>& vec, int account_num);

int main() {

    // Task 1: account as struct
    //      1a
    cout << "Task1a:\n";
    
    string name;
    int number;
    vector<Account> acc_vec;
    
    ifstream my_file("accounts.txt");
    if (!my_file){
        cerr << "Error: the file does not exist" << endl;
    }
    while (my_file >> name >> number){
        Account acc;
        acc.name = name;
        acc.number = number;
        acc_vec.push_back(acc);
    }
    my_file.close();
    
    for(size_t i = 0; i < acc_vec.size(); ++i){
        cout << acc_vec[i].name << " " << acc_vec[i].number << endl;
    }

    //      1b
    cout << "Task1b:\n";
    cout << "Filling vector of struct objects, using {} initialization:\n";
    
    acc_vec.clear();
    my_file.open("accounts.txt");
    while (my_file >> name >> number){
        Account acc2 {name, number};
        acc_vec.push_back(acc2);
    }
    my_file.close();
    
    for(size_t i = 0; i < acc_vec.size(); ++i){
        cout << acc_vec[i].name << " " << acc_vec[i].number << endl;
    }

    //==================================
    // Task 2: account as class

    //      2a
    cout << "==============\n";
    cout << "\nTask2a:";
    cout << "\nFilling vector of class objects, using local class object:\n";
    
    vector<Account2> acc2_vec;
    my_file.open("accounts.txt");
    while (my_file >> name >> number){
        Account2 acc2(name, number);
        acc2_vec.push_back(acc2);
    }
    my_file.close();
    
    for(size_t i = 0; i < acc2_vec.size(); ++i){
        cout << acc2_vec[i].get_name() << " " << acc2_vec[i].get_number() << endl;
    }
    
    cout << "\nTask2b:\n";
    cout << "output using output operator with getters\n";
    //line 162
    for(size_t i = 0; i < acc2_vec.size(); ++i){
        cout << acc2_vec[i] << endl;
    }
    
    cout << "\nTask2c:\n";
    cout << "output using output operator as friend without getters\n";
    //line 166
    for(size_t i = 0; i < acc2_vec.size(); ++i){
        cout << acc2_vec[i] << endl;
    }

    cout << "\nTask2d:\n";
    cout << "Filling vector of class objects, using temporary class object:\n";
    acc2_vec.clear();
    my_file.open("accounts.txt");
    while (my_file >> name >> number){
        acc2_vec.push_back(Account2(name, number));
    }
    my_file.close();
    
    for(size_t i = 0; i < acc2_vec.size(); ++i){
        cout << acc2_vec[i] << endl;
    }
    
    cout << "\nTask2e:\n";
    cout << "Filling vector of class objects, using emplace_back:\n";
    acc2_vec.clear();
    my_file.open("accounts.txt");
    while (my_file >> name >> number){
        acc2_vec.emplace_back(name, number);
    }
    my_file.close();
    
    for(size_t i = 0; i < acc2_vec.size(); ++i){
        cout << acc2_vec[i] << endl;
    }
    
    cout << "==============\n";
    cout << "\nTask 3:\nAccounts and Transaction:\n";

    ifstream my_file2;
    vector<Account3> acc3_vec;
    string command;
    int account_num;
    int amount;
    size_t index;
    my_file.open("transactions.txt");
    if(!my_file){
        cerr << "No such file exists" << endl;
    }
    while (my_file >> command){
        if (command == "Account"){
            my_file >> name >> number;
            Account3 acc(name, number);
            acc3_vec.push_back(acc);
        } else if (command == "Deposit"){
            my_file >> account_num >> amount;
            index = find_account(acc3_vec, account_num);
            if(index == acc3_vec.size()){
                cerr << "No such account number exists." << endl;
            } else {
                acc3_vec[index].deposit(amount);
            }
        } else if (command == "Withdraw"){
            my_file >> account_num >> amount;
            index = find_account(acc3_vec, account_num);
            if(index == acc3_vec.size()){
                cerr << "No such account number exists." << endl;
            } else {
                acc3_vec[index].withdraw(amount);
            }
        }
    }
    for(size_t i = 0; i < acc3_vec.size(); ++i){
        cout << acc3_vec[i] << endl;
    }
    my_file.close();
    

    cout << "==============\n";
    cout << "\nTask 4:\nTransaction nested in public section of Account:\n";

    cout << "==============\n";
    cout << "\nTask 5:\nTransaction nested in private section of Account:\n";

    
}

//ostream& operator<< (ostream& os, const Account2& acc2){
//    os << acc2.get_name() << " " << acc2.get_number() << endl;
    
//    return os;
//}

size_t find_account(const vector<Account3>& vec, int account_num){
    for (size_t i = 0; i < vec.size(); ++i){
        if(vec[i].get_number() == account_num){
            return i;
        }
    }
    return vec.size();
}

ostream& operator<< (ostream& os, const Account2& acc2){
    os << acc2.name << " " << acc2.number << endl;
    
    return os;
    }
```