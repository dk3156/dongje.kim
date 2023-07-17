---
title: "Lab11"
date: Homework
mainSectionTitle: "ptc"
---

## Answers
[Download linked_list_basics.pdf](https://dk3156.github.io/experience/oop/labs/linked_list_basics.pdf)
```
/*
  rec10_start.cpp
  Starting code 22S
 */

#include <iostream>
#include <vector>
using namespace std;

class Instrument {
public:
  virtual void makeSound() const = 0;
  virtual void normalPlay() const = 0;
};

void Instrument::makeSound() const{
  cout << "To make a sound... ";
}

class Percussion : public Instrument{
public:
  void makeSound() const {
    Instrument::makeSound();
    cout << "hit me!"<< endl;}
};

class Drum : public Percussion {
public:
  void normalPlay() const {cout << "Boom" << endl;}
};

class Cymbal : public Percussion {
public:
  void normalPlay() const {cout << "Crash" << endl;}
};


class Brass : public Instrument{
public:
  Brass(unsigned piece): mouthpiece(piece){}
  void makeSound() const {
    Instrument::makeSound();
    cout << "blow on a mouthpiece of size " << mouthpiece << endl;}
private:
  unsigned mouthpiece;
};

class Trombone : public Brass{
public:
  Trombone(unsigned piece): Brass(piece){}
  void normalPlay() const {cout << "Blat" << endl;}
};

class Trumpet : public Brass{
public:
  Trumpet(unsigned piece): Brass(piece){}
  void normalPlay() const {cout << "Toot" << endl;}
};


class  String : public Instrument{
public:
  String(unsigned pitch): pitch(pitch){}
  void makeSound() const {
    Instrument::makeSound();
    cout << "bow a string with pitch " << pitch << endl;}
private:
  unsigned pitch;
};

class Violin : public String{
public:
  Violin(unsigned pitch): String(pitch){}
  void normalPlay() const {cout << "Screech" << endl;}
};
class Cello : public String{
public:
  Cello(unsigned pitch): String(pitch){}
  void normalPlay() const {cout << "Squawk" << endl;}
};



class MILL {
public:
  void receiveInstr(Instrument& instr){
      instr.makeSound();
     //check the vector for the first empty spot
     //i.e. nullptr, assign to it, return, otherwise after the loop push_back
     for (size_t i=0; i< ins_vec.size(); ++i){
         if(ins_vec[i] == nullptr){
             ins_vec[i] = &instr;
             return;
         }
     }
    ins_vec.push_back(&instr);
  }

  void dailyTestPlay() const {
    for (size_t i=0; i<ins_vec.size(); ++i){
      if(ins_vec[i] != nullptr){
        ins_vec[i]->makeSound();
      }
    }
  }

  Instrument* loanOut(){
    for (size_t i=0; i<ins_vec.size(); ++i){
      if(ins_vec[i] != nullptr){
          Instrument* new_instr = ins_vec[i];
          ins_vec[i] = nullptr;
        return new_instr;
      }
    }
    return nullptr;
};

private:
  vector<Instrument*> ins_vec;
};
//
// Musician class is provided to the students. Requires the Instrument class.
//

class Musician {
public:
    Musician() : instr(nullptr) {}

    // acceptInstr takes in an instrument for the Musician to later play.
    //  "loses" any instrument that it used to have.
    void acceptInstr(Instrument* instPtr) { instr = instPtr; }

    // giveBackInstr: gives the instrument that the Musicial was holding "back"
    Instrument* giveBackInstr() {
        Instrument* result(instr);
        instr = nullptr;
        return result;
    }

    // testPlay: testing out my instrument
    void testPlay() const {
        if (instr) instr->makeSound();
        else cerr << "have no instr\n";
    }

    void normalPlay() const {
      if (instr) instr->normalPlay();
    }

private:
    Instrument* instr;
};

class Orch {
public:
  void addPlayer (Musician& musician){
    musician_vec.push_back(&musician);
  }

  void play() const {
    for (size_t i=0; i<musician_vec.size(); ++i){
      musician_vec[i]->normalPlay();
    }
  }
private:
  vector<Musician*> musician_vec;
};

int main() {

    //
    // PART ONE
    //
    cout << "P A R T  O N E\n";

    cout << "Define some instruments --------------------------------------\n";
    Drum drum;
    Cello cello(673);
    Cymbal cymbal;
    Trombone tbone(4);
    Trumpet trpt(12) ;
    Violin violin(567) ;

    // use the debugger to look at the mill
    cout << "Define the MILL ----------------------------------------------\n";
    MILL mill;

    cout << "Put the instruments into the MILL ----------------------------\n";
    mill.receiveInstr(trpt);
    mill.receiveInstr(violin);
    mill.receiveInstr(tbone);
    mill.receiveInstr(drum);
    mill.receiveInstr(cello);
    mill.receiveInstr(cymbal);

    cout << "Daily test ---------------------------------------------------\n"
	 << "dailyTestPlay()" << endl;
    mill.dailyTestPlay();
    cout << endl;

    cout << "Define some Musicians-----------------------------------------\n";
    Musician harpo;
    Musician groucho;

    cout << "TESTING: groucho.acceptInstr(mill.loanOut());-----------------\n";
    groucho.testPlay();
    cout << endl;
    groucho.acceptInstr(mill.loanOut());
    cout << endl;
    groucho.testPlay();
    cout << endl;
    cout << "dailyTestPlay()" << endl;
    mill.dailyTestPlay();

    cout << endl << endl;

    groucho.testPlay();
    cout << endl;
    mill.receiveInstr(*groucho.giveBackInstr());
    harpo.acceptInstr(mill.loanOut());
    groucho.acceptInstr(mill.loanOut());
    cout << endl;
    groucho.testPlay();
    cout << endl;
    harpo.testPlay();
    cout << endl;
    cout << "dailyTestPlay()" << endl;
    mill.dailyTestPlay();

    cout << "TESTING: mill.receiveInstr(*groucho.giveBackInstr()); --------\n";

    // fifth
    mill.receiveInstr(*groucho.giveBackInstr());
    cout << "TESTING: mill.receiveInstr(*harpo.giveBackInstr()); ----------\n";
    mill.receiveInstr(*harpo.giveBackInstr());


    cout << endl;
    cout << "dailyTestPlay()" << endl;
    mill.dailyTestPlay();
    cout << endl;


    //
    // PART TWO
    //
    cout << "P A R T  T W O\n";


    Musician bob;
    Musician sue;
    Musician mary;
    Musician ralph;
    Musician jody;
    Musician morgan;

    Orch orch;

    // THE SCENARIO

    //Bob joins the orchestra without an instrument.
    orch.addPlayer(bob);

    //The orchestra performs
    cout << "orch performs\n";
    orch.play();

    //Sue gets an instrument from the MIL2 and joins the orchestra.
    sue.acceptInstr(mill.loanOut());
    orch.addPlayer(sue);

    //Ralph gets an instrument from the MIL2.
    ralph.acceptInstr(mill.loanOut());

    //Mary gets an instrument from the MIL2 and joins the orchestra.
    mary.acceptInstr(mill.loanOut());
    orch.addPlayer(mary);

    //Ralph returns his instrument to the MIL2.
    mill.receiveInstr(*ralph.giveBackInstr());

    //Jody gets an instrument from the MIL2 and joins the orchestra.
    jody.acceptInstr(mill.loanOut());
    orch.addPlayer(jody);

    // morgan gets an instrument from the MIL2
    morgan.acceptInstr(mill.loanOut());

    //The orchestra performs.
    cout << "orch performs\n";
    orch.play();

    //Ralph joins the orchestra.
    orch.addPlayer(ralph);

    //The orchestra performs.
    cout << "orch performs\n";
    orch.play();

    // bob gets an instrument from the MIL2
    bob.acceptInstr(mill.loanOut());

    // ralph gets an instrument from the MIL2
    ralph.acceptInstr(mill.loanOut());

    //The orchestra performs.
    cout << "orch performs\n";
    orch.play();

    //Morgan joins the orchestra.
    orch.addPlayer(morgan);

    //The orchestra performs.
    cout << "orch performs\n";
    orch.play();
}
```