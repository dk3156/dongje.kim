---
title: "Lab2"
date: Homework
mainSectionTitle: "ptc"
---

## Answers
```
//
//  main.cpp
//  rec02
//
//  Created by DJ on 2/4/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#include <iostream>
#include <fstream>
#include <vector>

using namespace std;

struct Formula{
    vector<string> names;
    int carb_num;
    int hydro_num;
};

void openStream(ifstream&);
void readFile(ifstream&, vector<Formula>&);
void fillVec(vector<Formula>&, string, int, int);
void sortVector(vector<Formula>&);
void displayVector(vector<Formula>&);
void printSingleFormula(Formula&);
size_t find(vector<Formula>&, int, int);


int main() {
    ifstream myFile;
    vector<Formula> carb_vec;
    
    openStream(myFile);
    readFile(myFile, carb_vec);
    sortVector(carb_vec);
    displayVector(carb_vec);
    
    return 0;
}

void printSingleFormula(const Formula& formula){
    cout << "C" << formula.carb_num << "H" << formula.hydro_num << " ";
    for (size_t i = 0; i < formula.names.size(); ++i){
        cout << formula.names[i] << " ";
    }
    cout << endl;
}

void openStream(ifstream& file){
    do{
        file.clear();
        string input;
        cout << "Type the name of your file: ";
        cin >> input;
        file.open(input);
    } while(!file.is_open());
}


void readFile(ifstream& myfile, vector<Formula>& carb_vec){
    vector<string> names;
    string name;
    char carb;
    char hydro;
    int carb_num;
    int hydro_num;
    
    while (myfile >> name >> carb >> carb_num >> hydro >> hydro_num){
        fillVec(carb_vec, name, carb_num, hydro_num);
    }
    myfile.close();
}


void fillVec(const vector<Formula>& carb_vec, string name, int carb_num, int hydro_num){
    Formula hydrocarbon;
    size_t index = find(carb_vec, carb_num, hydro_num);
    if (index == carb_vec.size()){
        hydrocarbon.names.push_back(name);
        hydrocarbon.carb_num = carb_num;
        hydrocarbon.hydro_num = hydro_num;
        carb_vec.push_back(hydrocarbon);
    } else {
        carb_vec[index].names.push_back(name);
    }
}

size_t find(vector<Formula>& carb_vec, int carb_num, int hydro_num){
    for (size_t i = 0; i < carb_vec.size(); ++i){
        if(carb_vec[i].carb_num == carb_num && carb_vec[i].hydro_num == hydro_num){
            return i;
        }
    }
    return carb_vec.size();
}


void sortVector(vector<Formula>& carb_vec){
    for (int i = 0; i < carb_vec.size(); ++i){
        for (int j = 0; j < carb_vec.size() - 1; ++j){
            if (carb_vec[j].carb_num > carb_vec[j+1].carb_num ||
                (carb_vec[j].carb_num == carb_vec[j+1].carb_num &&
                 carb_vec[j].hydro_num > carb_vec[j+1].hydro_num)){
                Formula temp;
                temp.names = carb_vec[j].names;
                temp.carb_num = carb_vec[j].carb_num;
                temp.hydro_num = carb_vec[j].hydro_num;
                carb_vec[j] = carb_vec[j+1];
                carb_vec[j+1] = temp;
            }
        }
    }
}


void displayVector(const vector<Formula>& carb_vec){
    for (size_t i = 0; i < carb_vec.size(); ++i){
        printSingleFormula(carb_vec[i]);
    }
}
```