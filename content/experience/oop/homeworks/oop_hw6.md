---
title: "HW6"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

[Download nobleWarrior.txt](https://dk3156.github.io/experience/oop/homeworks/nobleWarrior.txt)

## Main.cpp
```
//
//  main.cpp
//  hw06
//
//  Created by DJ on 4/12/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#include "Noble.h"
#include "Warrior.h"
#include <iostream>
#include <fstream>
using namespace std;
using namespace WarriorCraft;

void read_file();
bool warrior(const string& name, const double strength, vector<Warrior*>& war_vec);
bool noble(const string& name, vector<Noble*>& noble_vec);
bool hire(const string& noble_name, const string& war_name, vector<Noble*>& noble_vec, vector<Warrior*>& war_vec);
bool fire(const string& noble_name, const string& war_name, vector<Noble*>& noble_vec, vector<Warrior*>& war_vec);
void status(const vector<Noble*>& noble_vec, const vector<Warrior*>& war_vec);
bool battle(const string& name1, const string& name2, vector<Noble*>& noble_vec);
void clear(vector<Noble*>& noble_vec, vector<Warrior*>& war_vec);
size_t check_noble(const string&, vector<Noble*>&);
size_t check_warrior(const string&, vector<Warrior*>&);


int main() {
    read_file();
}

void read_file(){
    ifstream my_file("nobleWarriors.txt");
    if (!my_file) {
        cerr << "failed to open the file" << endl;
        exit(1);
    }
    
    vector<Noble*> noble_vec;
    vector<Warrior*> war_vec;
    string command;
    string name1;
    string name2;
    double strength;
    
    while(my_file >> command){
        if (command == "Warrior"){
            my_file >> name1 >> strength ;
            warrior(name1, strength, war_vec);
        } else if (command == "Noble"){
            my_file >> name1;
            noble(name1, noble_vec);
        } else if (command == "Hire"){
            my_file >> name1 >> name2;
            hire(name1, name2, noble_vec, war_vec);
        } else if (command == "Fire"){
            my_file >> name1 >> name2;
            fire(name1, name2, noble_vec, war_vec);
        } else if (command == "Status"){
            status(noble_vec, war_vec);
        } else if (command == "Battle"){
            my_file >> name1 >> name2;
            battle(name1, name2, noble_vec);
        } else if (command == "Clear"){
            clear(noble_vec, war_vec);
        }
    }
    
    my_file.close();
}

bool warrior(const string& name, const double strength, vector<Warrior*>& war_vec){
    size_t len = war_vec.size();
    for (size_t i = 0; i < len; ++i){
        if(war_vec[i]->get_name() == name){
            return false;
        }
    }
    war_vec.push_back(new Warrior(name, strength));
    return true;
}

bool noble(const string& name, vector<Noble*>& noble_vec){
    size_t len = noble_vec.size();
    for (size_t i = 0; i < len; ++i){
        if (noble_vec[i]->get_name() == name){
            return false;
        }
    }
    noble_vec.push_back(new Noble(name));
    return true;
}

bool hire(const string& noble_name, const string& war_name, vector<Noble*>& noble_vec, vector<Warrior*>& war_vec){
    size_t ind_noble = check_noble(noble_name, noble_vec);
    size_t ind_war = check_warrior(war_name, war_vec);
    if (ind_noble != noble_vec.size() && ind_war != war_vec.size()){
        return(noble_vec[ind_noble]->hire(war_vec[ind_war]));
    }
    cerr << "Attempting to hire using unknown warrior: " << war_name << endl;
    return false;
}

bool fire(const string& noble_name, const string& war_name, vector<Noble*>& noble_vec, vector<Warrior*>& war_vec){
    size_t ind_noble = check_noble(noble_name, noble_vec);
    size_t ind_war = check_warrior(war_name, war_vec);
    if (ind_noble != noble_vec.size() && ind_war != war_vec.size()){
        return(noble_vec[ind_noble]->fire(war_vec[ind_war]));
    }
    return false;
}

void status(const vector<Noble*>& noble_vec, const vector<Warrior*>& war_vec){
    cout << "Status" << endl << "=====" << endl << "Nobles:" << endl;
    size_t len_noble = noble_vec.size();
    size_t len_war = war_vec.size();
    bool noble_exists = false;
    for (size_t i = 0; i < len_noble; ++i){
        cout << *noble_vec[i] << endl;
        noble_exists = true;
    }
    if (!noble_exists){
        cout << "NONE" << endl;
    }
    bool employed_war = false;
    cout << "Unemployed Warriors:" << endl;
    for (size_t i = 0; i < len_war; ++i){
        if(!war_vec[i]->get_employed()){
            cout << *war_vec[i] << endl;
            employed_war = true;
        }
    }
    if (!employed_war){
        cout << "NONE" << endl;
    }
}

bool battle(const string& name1, const string& name2, vector<Noble*>& noble_vec){
    size_t len = noble_vec.size();
    size_t i = check_noble(name1, noble_vec);
    size_t j = check_noble(name2, noble_vec);
    if (i == len || j == len){
        cerr << "Attempt to battle using unknown noble" << endl;
        return false;
    } else {
        noble_vec[i]->battle(noble_vec[j]);
    }
    return true;
}

void clear(vector<Noble*>& noble_vec, vector<Warrior*>& war_vec){
    size_t len_noble = noble_vec.size();
    size_t len_war = war_vec.size();
    for(size_t i = 0; i < len_noble; ++i){
        delete noble_vec[i];
    }
    for(size_t i = 0; i < len_war; ++i){
        delete war_vec[i];
    }
    noble_vec.clear();
    war_vec.clear();
}

//this function returns the index of the warrior specified by its name in the warrior vector
size_t check_warrior(const string& war_name, vector<Warrior*>& war_vec){
    for (size_t i = 0; i < war_vec.size(); ++i){
        if(war_vec[i]->get_name() == war_name){
            return i;
        }
    }
    return war_vec.size();
}

//this function returns the index of the noble specified by its name in the noble vector
size_t check_noble(const string& noble_name, vector<Noble*>& noble_vec){
    size_t len = noble_vec.size();
    for (size_t i = 0; i < len; ++i){
        if (noble_vec[i]->get_name() == noble_name){
            return i;
        }
    }
    return noble_vec.size();
}
```

## Noble.cpp
```
//
//  Noble.cpp
//  hw06
//
//  Created by DJ on 4/12/22.
//  Copyright © 2022 DJ. All rights reserved.
//


#include "Warrior.h"
#include "Noble.h"
#include <iostream>
#include <string>
using namespace std;

namespace WarriorCraft{
    ostream& operator<< (ostream& os, const Noble& rhs){
        os << rhs.name << " has an army of " << rhs.army.size() << endl;
        for (size_t i = 0; i < rhs.army.size(); ++i){
          os << '\t' << *rhs.army[i] << endl;
        }
        return os;
    }

    Noble::Noble(const string& name): name(name){
        strength = 0;
    }

    const string& Noble::get_name() const{
        return name;
    }

    void Noble::set_warrior_strength(double ratio){
        for(size_t i = 0; i < army.size(); ++i){
                army[i]->set_strength(ratio);
        }
        strength -= ratio*strength;
    }

    void Noble::battle(Noble* other) {
        cout << name << " battles " << other->name << endl;
        double other_strg = other->get_strength();
            
            if(strength == 0 && other_strg == 0){
                cout << "Oh, NO! They're both dead! Yuck!" << endl;
            } else if(strength == 0){
                cout << "He's dead, " << other->name << endl;
            } else if(other_strg == 0){
                cout << "He's dead, " << name << endl;
            } else if(strength == other_strg){
                this->set_warrior_strength(1);//setting all army's strength to zero with ratio 1
                other->set_warrior_strength(1);
                cout << "Mutual Annihilation: " << name << " and " << other->name << " die at each other's hands" << endl;
            } else if(strength > other_strg){
                double ratio = other_strg/ strength;
                this ->set_warrior_strength(ratio);
                other->set_warrior_strength(1);
                cout << name << " defeats " << other->name << endl;
            } else {
                double ratio = strength / other_strg;
                other->set_warrior_strength(ratio);
                this->set_warrior_strength(1);
                cout << other->name << " defeats " << name << endl;
            }
    }

    bool Noble::hire(Warrior* war){
        if (!war->get_employed()){
            army.push_back(war);
            war->switch_employed();
            war->set_owner(this);
            strength += war->get_strength();
            return true;
        }
        cerr << "Attempt to hire " << war->get_name() << " by " << name << " failed!" << endl;
        return false;
    }

    bool Noble::fire(Warrior* war){
        size_t ind_war = find_warrior(war->get_name());
        if(ind_war != army.size()){
            army[ind_war]->switch_employed();
            army[ind_war]->set_owner(nullptr);
            strength -= war->get_strength();
            Warrior* war_copy = army[ind_war];
            army[ind_war] = army[army.size()-1];
            army[army.size()-1] = war_copy;
            army.pop_back();
            cout << "You don't work for me anymore " << war->get_name() << " -- " << name << endl;
            return true;
        }
        return false;
    }

    size_t Noble::find_warrior(const string& war){
        for (size_t i = 0; i < army.size(); ++i){
            if(army[i]->get_name() == war){
                return i;
            }
        }
        return army.size();
    }

    double Noble::get_strength(){
        return strength;
    }
}
```

## Noble.h
```
#ifndef Noble_h
#define Noble_h
#include <iostream>
#include <string>
#include <vector>

namespace WarriorCraft{

    class Warrior;

    class Noble{
        friend std::ostream& operator<< (std::ostream& os, const Noble& rhs);
    
    public:
        Noble(const std::string& name);
        const std::string& get_name() const;
        void set_warrior_strength(double ratio);
        void battle(Noble* other);
        bool hire(Warrior* war);
        bool fire(Warrior* war);
        size_t find_warrior(const std::string& war);
        double get_strength();
        
    private:
        std::string name;
        double strength;
        std::vector<Warrior*> army;
    };
}
```

## Warrior.cpp
```
#include "Warrior.h"
#include "Noble.h"
#include <iostream>
#include <string>
using namespace std;

namespace WarriorCraft{

        ostream& operator<< (ostream& os, const Warrior& rhs){
            os << rhs.name << ": " << rhs.strength;
            return os;
        }
        
        Warrior::Warrior(const string& name, double strength): name(name), strength(strength){
            employed = false;
            owner = nullptr;
        }
        const string& Warrior::get_name() const {
            return name;
        }
        double Warrior::get_strength() const {
            return strength;
        }
        void Warrior::set_strength(double ratio){
            strength -= ratio*strength;
        }
        void Warrior::switch_employed(){
            employed = !employed;
        }
        bool Warrior::get_employed() const {
            return employed;
        }
        void Warrior::run_away(){
            switch_employed();
            owner->fire(this);
            owner = nullptr;
        }
        void Warrior::set_owner(Noble* noble){
            owner = noble;
        }
    }
```

## Warrior.h
```
#ifndef Warrior_h
#define Warrior_h
#include <string>
#include <iostream>

namespace WarriorCraft {
    class Noble;

    class Warrior{
        friend std::ostream& operator<< (std::ostream& os, const Warrior& rhs);
        
    public:

        Warrior(const std::string& name, double strength);
        const std::string& get_name() const;
        double get_strength() const;
        void set_strength(double ratio);
        void switch_employed();
        bool get_employed() const;
        void run_away();
        void set_owner(Noble* noble);
        
    private:
        std::string name;
        double strength;
        bool employed;
        Noble* owner;
    };
}

#endif /* Warrior_h */
```