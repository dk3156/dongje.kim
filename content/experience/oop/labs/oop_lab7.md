---
title: "Lab7"
date: Homework
mainSectionTitle: "ptc"
---

## Answers

## course.cpp
```
//
//  course.cpp
//  rec07
//
//  Created by DJ on 3/11/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#include "course.h"
#include "student.h"
#include <iostream>
using namespace std;

namespace BrooklynPoly{
    ostream& operator<<(ostream& os, const Course& rhs) {
        os << rhs.name << ": ";
        for (size_t i = 0; i < rhs.students.size(); ++i){
            os << rhs.students[i]->getName() << " ";
        }
        return os;
    }
    
    Course::Course(const string& courseName): name(courseName) {}
    
    const string& Course::getName() const {return name;}
    
    bool Course::addStudent(Student* rhs){
        for (size_t i = 0; i < students.size(); ++i){
            if (students[i]->getName() == rhs->getName()){
                return false;
            }
        }
        students.push_back(rhs);
        return true;
    }
    
    void Course::removeStudentsFromCourse(){
        for (size_t i = 0; i < students.size(); ++i){
            students[i]->removedFromCourse(this);
        }
        students.clear();
    }
    
    //student definition
    ostream& operator<<(ostream& os, const Student& rhs) {
        os << rhs.name << ": ";
        for (size_t i = 0; i < rhs.courses.size(); ++i){
            os << rhs.courses[i]->getName() << " ";
        }
        return os;
    }
    
    Student::Student(const string& name): name(name) {}
    
    const string& Student::getName() const {return name;}
    
    bool Student::addCourse(Course* rhs){
        for (size_t i = 0; i < courses.size(); ++i){
            if (courses[i] == rhs){
                return false;
            }
        }
        courses.push_back(rhs);
        return true;
    }
}
```

## course.h
```
//
//  course.h
//  rec07
//
//  Created by DJ on 3/11/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#ifndef course_h
#define course_h

#include<iostream>
#include<string>
#include<vector>

namespace BrooklynPoly{
    class Student;
    
    class Course {
        friend std::ostream& operator<<(std::ostream& os, const Course& rhs);
    public:
        // Course methods needed by Registrar
        Course(const std::string& courseName);
        const std::string& getName() const;
        bool addStudent(Student*);
        void removeStudentsFromCourse();
        
    private:
        std::string name;
        std::vector<Student*> students;
    };
}


#endif /* course_h */
```

## registrar.cpp
```
//
//  registrar.cpp
//  rec07
//
//  Created by DJ on 3/11/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#include "registrar.h"
#include "course.h"
#include "student.h"
#include <iostream>
using namespace std;

namespace BrooklynPoly {
    ostream& operator<<(ostream& os, const Registrar& rhs){
        os << "Registrar's report: " << endl;
        os << "Courses:" << endl;
        for (size_t i = 0; i < rhs.courses.size(); ++i){
            os << *rhs.courses[i] << endl;
        }
        os << "Students:" << endl;
        for (size_t i = 0; i < rhs.students.size(); ++i){
            os << *rhs.students[i] << endl;
        }
        return os;
    }
    
    Registrar::Registrar(){}
    
    bool Registrar::addCourse(const string& name){
        size_t idx = findCourse(name);
        if (idx != courses.size()){
            return false;
        }
        courses.push_back(new Course(name));
        return true;
    }
    
    bool Registrar::addStudent(const string& name){
        size_t idx = findStudent(name);
        if (idx != students.size()){
            return false;
        }
        students.push_back(new Student(name));
        return true;
    }
    
    bool Registrar::enrollStudentInCourse(const string& studentName,
                                          const string& courseName){
        if (findStudent(studentName) == students.size() || findCourse(courseName) == courses.size()){
            return false;
        }
        Student* stu = students[findStudent(studentName)];
        Course* cour = courses[findCourse(courseName)];
        stu->addCourse(cour);
        cour->addStudent(stu);
        return true;
    }
    
    bool Registrar::cancelCourse(const string& courseName){
        size_t idx = findCourse(courseName);
        if(idx == courses.size()){
            return false;
        }
        courses[idx]->removeStudentsFromCourse();
        delete courses[idx];
        for (size_t i = idx; i < courses.size(); ++i){
            courses[i] = courses[i+1];
        }
        courses.pop_back();
        return true;
    }
    
    void Registrar::purge(){
        for (size_t i = 0; i < courses.size(); ++i){
            delete courses[i];
        }
        courses.clear();
        for (size_t i = 0; i < students.size(); ++i){
            delete students[i];
        }
        students.clear();
    }
    
    
    size_t Registrar::findStudent(const string& name) const{
        for (size_t i = 0; i < students.size(); ++i){
            if(students[i]->getName() == name){
                return i;
            }
        }
        return students.size();
    }
    
    size_t Registrar::findCourse(const string& name) const{
        for (size_t i = 0; i < courses.size(); ++i){
            if(courses[i]->getName() == name){
                return i;
            }
        }
        return courses.size();
    }
}
```

## registrar.h
```
//
//  registrar.h
//  rec07
//
//  Created by DJ on 3/11/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#ifndef registrar_h
#define registrar_h
#include<iostream>
#include<string>
#include<vector>

class Student;
class Course;
class Registrar {
    friend std::ostream& operator<<(std::ostream& os, const Registrar& rhs);
public:
    Registrar();
    bool addCourse(const std::string&);
    bool addStudent(const std::string&);
    bool enrollStudentInCourse(const std::string& studentName,
                               const std::string& courseName);
    bool cancelCourse(const std::string& courseName);
    void purge();
    
private:
    size_t findStudent(const std::string&) const;
    size_t findCourse(const std::string&) const;
    
    std::vector<Course*> courses;
    std::vector<Student*> students;
};


#endif /* registrar_h */
```

## student.cpp
```
//
//  student.cpp
//  rec07
//
//  Created by DJ on 3/11/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#include "course.h"
#include "student.h"
#include <iostream>
using namespace std;

namespace BrooklynPoly{
    ostream& operator<<(ostream& os, const Student& rhs) {
        os << rhs.name << ": ";
        for (size_t i = 0; i < rhs.courses.size(); ++i){
            os << rhs.courses[i]->getName() << " ";
        }
        return os;
    }
    
    Student::Student(const string& name): name(name) {}
    
    const string& Student::getName() const {return name;}
    
    bool Student::addCourse(Course* rhs){
        for (size_t i = 0; i < courses.size(); ++i){
            if (courses[i] == rhs){
                return false;
            }
        }
        courses.push_back(rhs);
        return true;
    }
    
    // Student method needed by Course
    void Student::removedFromCourse(Course* rhs) {
        for (size_t i = 0; i < courses.size(); ++i){
            if (courses[i] == rhs){
                for (size_t j = i; j < courses.size(); ++j){
                    courses[j] = courses[j+1];
                }
                courses.pop_back();
                return;
            }
        }
    }

}
```

## student.h
```
//
//  student.h
//  rec07
//
//  Created by DJ on 3/11/22.
//  Copyright © 2022 DJ. All rights reserved.
//

#ifndef student_h
#define student_h
#include<string>
#include<vector>
#include<iostream>

namespace BrooklynPoly{
    class Course;
    class Student {
        friend std::ostream& operator<<(std::ostream& os, const Student& rhs);
    public:
        // Student methods needed by Registrar
        Student(const std::string& name);
        const std::string& getName() const;
        bool addCourse(Course*);
        
        // Student method needed by Course
        void removedFromCourse(Course*);
        
    private:
        std::string name;
        std::vector<Course*> courses;
    };
}

#endif /* student_h */
```