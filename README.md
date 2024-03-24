# Mylinklist-github-
Mylinklist(改寫自github)
#include <iostream>
using std::cout;
using std::endl;

class Mylinklist;// 為了將class Mylinklist設成class MylinklistNode的friend,
                     // 需要先宣告

class Mylinklistnode{
    
private://不是大家都可以用(不是公開分享)

    int data;
    Mylinklistnode*next;

public://大家都可以用(公開分享)
    
   Mylinklistnode():data(0),next(0){};
   Mylinklistnode(int a):data(a),next(0){}; 

   friend class Mylinklist;  //friend 可以和private搭配使用
    
    
};


class Mylinklist{
private:
    // int size;                // size是用來記錄Linked list的長度, 非必要
    Mylinklistnode *first;            // list的第一個node
public:
    Mylinklist():first(0){};
    void showlinklist();           // 印出list的所有資料
    void insert_head(int x);     // 在list的開頭新增node
    void insert_tail(int x);      // 在list的尾巴新增node
    void Delete(int x);         // 刪除list中的 int x
    void deleteall();               // 把整串list刪除
    void reverselinklist();             // 將list反轉: 7->3->14 => 14->3->7
};


void Mylinklist::showlinklist(){

    if (first == 0) {                      // 如果first node指向NULL, 表示list沒有資料
        cout << "List is empty.\n";
        return;
    }

    Mylinklistnode *current = first;             // 用pointer *current在list中移動
    while (current != 0) {                 // Traversal
        cout << current->data << " ";
        current = current->next;
    }
    cout << endl;
}


void Mylinklist::insert_head(int x){

    Mylinklistnode *newNode = new Mylinklistnode(x);   // 配置新的記憶體
    newNode->next = first;                 // 先把first接在newNode後面
    first = newNode;                       // 再把first指向newNode所指向的記憶體位置
}


void Mylinklist::insert_tail(int x){

    Mylinklistnode *newNode = new Mylinklistnode(x);   // 配置新的記憶體

    if (first == 0) {                      // 若list沒有node, 令newNode為first
        first = newNode;
        return;
    }

    Mylinklistnode *current = first;
    while (current->next != 0) {           // Traversal
        current = current->next;
    }
    current->next = newNode;               // 將newNode接在list的尾巴
}


void Mylinklist::Delete(int x){

    Mylinklistnode *current = first,      
             *previous = 0;
    while (current != 0 && current->data != x) {  // Traversal
        previous = current;                       // 如果current指向NULL
        current = current->next;                  // 或是current->data == x
    }                                             // 即結束while loop

    if (current == 0) {                 // list沒有要刪的node, 或是list為empty
        std::cout << "There is no " << x << " in list.\n";
        // return;
    }
    else if (current == first) {        // 要刪除的node剛好在list的開頭
    	first = current->next;          // 把first移到下一個node
        delete current;                 // 如果list只有一個node, 那麼first就會指向NULL
        current = 0;                    // 當指標被delete後, 將其指向NULL, 可以避免不必要bug
        // return;                     
    }
    else {                              // 其餘情況, list中有欲刪除的node, 
        previous->next = current->next; // 而且node不為first, 此時previous不為NULL
        delete current;
        current = 0;
        // return;
    }
}


void Mylinklist::deleteall(){

    while (first != 0) {            // Traversal
        Mylinklistnode *current = first;
        first = first->next;
        delete current;
        current = 0;
    }
}

void Mylinklist::reverselinklist(){

    if (first == 0 || first->next == 0) {
        // list is empty or list has only one node
        return;
    }

    Mylinklistnode *previous = 0,
             *current = first,
             *preceding = first->next;

    while (preceding != 0) {
        current->next = previous;      // 把current->next轉向
        previous = current;            // previous往後挪
        current = preceding;           // current往後挪
        preceding = preceding->next;   // preceding往後挪
    }                                  // preceding更新成NULL即跳出while loop

    current->next = previous;          // 此時current位於最後一個node, 將current->next轉向
    first = current;                   // 更新first為current
}

int main() {

    Mylinklist list;     // 建立LinkedList的object
    list.showlinklist();    // 目前list是空的
    list.Delete(4);      // list是空的, 沒有4
    list.insert_tail(5);   // list: 5
    list.insert_tail(3);   // list: 5 3
    list.insert_head(9);  // list: 9 5 3
    list.showlinklist();    // 印出:  9 5 3
    list.insert_tail(4);   // list: 9 5 3 4
    list.Delete(9);      // list: 5 3 4
    list.showlinklist();    // 印出:  5 3 4
    list.insert_head(8);  // list: 8 5 3 4
    list.showlinklist();    // 印出:  8 5 3 4
    list.reverselinklist();      // list: 4 3 5 8
    list.showlinklist();    // 印出:  4 3 5 8
    list.deleteall();        // 清空list
    list.showlinklist();    // 印出: List is empty.

    return 0;
}
























