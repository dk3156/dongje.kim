---
title: "HW4"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

[Download hw4_output.txt](https://dk3156.github.io/experience/oop/homeworks/hw4_output.txt)
```
/*
  testNobleWarrior.cpp
  John Sterling
  Association: Noble - Warrior
  Test code for hw04
 
 author: Dongje Kim
 */

#include <iostream>
#include <vector>
#include <string>
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
    void battle(Noble& other) {
        cout << name << " battles " << other.name << endl;
       
        if(strength == 0 && other.strength == 0){
            cout << "Oh, NO! They're both dead! Yuck!" << endl;
        } else if(strength == 0){
            cout << "He's dead, " << other.name << endl;
        } else if(other.strength == 0){
            cout << "He's dead, " << name << endl;
        } else if(strength == other.strength){
            strength = 0;
            set_warrior_zero();
            other.strength = 0;
            other.set_warrior_zero();
            cout << "Mutual Annihilation: " << name << " and " << other.name << " die at each other's hands" << endl;
        } else if(strength > other.strength){
            double ratio = other.strength / strength;
            set_warrior_strength(ratio);
            other.strength = 0;
            other.set_warrior_zero();
            cout << name << " defeats " << other.name << endl;
        } else {
            double ratio = strength / other.strength;
            set_warrior_strength(ratio);
            strength = 0;
            set_warrior_zero();
            cout << other.name << " defeats " << name << endl;
        }
    }
    
    //this method represents hiring a warrior by noble.
    void hire(Warrior& war){
        size += 1;
        strength += war.get_strength();
        Warrior* war_ptr = &war;
        army.push_back(war_ptr);
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


int main() {
	
    Noble art("King Arthur");
    Noble lance("Lancelot du Lac");
    Noble jim("Jim");
    Noble linus("Linus Torvalds");
    Noble billie("Bill Gates");
	
    Warrior cheetah("Tarzan", 10);
    Warrior wizard("Merlin", 15);
    Warrior theGovernator("Conan", 12);
    Warrior nimoy("Spock", 15);
    Warrior lawless("Xena", 20);
    Warrior mrGreen("Hulk", 8);
    Warrior dylan("Hercules", 3);
	
    jim.hire(nimoy);
    lance.hire(theGovernator);
    art.hire(wizard);
    lance.hire(dylan);
    linus.hire(lawless);
    billie.hire(mrGreen);
    art.hire(cheetah);
	
    cout << "==========\n"
         << "Status before all battles, etc.\n";
    cout << jim << endl;
    cout << lance << endl;
    cout << art << endl;
    cout << linus << endl;
    cout << billie << endl;
    cout << "==========\n";
	
    art.fire(cheetah);
    cout << art << endl;
	
    art.battle(lance);
    jim.battle(lance);
    linus.battle(billie);
    billie.battle(lance);

    cout << "==========\n"
         << "Status after all battles, etc.\n";
    cout << jim << endl;
    cout << lance << endl;
    cout << art << endl;
    cout << linus << endl;
    cout << billie << endl;
    cout << "==========\n";
	
}
```