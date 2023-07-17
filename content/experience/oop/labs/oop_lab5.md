---
title: "Lab5"
date: Homework
mainSectionTitle: "ptc"
---

## Answers
```
/*
  rec05-start.cpp
 */

#include <iostream>
#include <string>
#include <vector>
using namespace std;


class Section{
    
    class Student{
        friend ostream& operator<<(ostream& os, const Student& rhs){
            os << "Name: " << rhs.name << ", Grades: ";
            for (size_t i = 0; i < rhs.grades.size(); ++i){
                os << rhs.grades[i] << " ";
            }
            
            return os;
        }
        
    public:
        Student(const string& name): name(name), grades(14,-1){}
        
        const string& get_name() const {
            return name;
        }
        
        void update_score(int score, int index){
            grades[index-1] = score;
        }
        
    private:
        string name;
        vector<int> grades;
    };
    
    class TimeSlot {
        friend ostream& operator<<(ostream& os, const TimeSlot& rhs){
            os << "[Day: " << rhs.day << ", Start time: " << rhs.hour << "]";
            
            return os;
        }
        
    public:
        TimeSlot(const string& day, int hour): day(day), hour(convert_time(hour)){}
    
        
    private:
        string day;
        string hour;
        string convert_time(int hour){
            if (hour <= 12){
                return to_string(hour) + "am";
            } else {
                int new_hour = hour - 12;
                return to_string(new_hour) + "pm";
            }
        }
    };
    
    friend ostream& operator<<(ostream& os, const Section& rhs){
        os << "Section: " << rhs.name << ", Time slot: " << rhs.timeslot << ", " << "Students: " << endl;
        for (size_t i = 0; i < rhs.student_list.size(); ++i){
            os << *rhs.student_list[i] << endl;
        }
        
        return os;
    }
                 
public:
    Section(const string& name, const string& day, int hour): name(name), timeslot(day, hour){}
    
    void addStudent(const string& name){
        Student* student = new Student(name);
        student_list.push_back(student);
    }
    
    void update_student_record(const string& name, int score, int index){
        for (size_t i = 0; i < student_list.size(); ++i){
            if (student_list[i]->get_name() == name){
                student_list[i]->update_score(score, index);
            }
        }
    }
    
private:
    string name;
    TimeSlot timeslot;
    vector<Student*> student_list;
};

class LabWorker{
    friend ostream& operator<< (ostream& os, const LabWorker& rhs){
        if(rhs.section == nullptr){
            os << rhs.name << " does not have a section" << endl;
        } else {
            os << rhs.name << " has " << *rhs.section;
        }
        
        return os;
}
    
public:
    LabWorker(const string& name): name(name), section(nullptr){}
    
    void addSection(Section& section){
        this->section = &section;
        
    }
    
    void addGrade(const string& name, int score, int index){
        section->update_student_record(name, score, index);
    }
    
private:
    string name;
    Section* section;
};

// Test code
void doNothing(Section sec) { cout << sec << endl; }

int main() {

    cout << "Test 1: Defining a section\n";
    Section secA2("A2", "Tuesday", 16);
    cout << secA2 << endl;

    cout << "\nTest 2: Adding students to a section\n";
    secA2.addStudent("John");
    secA2.addStudent("George");
    secA2.addStudent("Paul");
    secA2.addStudent("Ringo");
    cout << secA2 << endl;

    cout << "\nTest 3: Defining a lab worker.\n";
    LabWorker moe( "Moe" );
    cout << moe << endl;

    cout << "\nTest 4: Adding a section to a lab worker.\n";
    moe.addSection( secA2 );
    cout << moe << endl;

    cout << "\nTest 5: Adding a second section and lab worker.\n";
    LabWorker jane( "Jane" );
    Section secB3( "B3", "Thursday", 11 );
    secB3.addStudent("Thorin");
    secB3.addStudent("Dwalin");
    secB3.addStudent("Balin");
    secB3.addStudent("Kili");
    secB3.addStudent("Fili");
    secB3.addStudent("Dori");
    secB3.addStudent("Nori");
    secB3.addStudent("Ori");
    secB3.addStudent("Oin");
    secB3.addStudent("Gloin");
    secB3.addStudent("Bifur");
    secB3.addStudent("Bofur");
    secB3.addStudent("Bombur");
    jane.addSection( secB3 );
    cout << jane << endl;

    cout << "\nTest 6: Adding some grades for week one\n";
    moe.addGrade("John", 17, 1);
    moe.addGrade("Paul", 19, 1);
    moe.addGrade("George", 16, 1);
    moe.addGrade("Ringo", 7, 1);
    cout << moe << endl;

    cout << "\nTest 7: Adding some grades for week three (skipping week 2)\n";
    moe.addGrade("John", 15, 3);
    moe.addGrade("Paul", 20, 3);
    moe.addGrade("Ringo", 0, 3);
    moe.addGrade("George", 16, 3);
    cout << moe << endl;

    cout << "\nTest 8: We're done (almost)! \nWhat should happen to all "
         << "those students (or rather their records?)\n";

    cout << "\nTest 9: Oh, IF you have covered copy constructors in lecture, "
         << "then make sure the following call works:\n";
    doNothing(secA2);
    cout << "Back from doNothing\n\n";

} // main
```