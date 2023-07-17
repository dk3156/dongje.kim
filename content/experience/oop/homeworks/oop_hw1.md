---
title: "HW1"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

```
//
//  hw01.cpp
//
//  Created by DJ on 2/2/22.
//  Copyright © 2022 DJ. All rights reserved.
//  This file reads an encrypted input file and decrypt it

#include <iostream>
#include <fstream>
#include <vector>
using namespace std;

char convertChar (char, int);
void convertString (string&, int);

int main() {
    ifstream my_file("encrypted.txt");
    string line;
    int distance;
    vector<string> lines;
    my_file >> distance;
    
    //reads each line of the file and put it in a vector
    while (getline(my_file, line)){
        lines.push_back(line);
    }
    //convert each line to its original string by calling convertString
    for (size_t i = 1; i < lines.size(); ++i){
        convertString(lines[i], distance);
    }
    //output decrypted lines in reverse
    for (size_t i = lines.size(); i > 0; --i){
        cout << lines[i-1] << endl;
    }
    
    return 0;
}
/* This function takes in a character and a rotation
 * distance value and convert it to its original character.
 * return: char
 */
char convertChar (char letter, int distance){
    char newChar;
    if ('a' <= letter && letter <= 'z'){
        newChar = letter - distance;
        if (newChar < 'a'){
            newChar = newChar + 26;
        }
    } else {
        newChar = letter;
    }
    return newChar;
}
/* This function takes in a string and a rotation
 * distance value and convert it to its original string.
 * return: none
 */
void convertString (string& line, int distance){
    for(int i = 0; i < line.length(); ++i){
        line[i] = convertChar(line[i], distance);
    }
}

```