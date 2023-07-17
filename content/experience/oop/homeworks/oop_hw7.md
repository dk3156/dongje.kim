---
title: "HW7"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

## Main.cpp
```
//
//  main.cpp
//  hw07
//
//  Created by DJ on 4/13/22.
//
#include "Noble.h"
#include "Protector.h"
#include <iostream>
#include <fstream>
using namespace std;
using namespace WarriorCraft;

int main() {
    Lord sam("Sam");
    Archer samantha("Samantha", 200);
    sam.hires(samantha);
    Lord joe("Joe");
    PersonWithStrengthToFight randy("Randolf the Elder", 250);
    joe.battle(randy);
    joe.battle(sam);
    Lord janet("Janet");
    Swordsman hardy("TuckTuckTheHardy", 100);
    Swordsman stout("TuckTuckTheStout", 80);
    janet.hires(hardy);
    janet.hires(stout);
    PersonWithStrengthToFight barclay("Barclay the Bold", 300);
    janet.battle(barclay);
    janet.hires(samantha);
    Archer pethora("Pethora", 50);
    Archer thora("Thorapleth", 60);
    Wizard merlin("Merlin", 150);
    janet.hires(pethora);
    janet.hires(thora);
    sam.hires(merlin);
    janet.battle(barclay);
    sam.battle(barclay);
    joe.battle(barclay);
}
```

## Noble.cpp
```

#include "Protector.h"
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

    void Noble::battle(Noble& other) {
        cout << name << " battles " << other.name << endl;
        double other_strg = other.get_strength();
            
            if(strength == 0 && other_strg == 0){
                cout << "Oh, NO! They're both dead! Yuck!" << endl;
            } else if(strength == 0){
                cout << "He's dead, " << other.name << endl;
            } else if(other_strg == 0){
                cout << "He's dead, " << name << endl;
            } else if(strength == other_strg){
                set_warrior_strength(1);//setting all army's strength to zero with ratio 1
                other.set_warrior_strength(1);
                cout << "Mutual Annihilation: " << name << " and " << other->name << " die at each other's hands" << endl;
            } else if(strength > other_strg){
                double ratio = other_strg/ strength;
                set_warrior_strength(ratio);
                other.set_warrior_strength(1);
                cout << name << " defeats " << other->name << endl;
            } else {
                double ratio = strength / other_strg;
                other.set_warrior_strength(ratio);
                set_warrior_strength(1);
                cout << other.name << " defeats " << name << endl;
            }
    }

    bool Noble::hires(Protector& prot){
        if (!prot.get_employed()){
            army.push_back(&prot);
            prot.switch_employed();
            prot.set_owner(this);
            strength += prot.>get_strength();
            return true;
        }
        cerr << "Attempt to hire " << prot.get_name() << " by " << name << " failed!" << endl;
        return false;
    }

    bool Noble::fires(Protector& prot){
        size_t ind_war = find_warrior(prot.get_name());
        if(ind_war != army.size()){
            army[ind_war]->switch_employed();
            army[ind_war]->set_owner(nullptr);
            strength -= prot.get_strength();
            Protector* prot_copy = army[ind_war];
            army[ind_war] = army[army.size()-1];
            army[army.size()-1] = prot_copy;
            army.pop_back();
            cout << "You don't work for me anymore " << prot.get_name() << " -- " << name << endl;
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

    Lord::Lord(const string& name): Noble(name){}

    void Lord::battle(Noble& other){
        Noble::battle(other);
        for (size_t i = 0; i < army.size(); ++i){
            army[i]->defend();
        }
    }

    PersonWithStrengthtoFight::PersonWithStrengthToFight(const string& name, double strength): Noble(name), strength(strength){}

    void PersonWithStrengthToFight::battle(Noble& other){
        cout << "UGH!!" << endl;
        Noble::battle(other);
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
using namespace std;

namespace WarriorCraft{

    class Protector;

    class Noble{
        friend std::ostream& operator<< (std::ostream& os, const Noble& rhs);
        virtual void battle(Noble& other) = 0;
    
    public:
        Noble(const std::string& name);
        const std::string& get_name() const;
        void set_warrior_strength(double ratio);
        bool hires(Protector& prot);
        bool fires(Protector& prot);
        size_t find_warrior(const std::string& war);
        double get_strength();
        
    private:
        std::string name;
    protected: double strength;
        std::vector<Protector*> army;
    };

    class Lord : public Noble{
    public:
        Lord(const string& name);
        void battle(Noble& other);
    };

    class PersonWithStrengthToFight : public Noble{
    public:
        PersonWithStrengthToFight(const string& name, double strength);
        void battle(Noble& other);
    };
}
#endif /* Noble_h */
```

## Protector.cpp
```
#include "Protector.h"
#include "Noble.h"
#include <iostream>
#include <string>
using namespace std;

namespace WarriorCraft{

    ostream& operator<< (ostream& os, const Protectors& rhs){
        os << rhs.name << ": " << rhs.strength;
        return os;
    }
        
    Protector::Protector(const string& name, double strength): name(name), strength(strength){
        employed = false;
        owner = nullptr;
    }
    const string& Protector::get_name() const {
        return name;
    }
    double Protector::get_strength() const {
        return strength;
    }
    void Protector::set_strength(double ratio){
        strength -= ratio*strength;
    }
    void Protector::switch_employed(){
        employed = !employed;
    }
    bool Protector::get_employed() const {
        return employed;
    }
    void Protector::run_away(){
        switch_employed();
        owner->fire(this);
        owner = nullptr;
    }
    void Protector::set_owner(Noble* noble){
        owner = noble;
    }
    
    void Protector::defend(){}

    Wizard::Wizard(const string& name, double strength): Protector(name, strength){}
    void Wizard::defend(){
        cout << "POOF!" << endl;
    }

    Warrior::Warrior(const string& name, double strength): Protector(name, strength){}
    void Warrior::defend(){
        cout << name << " says: Take that in the name of my lord, " << owner->get_name() << endl;
    }

    Swordsman::Swordsman(const string& name, double strength): Protector(name, strength){}
    void Swordsman::defend(){
        cout << "CLANG" << " ";
        Warriors::defend();
    }

    Archer::Archer(const string& name, double strength): Protector(name, strength){}
    void Archer::defend() override { 
        cout << "TWANG!" << " ";
        Warriors::defend();
    }
}
```

## Protector.h
```

#ifndef Protector_h
#define Protector_h
#include <string>
#include <iostream>

namespace WarriorCraft {
    class Noble;

    class Protector{
        friend std::ostream& operator<< (std::ostream& os, const Protector& rhs);
        virtual void defend() = 0;
        
    public:

        Protector(const std::string& name, double strength);
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

    class Wizard : public Protector{
    public:
        Wizard(const string& name, double strength);
        void defend();
    };

    class Warrior : public Protector{
    public:
        Warrior(const string& name, double strength);
        void defend();
    };

    class Swordsman : public Warrior{
    public:
        Swordsman(const string& name, double strength);
        void defend();
    };

    class Archer : public Warrior{
    public:
        Archer(const string& name, double strength);
        void defend();
    };
}

#endif /* Protector_h */
```