# data-structure-and-algorithm
//project2
//Due: Monday (9/24) at 11:59pm

#include <iostream>
#include <vector>
#include <queue>
#include <stack>

using namespace std;

class pos {
public:
	int id;//0...63
	bool visited;
	bool in_wait;
	int from_id;
	pos() { id = -1; visited = false; in_wait = false; from_id = -1; }
	bool compare(int x, int y) {
		if (x >= 0 && x <= 7) {
			if (y >= 1 && y <= 8) return true;
		}
		return false;
	}
	int move(int k) {
		//For this move direction (1 ... 8)
		//return the new_id once a move is made
		//or any illeagal value not in 0...63 
		int x = id % 8, y = id / 8 + 1;
		switch (k) {
		case 1: x = x + 2; y = y - 1; if (compare(x, y)) return (y - 1) * 8 + x; else return -1;
		case 2: x = x + 1; y = y - 2; if (compare(x, y)) return (y - 1) * 8 + x; else return -1;
		case 3: x = x - 1; y = y - 2; if (compare(x, y)) return (y - 1) * 8 + x; else return -1;
		case 4: x = x - 2; y = y - 1; if (compare(x, y)) return (y - 1) * 8 + x; else return -1;
		case 5: x = x - 2; y = y + 1; if (compare(x, y)) return (y - 1) * 8 + x; else return -1;
		case 6: x = x - 1; y = y + 2; if (compare(x, y)) return (y - 1) * 8 + x; else return -1;
		case 7: x = x + 1; y = y + 2; if (compare(x, y)) return (y - 1) * 8 + x; else return -1;
		case 8: x = x + 2; y = y + 1; if (compare(x, y)) return (y - 1) * 8 + x; else return -1;
		}
	}
};

void print_path(vector<pos> &board, int k);
void data_input(int &start_id, int &end_id);
void BFS(vector<pos> &board, int &start_id, int &end_id);
void DFS(vector<pos> &board, int &start_id, int &end_id);

//You are not allowed to modify main or class pos.
//For class pos, you need to implement move functiion
//will explain this in class.
int main() {
	vector<pos> board(64);
	for (int i = 0; i < 64; i++) board[i].id = i;
	//queue<int> Queue;
	int start_id = -1, end_id = -1; //legal values are 0 ... 63
	data_input(start_id, end_id);
	
	BFS(board, start_id, end_id);
	for (int i = 0; i < 64; i++) {
		board[i].id = i;
		board[i].visited = false;
		board[i].in_wait = false;
		board[i].from_id = -1;
	}
	
	DFS(board, start_id, end_id);

	getchar();
	getchar();
	return 0;
}

void data_input(int &start_id, int &end_id) {
	//stard_id, end_id will be in 0 ... 63
	char x1, x2;
	int y1, y2;
	cout << "Enter starting position --  x is in a ... h and y is in 1 ... 8" << endl;
	cin >> x1 >> y1;
	start_id = (y1 - 1) * 8 + (int)x1 - 97;
	cout << "Enter ending position -- x is in a ... h and y is in 1 ... 8" << endl;
	cin >> x2 >> y2;
	end_id = (y2 - 1) * 8 + (int)x2 - 97;
	//cout << "start id" << " " << start_id << endl;
	//cout << "end id" << " " << end_id << endl;
}

void BFS(vector<pos> &board, int &start_id, int &end_id) {
	// Your code here.
   //If you find no solution, print the following
  //cout << "No solution is possible" << endl;
	if (start_id == end_id) return;
	queue<pos> q;
	q.push(board[start_id]);
	board[start_id].in_wait = true;
	while (!q.empty()) {
		pos qtemp = q.front();
		q.pop();
		qtemp.in_wait = false;
		qtemp.visited = true;
		for (int i = 1; i <= 8; i++) {
			int temp = qtemp.move(i);
			if (temp != -1 && board[temp].in_wait == false && board[temp].visited == false) {
				board[temp].from_id = qtemp.id;
				board[temp].in_wait = true;
				q.push(board[temp]);
			}
		}
	}
	if (board[end_id].from_id == -1) { cout << "No solution is possible" << endl; }
	else {
		cout << "Target is reached!" << endl;
		print_path(board, end_id);
	}
}


void DFS(vector<pos> &board, int &start_id, int &end_id) {
	// Your code here.
   //If you find no solution, print the following
  //cout << "No solution is possible" << endl;
	if (start_id == end_id) return;
	stack<pos> s;
	s.push(board[start_id]);
	board[start_id].in_wait = true;
	while (!s.empty()) {
		pos ptemp = s.top();
		s.pop();
		ptemp.in_wait = false;
		ptemp.visited = true;
		for (int i = 1; i <= 8; i++) {
			int temp = ptemp.move(i);
			if (temp != -1 && board[temp].in_wait == false && board[temp].visited == false) {
				board[temp].from_id = ptemp.id;
				board[temp].in_wait = true;
				s.push(board[temp]);
			}
		}
	}
	if(board[end_id].from_id == -1){ cout << "No solution is possible" << endl; }
	else{
		cout << "Target is reached!" << endl;
		print_path(board, end_id);
	}

}


void print_path(vector<pos> &board, int k) {
	//In both DFS and BFS, when the target postion is reached, this function will be called.
	//Your code
	int p = k;
	
	while (board[p].from_id != -1) {
		int x1 = p % 8, y1 = p / 8 + 1;
		char z1 = (char)(x1+97) ;
		cout << "(" << z1 << "," << y1 << ")" << "<--";
		p = board[p].from_id;
	}
	int x2 = board[p].id % 8, y2 = board[p].id/8 + 1;
	char z2 = (char)(x2 + 97);
	cout << "(" << z2 << "," << y2 << ")" << endl;
}

//The following is a sample screenshot.  You have to ensure that your output format is idential to it.

/*
Enter starting position --  x is in a ... h and y is in 1 ... 8
b 3
Enter ending position -- x is in a ... h and y is in 1 ... 8
f 2

Target is reached!
(f,2) <-- (e,4) <-- (d,2) <-- (b,3)

Target is reached!
(f,2) <-- (d,1) <-- (c,3) <-- (a,2) <-- (b,4) <-- (a,6) <-- (c,7) <-- (a,8) <-- (b,6) <-- (c,4) <-- (e,5) <-- (g,6) <-- (f,8) <-- (h,7) <-- (f,6) <-- (g,8) <-- (h,6) <-- (f,5) <-- (d,4) <-- (b,3)

*/
