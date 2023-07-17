---
title: "HW5"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

[Download hw5_output.txt](https://dk3156.github.io/experience/oop/homeworks/hw5_output.txt)

```
//
//  main.cpp
//  hw05
//
//  Created by DJ on 3/2/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#include <iostream>
#include <vector>
#include <string>
#include <fstream>
using namespace std;

/*
 * This class represents warrior.
 * member variable: name and strength
 */
class Warrior{
    
    friend ostream& operator<< (ostream& os, const Warrior* rhs){
        os << rhs->name << ": " << rhs->strength;
        return os;
    }
    
public:
    //constructor
    Warrior(const string& name, double strength): name(name), strength(strength){}
    
    //below are two methods that return name and strength of the warrior object.
    string get_name() const {return name;}
    int get_strength() const {return strength;}
    
    
    //this method sets strength to zero
    void set_strength_zero() {strength = 0;}
    
    
    //this method sets strength to the reduced amount by the result of a battle
    //parameter: ratio
    void set_strength(double ratio) {strength -= ratio*strength;}
    
    
private:
    string name;
    double strength;
};


/*
 *this class represents Noble object.
 *member variables: name, army(vector of warrior pointers), strength(combined strength of the warriors), and size(numbers of remaining warriors)
 */
class Noble{
    friend ostream& operator<< (ostream& os, const Noble& rhs){
        os << rhs.name << " has an army of " << rhs.size;
        for (size_t i = 0; i < rhs.army.size(); ++i){
            if (rhs.army[i] == nullptr){
                continue;
            } else {os << endl << '\t' << rhs.army[i];}
        }
        return os;
    }
    
public:
    //constructor
    Noble(const string& name): name(name){}
    string get_name() const {return name;}
    
    //this method sets every remaining warrior's strength into zero
    void set_warrior_zero(){
        for(size_t i = 0; i < army.size(); ++i){
            if(army[i] != nullptr){
                army[i]->set_strength_zero();
            }
        }
    }
    
    //this method sets every remaining warrior's strength reduced by the value of ratio. ratio is defeated noble's strength divided by winning noble's strength.
    void set_warrior_strength(double ratio){
        for(size_t i = 0; i < army.size(); ++i){
            if(army[i] != nullptr){
                army[i]->set_strength(ratio);
            }
        }
    }
    
    //this method prints out the result of battles.
    void battle(Noble* other) {
        cout << name << " battles " << other->name << endl;
        
        if(strength == 0 && other->strength == 0){
            cout << "Oh, NO! They're both dead! Yuck!" << endl;
        } else if(strength == 0){
            cout << "He's dead, " << other->name << endl;
        } else if(other->strength == 0){
            cout << "He's dead, " << name << endl;
        } else if(strength == other->strength){
            strength = 0;
            set_warrior_zero();
            other->strength = 0;
            other->set_warrior_zero();
            cout << "Mutual Annihilation: " << name << " and " << other->name << " die at each other's hands" << endl;
        } else if(strength > other->strength){
            double ratio = other->strength / strength;
            set_warrior_strength(ratio);
            other->strength = 0;
            other->set_warrior_zero();
            cout << name << " defeats " << other->name << endl;
        } else {
            double ratio = strength / other->strength;
            set_warrior_strength(ratio);
            strength = 0;
            set_warrior_zero();
            cout << other->name << " defeats " << name << endl;
        }
    }
    
    //this method represents hiring a warrior by noble.
    void hire(Warrior& war){
        size += 1;
        strength += war.get_strength();
        army.push_back(&war);
    }
    
    //this method represents firing a warrior by noble.
    void fire(const Warrior& war){
        size_t index = this->find_warrior(war);
        if (index == army.size()){
            cerr << "Error: the warrior to be fired does not exist" << endl;
        } else {
            strength -= war.get_strength();
            size -= 1;
            army[index] = nullptr;
            cout << "You don't work for me anymore " << war.get_name() <<  "! -- " << name << "." << endl;
        }
    }
    
    //this method returns the index of the specified warrior object in the army vector. If the warrior does not exist, it returns the total size of army vector.
    size_t find_warrior(const Warrior& war){
        for (size_t i = 0; i < army.size(); ++i){
            if(army[i]->get_name() == war.get_name()){
                return i;
            }
        }
        return army.size();
        
    }
    
private:
    string name;
    vector<Warrior*> army;
    double strength = 0;
    int size = 0;
};

size_t check_noble(const string&, vector<Noble*>&);
size_t check_warrior(const string&, vector<Warrior*>&);

bool warrior_employed(const string&, const vector<string>&);

int main() {
    
    ifstream my_file("nobleWarriors.txt");
    if (!my_file) {
        cerr << "failed to open warriors.txt" << endl;
        exit(1);
    }
    vector<Noble*> noble_vec;
    vector<Warrior*> unemployed_war;
    vector<string> employed;
    
    
    string command;
    string noble_name;
    string war_name;
    double strength;
    string noble1;
    string noble2;

    while(my_file >> command){
        if (command == "Warrior"){
            my_file >> war_name >> strength ;
            unemployed_war.push_back(new Warrior(war_name, strength));
        } else if (command == "Noble"){
            my_file >> noble_name;
            noble_vec.push_back(new Noble(noble_name));
        } else if (command == "Hire"){
            my_file >> noble_name >> war_name;
          
            if (warrior_employed(war_name, employed)){
                cerr << "Attempting to hire " << war_name << " by " << noble_name << " failed!" << endl;
            } else {
                size_t i = check_warrior(war_name, unemployed_war);
                size_t j = check_noble(noble_name, noble_vec);
                
                if (i == unemployed_war.size()){
                    cerr << "Attempt to hire using unknown warrior: " << war_name << endl;
                } else if (j == noble_vec.size()){
                    cerr << "Attempt to hire using unknown noble: " << noble_name << endl;
                } else {
                    noble_vec[j]->hire(*unemployed_war[i]);
                    employed.push_back(unemployed_war[i]->get_name());
                    delete unemployed_war[i];
                    unemployed_war[i] = nullptr;
                }
            }
        
            
        } else if (command == "Status"){
            cout << "=====" << endl << "Nobles:" << endl;
            for (size_t i = 0; i < noble_vec.size(); ++i){
                cout << *noble_vec[i] << endl;
            }
            cout << "Unemployed Warriors:" << endl;
            size_t size = 0;
            for (size_t i = 0; i < unemployed_war.size(); ++i){
                if (unemployed_war[i] != nullptr){
                    size++;
                }
            }
            cout << size << endl;
            
        } else if (command == "Battle"){
            my_file >> noble1 >> noble2;
            size_t i = check_noble(noble1, noble_vec);
            size_t j = check_noble(noble2, noble_vec);
            
            if (i == noble_vec.size() || j == noble_vec.size()){
                cerr << "Attempt to battle using unknown noble" << endl;
            } else {
                noble_vec[i]->battle(noble_vec[j]);
            }
        }
    }
    noble_vec.clear();
    unemployed_war.clear();
    my_file.close();
}

size_t check_warrior(const string& war_name, vector<Warrior*>& war_vec){
    for (size_t i = 0; i < war_vec.size(); ++i){
        if(war_vec[i] != nullptr && war_vec[i]->get_name() == war_name){
            return i;
        }
    }
    return war_vec.size();
}

size_t check_noble(const string& noble_name, vector<Noble*>& noble_vec){
    for (size_t i = 0; i < noble_vec.size(); ++i){
        if (noble_vec[i]->get_name() == noble_name){
            return i;
        }
    }
    return noble_vec.size();
}

bool warrior_employed(const string& war_name, const vector<string>& employed){
    for (size_t i = 0; i < employed.size(); ++i){
        if(employed[i] == war_name){
            return true;
        }
    }
    return false;
}
```