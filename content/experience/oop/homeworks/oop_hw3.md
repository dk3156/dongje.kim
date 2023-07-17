---
title: "HW3"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

[Download warriors.txt](https://dk3156.github.io/experience/oop/homeworks/hw3_warriors.txt)
```
//
//  main.cpp
//  hw03
//
//  Created by DJ on 2/16/22.
//  This file reads warriors.txt and prints information about the warriors.
//

#include <iostream>
#include <vector>
#include <fstream>
using namespace std;

class Warrior{
    
    friend ostream& operator<< (ostream& os, const Warrior& rhs){
        os << "Warrior: " << rhs.name << ", " << rhs.weapon;
        return os;
    }
    
    class Weapon{
        friend ostream& operator<< (ostream& os, const Weapon& rhs){
            os << "weapon: " << rhs.weapon_name << ", " << rhs.strength;
            return os;
        }
    public:
        Weapon(const string& weapon_name, int strength): weapon_name(weapon_name), strength(strength){}
        
        int get_strength() const {return strength;}
        
        void set_strength(int num){
            strength = num;
        }
        
    private:
        string weapon_name;
        int strength;
    };
    
public:
    Warrior(const string& name, const string& weapon_name, int strength): name(name), weapon(weapon_name, strength){}
    
    string get_name() const {return name;}
    
    //This method prints out results of the battle of two warrior object.
    void battle(Warrior& other) {
        cout << name << " battles " << other.name << endl;
        
        if(weapon.get_strength() == 0 && other.weapon.get_strength() == 0){
            cout << "Oh, NO! They're both dead! Yuck!" << endl;
        } else if(weapon.get_strength() == 0){
            cout << "He's dead, " << other.name << endl;
        } else if(other.weapon.get_strength() == 0){
            cout << "He's dead, " << name << endl;
        } else if(weapon.get_strength() == other.weapon.get_strength()){
            weapon.set_strength(0);
            other.weapon.set_strength(0);
            cout << "Mutual Annihilation: " << name << " and " << other.name << " die at each other's hands" << endl;
        } else if(weapon.get_strength() > other.weapon.get_strength()){
            weapon.set_strength(weapon.get_strength() - other.weapon.get_strength());
            other.weapon.set_strength(0);
            cout << name << " defeats " << other.name << endl;
        } else {
            weapon.set_strength(other.weapon.get_strength() - weapon.get_strength());
            weapon.set_strength(0);
            cout << other.name << " defeats " << name << endl;
        }
    }

private:
    string name;
    Weapon weapon;
};


void read_file(ifstream&);
size_t find_warrior_by_name(const string&, const vector<Warrior>&);
size_t find_warrior(const Warrior&, const vector<Warrior>&);
void show_status(const vector<Warrior>&);
void fill_vector(const Warrior&, vector<Warrior>&);


int main() {
    //read the file and checks if the file exists.
    ifstream my_file("warriors.txt");
    if (!my_file) {
        cerr << "failed to open warriors.txt" << endl;
        exit(1);
    }
    read_file(my_file);
    my_file.close();
    
    return 0;
}           

/* This function takes in ifstream file and reads the first tokken on each line.
 * depending on the command, calls the corresponding function.
 * return: void
 */
void read_file(ifstream& my_file){
    vector<Warrior> war_vec;
    string command;
    string name;
    string weapon_name;
    int strength;
    string war1;
    string war2;
    
    while(my_file >> command){
        if (command == "Warrior"){
            my_file >> name >> weapon_name >> strength ;
            Warrior war(name, weapon_name, strength);
            fill_vector(war, war_vec);
        } else if (command == "Status"){
            show_status(war_vec);
        } else if (command == "Battle"){
            my_file >> war1 >> war2;
            war_vec[find_warrior_by_name(war1, war_vec)].battle(war_vec[find_warrior_by_name(war2, war_vec)]);
        }
    }
}

/* This function takes in vector of Warrior objects and display
 * the status of the warriors
 * return: void
 */
void show_status(const vector<Warrior>& war_vec){
    cout << "There are: " << war_vec.size() << " warriors" << endl;
    for(size_t i = 0; i < war_vec.size(); ++i){
        cout << war_vec[i] << endl;
    }
}

/* This function takes in name, strength of each warrior and if the warrior does not
 * exists in the vector then adds it to the vector, if not
 * prints an error message.
 * return: void
 */
void fill_vector(const Warrior& war, vector<Warrior>& war_vec){
    
    if (find_warrior(war, war_vec) == war_vec.size()){
        war_vec.push_back(war);
    } else {
        cerr << "Error: the warrior " << war.get_name() << " already exists" << endl;
    }
}

/* This function takes in name of the warrior and returns the index
 * of warrior object in the vector that corresponds to that name
 * return: size_t index
 */
size_t find_warrior_by_name(const string& name, const vector<Warrior>& vec){
    for(size_t i = 0; i < vec.size(); ++i){
        if (vec[i].get_name() == name){
            return i;
        }
    }
    return vec.size();
}

/* This function takes in warrior object and checks if the vector already
 * has the warrior with the same name
 * return: size_t index
 */
size_t find_warrior(const Warrior& war, const vector<Warrior>& vec){
    for(size_t i = 0; i < vec.size(); ++i){
        if (vec[i].get_name() == war.get_name()){
            return i;
        }
    }
    return vec.size();
}
```