---
title: "Lab9"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

```
#include <iostream>
#include <vector>
using namespace std;

class PrintedMaterial {
public:
    PrintedMaterial(unsigned num): numberOfPages(num){}
    virtual void displayNumPages() const = 0;
private:
    unsigned numberOfPages;
};

void PrintedMaterial::displayNumPages() const {
    cout << numberOfPages << endl;
}

class Magazine : public PrintedMaterial {
public:
    Magazine(unsigned num): PrintedMaterial(num){}
    void displayNumPages() const {
        cout << "This is magazine's printing method: " << endl;
        PrintedMaterial::displayNumPages();
    }
private:
};

class Book : public PrintedMaterial {
public:
    Book(unsigned num): PrintedMaterial(num){}
    void displayNumPages() const {
        cout <<"This is book's printing method: " << endl;
        PrintedMaterial::displayNumPages();
    }
private:
};

class TextBook : public Book {
public:
    TextBook(unsigned numPages, unsigned numIndxPgs )
    : Book(numPages),numOfIndexPages(numIndxPgs){}
    
    void displayNumPages() const {
        cout << "Pages: ";
        PrintedMaterial::displayNumPages();
        cout << "Index pages: ";
        cout << numOfIndexPages << endl;
    }
private:
    unsigned numOfIndexPages;
};

class Novel : public Book {
public:
    Novel(unsigned num): Book(num){}
private:
};

void displayNumberOfPages(const PrintedMaterial& pm){
    pm.displayNumPages();
}

// tester/modeler code
int main() {
    TextBook text(10, 2000);
    Novel novel(12);
    Magazine mag(14);
    
    text.displayNumPages();
    novel.displayNumPages();
    mag.displayNumPages();
    
    //PrintedMaterial pm(240);
    //cout << "printing pm: " << endl;
    //pm.displayNumPages();
    
    //cout << "assigning text object to pm:" << endl;
    //pm = text; //slicing
    //pm.displayNumPages();
    
    cout << "assigning text object to pm pointer:" << endl;
    PrintedMaterial* pmPtr;
    pmPtr = &text;
    pmPtr->displayNumPages();
    
    cout << "printing using PM reference type: " << endl;
    displayNumberOfPages(text);
    displayNumberOfPages(novel);
    displayNumberOfPages(mag);
    
    vector<PrintedMaterial*> pm_vec;
    pm_vec.push_back(&text);
    pm_vec.push_back(&novel);
    pm_vec.push_back(&mag);
    for (size_t i = 0; i < pm_vec.size(); ++i) {
        pm_vec[i]->displayNumPages();
        cout << endl;
    }
}
```