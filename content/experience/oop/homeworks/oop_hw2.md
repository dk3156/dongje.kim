---
title: "HW2"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

[Download warriors.txt](https://dk3156.github.io/experience/oop/homeworks/warriors.txt)
```
//
//  hw02.cpp
//
//  Created by DJ on 2/10/22.
//  Copyright © 2022 DJ. All rights reserved.
//  This file reads warriors.txt and prints information about the warriors.
//

#include <iostream>
#include <vector>
#include <fstream>
using namespace std;

struct Warrior{
    string name;
    int strength;
};

void read_file(ifstream&);
void create_warrior(string, int, vector<Warrior>&);
size_t find_warrior_by_name(string, const vector<Warrior>&);
size_t find_warrior(const Warrior&, const vector<Warrior>&);
void battle(Warrior&, Warrior&);
void show_status(vector<Warrior>&);


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
    int strength;
    string war1;
    string war2;
    
    while(my_file >> command){
        if (command == "Warrior"){
            my_file >> name >> strength;
            create_warrior(name, strength, war_vec);
        } else if (command == "Status"){
            show_status(war_vec);
        } else if (command == "Battle"){
            my_file >> war1 >> war2;
            battle(war_vec[find_warrior_by_name(war1, war_vec)], war_vec[find_warrior_by_name(war2, war_vec)]);
        }
    }
}

/* This function takes in vector of Warrior objects and display
 * the status of the warriors
 * return: void
 */
void show_status(vector<Warrior>& war_vec){
    cout << "There are: " << war_vec.size() << " warriors" << endl;
    
    for(size_t i = 0; i < war_vec.size(); ++i){
        cout << "Warrior: " << war_vec[i].name << ", strength: " << war_vec[i].strength << endl;
    }
}

/* This function takes in name, strength of each warrior and if the warrior does not
 * exists in the vector then adds it to the vector, if not
 * prints an error message.
 * return: void
 */
void create_warrior(string name, int strength, vector<Warrior>& vec){
    Warrior war;
    war.name = name;
    war.strength = strength;
    
    if (find_warrior(war, vec) == vec.size()){
        vec.push_back(war);
    } else {
        cerr << "Error: the warrior " << name << " already exists" << endl;
    }
}

/* This function takes in name of the warrior and returns the index
 * of warrior object in the vector that corresponds to that name
 * return: size_t index
 */
size_t find_warrior_by_name(string name, const vector<Warrior>& vec){
    for(size_t i = 0; i < vec.size(); ++i){
        if (vec[i].name == name){
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
        if (vec[i].name == war.name){
            return i;
        }
    }
    return vec.size();
}

/* This function takes in two warrior objects that battles
 * and prints out the corresponding result based on the difference of their strength
 * return: void
 */
void battle(Warrior& war1, Warrior& war2){
    cout << war1.name << " battles " << war2.name << endl;
    
    if(war1.strength == 0 && war2.strength == 0){
        cout << "Oh, NO! They're both dead! Yuck!" << endl;
    } else if (war1.strength == 0){
        cout << "He's dead, " << war2.name << endl;
    } else if (war2.strength == 0){
        cout << "He's dead, " << war1.name << endl;
    } else if (war1.strength == war2.strength){
        war1.strength = 0;
        war2.strength = 0;
        cout << "Mutual Annihilation: " << war1.name << " and " << war2.name << " die at each other's hands" << endl;
    } else if (war1.strength > war2.strength){
        war1.strength -= war2.strength;
        war2.strength = 0;
        cout << war1.name << " defeats " << war2.name << endl;
    } else {
        war2.strength -= war1.strength;
        war1.strength = 0;
        cout << war2.name << " defeats " << war2.name << endl;
    }
}
```