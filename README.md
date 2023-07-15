#include <bits/stdc++.h>
using namespace std;
map<pair<int, int>, int>ma;
class sudoku {
	int a[9][9];
public:
	sudoku(){
		for (int i = 0; i < 9; i++)
			for (int j = 0; j < 9; j++)
				cin >> a[i][j];
	}
	void set(int n, int i, int j){
		a[i][j] = n;
	}
	int get(int i, int j){
		return a[i][j];
	}
	int how_number(){
		int c = 0;
		for (int i = 0; i < 9; i++)
			for (int j = 0; j < 9; j++)
				if (a[i][j])
					c++;
		return c;
	}
	int how_number_in_row(int j){
		int c = 0;
		for (int i = 0; i < 9; i++)
			if (a[i][j])
				c++;
		return c;
	}
	int how_number_in_colon(int i){
		int c = 0;
		for (int j = 0; j < 9; j++)
			if (a[i][j])
				c++;
		return c;
	}
	int how_number_in_colon(int i, int j){
		int ii = i / 3, jj = j / 3, c = 0;
		for (int k = ii * 3; k < ii * 3 + 3; k++)
			for (int l = jj * 3; l < jj * 3 + 3; l++)
				if (a[k][l])
					c++;
		return c;
	}
	bool alrakm_mogood_wla_la(int n, int i, int j){
		for (int k = 0; k < 9; k++)
			if (a[k][j] == n)
				return 1;
		for (int k = 0; k < 9; k++)
			if (a[i][k] == n)
				return 1;
		int ii = i / 3;
		int jj = j / 3;
		for (int k = ii * 3; k < ii * 3 + 3; k++){
			for (int l = jj * 3; l < jj * 3 + 3; l++)
				if (n == a[k][l])
					return 1;
		}
		return 0;
	}
	void completed_row(){
		for (int i = 0; i < 9; i++){
			bool f = 0, f2 = 0;
			for (int k=1;k<10;k++){
				f = 0;
				for (int j = 0; j < 9; j++){
					if (a[i][j] == k){
						f = 1;
				    }
				}
				if (!f){
					vector <int>vec;
					for (int j = 0; j < 9; j++){
						if (!a[i][j]){
							f2 = 0;
							for (int l = 0; l < 9; l++){
								if (a[l][j] == k){
									f2 = 1; 
								}
							}
							if (!f2){
								int ii = i / 3, jj = j / 3;
								for (int ww = ii * 3; ww < ii * 3 + 3; ww++){
									for (int qq = jj * 3; qq < jj * 3 + 3; qq++)
										if (a[ww][qq] == k)
											f2 = 1;
								}
								if (!f2)
								vec.push_back(j);
							}
						}
					}
					if (vec.size() == 1){
						a[i][vec[0]] = k;
						ma[{i + 1, vec[0] + 1}] = k;
					}
					vec.clear();
				}
			}
		}
	}
	void completed_coln(){
		for (int i = 0; i < 9; i++){
			bool f = 0, f2 = 0;
			for (int k=1;k<10;k++){
				f = 0;
				for (int j = 0; j < 9; j++){
					if (a[j][i] == k){
						f = 1; break;
				    }
				}
				if (!f){
					vector <int>vec;
					for (int j = 0; j < 9; j++){
						if (!a[j][i]){
							f2 = 0;
							for (int l = 0; l < 9; l++){
								if (a[j][l] == k){
									f2 = 1; break;
								}
							}
							if (!f2){
								int ii = i / 3, jj = j / 3;
								for (int ww = jj * 3; ww < jj * 3 + 3; ww++){
									for (int qq = ii * 3; qq < ii * 3 + 3; qq++)
										if (a[ww][qq] == k)
											f2 = 1;
								}
								if (!f2)
								vec.push_back(j);
							}
						}
					}
					if (vec.size() == 1){
						a[vec[0]][i] = k;
						ma[{vec[0] + 1, i + 1}] = k;
					}
				}
			}
		}
	}
	bool mogodf_row(int i, int k){
		for (int j = 0; j < 9; j++){
			if (a[i][j] == k)
				return 1;
		}
		return 0;
	}
	bool mogodf_coln(int j, int k){
		for (int i = 0; i < 9; i++){
			if (a[i][j] == k)
				return 1;
		}
		return 0;
	}
	void completed_box(){
		for (int ai = 0; ai < 3; ai++){
			for (int aj = 0; aj < 3; aj++){
				for (int k = 1; k < 10; k++){
					bool f1 = 0;
					for (int i = ai * 3; i < ai * 3 + 3; i++){
						for (int j = aj * 3; j < aj * 3 + 3; j++){
							if (a[i][j] == k){
								f1 = 1;
							}
						}
					}
					if (!f1){
						vector<pair<int, int>> vec;
						for (int i = ai * 3; i < ai * 3 + 3; i++){
							for (int j = aj * 3; j < aj * 3 + 3; j++){
								if (!a[i][j]){
									if (!mogodf_coln(j, k) && !mogodf_row(i, k))
										vec.push_back({ i,j });
								}
							}
						}
						if (vec.size() == 1){
							a[vec[0].first][vec[0].second] = k;
							ma[{vec[0].first+1,vec[0].second+1}] = k;
						}
					}
				}
			}
		}
	}
	void completed_age(){
		for (int i = 0; i < 9; i++){
			for (int j = 0; j < 9; j++){
				if (get(i, j))
					continue;
				deque<int>dq;
				for (int k = 1; k <= 9; k++){
					if (!alrakm_mogood_wla_la(k, i, j))
						dq.push_back(k);
				}
				if (dq.size() == 1) {
					set(dq[0], i, j);
					ma[{i + 1, j = 1}] = dq[0];
				}
				dq.clear();
			}
		}
	}
	void print();
};
void sudoku::print()
{
	cout << endl;
	for (int i = 0; i < 9; i++){
		for (int j = 0; j < 9; j++){
			cout << a[i][j]<<' ';
			if (j % 3 == 2)
				cout << ' ';
		}
		if (i % 3 == 2)
			cout << endl;
		cout << endl;
	}
}
int main(){
	sudoku sd;
	//cout << sd.how_number() << endl;
	while (sd.how_number() != 81){
		int c = sd.how_number();
		sd.completed_row();
		sd.completed_coln();
	    sd.completed_box();
		sd.completed_age();
		if (c == sd.how_number()){
			break;
		}
	}
	//for (auto ma : ma)cout << ma.first.first << ' ' << ma.first.second << ' ' << ma.second << endl;
	//cout << sd.how_number() << endl;
	sd.print();
}
