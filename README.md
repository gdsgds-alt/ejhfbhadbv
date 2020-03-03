//#include <conio.h>
#include "windows.h"
#include <vector>
#include <iostream>
using namespace std;

class Board {
protected:
    void printFourString(char a1, char a2, char a3) {
        for (int i = 0; i < 4; i++) {
            switch (a1)
            {
            case 'x':
                cout << X[i];
                break;
            case 'o':
                cout << O[i];
                break;
            default:
                cout << "        ";
                break;
            }
            if (i != 0)
                cout << "||";
            else
                cout << "  ";

            switch (a2)
            {
            case 'x':
                cout << X[i];
                break;
            case 'o':
                cout << O[i];
                break;
            default:
                cout << "        ";
                break;
            }
            if (i != 0)
                cout << "||";
            else
                cout << "  ";

            switch (a3)
            {
            case 'x':
                cout << X[i];
                break;
            case 'o':
                cout << O[i];
                break;
            default:
                cout << "        ";
                break;
            }

            cout << endl;
        }
    }
public:
    char** brd = new char* [3];
    vector <string> X;
    vector <string> O;
    Board() {
        for (int i = 0; i < 3; i++) {
            brd[i] = new char[3];
            for (int j = 0; j < 3; j++) {
                brd[i][j] = ' ';
            }
        }
        X.push_back("___  ___");
        X.push_back("\\  \\/  /");
        X.push_back(" >    < ");
        X.push_back("/__/\\_ \\");
        O.push_back("  ____  ");
        O.push_back(" /  _ \\ ");
        O.push_back("(  <_> )");
        O.push_back(" \\____/ ");
    }

    void easyPrintBoard() {
        for (int i = 0; i < 5; i++) {
            if (i == 0 || i == 2 || i == 4) {
                for (int j = 0; j < 2; j++) {
                    cout << brd[i / 2][j] << '|';
                }
                cout << brd[i / 2][2] << endl;
            }
            else {
                cout << "------" << endl;
            }

        }
    }
    void strongPrintBoard() {
        for (int i = 0; i < 3; i++) {
            printFourString(brd[i][0], brd[i][1], brd[i][2]);
        }
    }

    int checkWinner() {
        for (int i = 0; i < 3; i += 3) {
            if (brd[i][0] == brd[i][1] && brd[i][1] == brd[i][2] && brd[i][0] != ' ') {
                switch (brd[i][0])
                {
                case 'x':
                    return 1;
                default:
                    return 0;
                }
            }
            if (brd[0][i] == brd[1][i] && brd[0][i] == brd[2][i] && brd[0][i] != ' ') {
                switch (brd[0][i])
                {
                case 'x':
                    return 1;
                default:
                    return 0;
                }
            }
        }
        if (brd[0][0] == brd[1][1] && brd[0][0] == brd[2][2] && brd[0][0] != ' ') {
            switch (brd[0][0])
            {
            case 'x':
                return 1;
            default:
                return 0;
            }
        }
        if (brd[0][2] == brd[1][1] && brd[0][2] == brd[2][0] && brd[0][2] != 'N') {
            switch (brd[0][2])
            {
            case 'x':
                return 1;
            default:
                return 0;
            }
        }
        return -1;
    }
};

class Bot {
protected:
    char** brd = new char* [3];
    /*void updateBoard(Board b) {
        for (int i = 0; i < 3; i++) {
            brd[i] = new char[3];
            for (int j = 0; j < 3; j++) {
                brd[i][j] = b.brd[i][j];
            }
        }
    }*/
    int bestMove(Board b) {
        /*if (!((brd[0][0] == brd[0][2]) && (brd[0][0] == 'o'))) {
            cout << brd[0][0] << "      " << brd[0][2] << "      " << endl;
        }*/
        for (int i = 0; i < 3; i++) {
            if ((b.brd[i][0] == b.brd[i][2]) && (b.brd[i][0] == 'o')) {
                return i * 10 + 1;
            }
            if (b.brd[i][0] == b.brd[i][1] && b.brd[i][0] != ' ' && b.brd[i][0] != 'x') {
                return i * 10 + 2;
            }
            if (b.brd[i][2] == b.brd[i][1] && b.brd[i][2] != ' ' && b.brd[i][2] != 'x') {
                return i * 10;
            }
            if (b.brd[0][i] == b.brd[2][i] && b.brd[0][i] != ' ' && b.brd[0][i] != 'x') {
                return 10 + i;
            }
            if (b.brd[0][i] == b.brd[1][i] && b.brd[0][i] != ' ' && b.brd[0][i] != 'x') {
                return 20 + i;
            }
            if (b.brd[2][i] == b.brd[1][i] && b.brd[2][i] != ' ' && b.brd[2][i] != 'x') {
                return i;
            }
        }
        if (b.brd[0][0] == b.brd[2][2] && b.brd[0][0] == 'o') {
            return 11;
        }
        if (b.brd[0][2] == b.brd[2][0] && b.brd[0][0] == 'o') {
            return 11;
        }
        if (b.brd[0][0] == ' ') {
            cout << 1 << endl;
            return 0;
        }
        if (b.brd[0][2] == ' ') {
            return 2;
        }
        if (b.brd[2][0] == ' ') {
            return 20;
        }
        if (b.brd[2][2] == ' ') {
            return 22;
        }
        if (b.brd[1][1] == ' ') {
            return 11;
        }
        return -1;
    }
public:
    Bot() {
        for (int i = 0; i < 3; i++) {
            brd[i] = new char[3];
            for (int j = 0; j < 3; j++) {
                brd[i][j] = ' ';
            }
        }
    }
    
    void bestSolution(Board b) {
        cout << bestMove(b) << endl;
        if (bestMove(b) != -1)
            if (b.brd[bestMove(b) / 10][bestMove(b) % 10] == ' ')
                b.brd[bestMove(b) / 10][bestMove(b) % 10] = 'x';
    }
};
int main()
{
    //int last = GetKeyState(VK_SPACE);
    //for (int i = 0; i < 100000; i++) {
    //   /* if (!last == GetKeyState(VK_SPACE)) {
    //        cout << GetKeyState(VK_SPACE);
    //    }*/
    //    cout << GetKeyState(VK_SPACE)<<endl;
    //    last = GetKeyState(VK_SPACE);
    //    Sleep(1);
    //}
    Board b;
    Bot bt;
    cout << GetKeyState(32);
    bool p = 0;
    while (GetKeyState(32)<1) {
        b.strongPrintBoard();
        cout<<
        while (p == 0) {
            if (GetKeyState(36) > 1 || GetKeyState(36) < 1) {
                b.brd[0][0] = 'o';
                p = 1;
            }
            else if (GetKeyState(38) > 1 || GetKeyState(38) < 1) {
                b.brd[0][1] = 'o';
                p = 1;
            }
            else if (GetKeyState(33) > 1 || GetKeyState(33) < 1) {
                b.brd[0][2] = 'o';
                p = 1;
            }//
            else if (GetKeyState(37) > 1 || GetKeyState(37) < 1) {
                b.brd[1][0] = 'o';
                p = 1;
            }
            else if (GetKeyState(12) > 1 || GetKeyState(12) < 1) {
                b.brd[1][1] = 'o';
                p = 1;
            }
            else if (GetKeyState(39) > 1 || GetKeyState(39) < 1) {
                b.brd[1][2] = 'o';
                p = 1;
            }//
            else if (GetKeyState(35) > 1 || GetKeyState(35) < 1) {
                b.brd[2][0] = 'o';
                p = 1;
            }
            else if (GetKeyState(40) > 1 || GetKeyState(40) < 1) {
                b.brd[2][1] = 'o';
                p = 1;
            }
            else if (GetKeyState(34) > 1 || GetKeyState(34) < 1) {
                b.brd[2][2] = 'o';
                p = 1;
            }
        }
        p = 0;
        bt.bestSolution(b); 
        if (b.checkWinner() != -1) {
            if (b.checkWinner() == 1) {
                cout << "I win";
            }
            else {
                cout << "You win";
            }
        }
        Sleep(2);
        system("cls");
    }
    return 0;
}
