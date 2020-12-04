#include "final.h"
#include<ctime>
#include<iostream>
#include<vector>
#define End 5
#define Player 9
#define Wall 1
#define Road 0
void create(int x, int y, std::vector<std::vector<int> > &map){
	int c[4][2] = { 0, 1, 1, 0, 0, -1, -1, 0 }; //right down left up
	int i, j, t;
	for (i = 0; i<4; i++){
		j = rand() % 4;
		t = c[i][0], c[i][0] = c[j][0], c[j][0] = t;
		t = c[i][1], c[i][1] = c[j][1], c[j][1] = t;
	}
	map[x][y] = Road;
	for (i = 0; i<4; i++){
		j = x + 2 * c[i][0];
		t = y + 2 * c[i][1];
		if (map[j][t] == Wall){
			map[x + c[i][0]][y + c[i][1]] = Road;
			create(j, t, map);
		}
	}
}
void mapcreate(int L, int W, std::vector<std::vector<int> > &map){
	int i, j;
	for (i = 0; i <= L + 1; i++){
		for (j = 0; j <= W + 1; j++){
			if (i == 0 || i == L + 1 || j == 0 || j == W + 1){
				map[i][j] = Road;
			}
			else{
				map[i][j] = Wall;
			}
		}
	}
	i = 2 * (rand() % (L / 2) + 1);
	j = 2 * (rand() % (W / 2) + 1);
	create(i, j, map);
}
void RandMap(int L, int W, std::vector<std::vector<int> > &map){
	srand(static_cast<unsigned int>(time(NULL)));
	mapcreate(L, W, map);
	map[L - 1][W - 1] = Player;
	map[2][2] = End;
	for (int i = 1; i<L + 1; i++){
		for (int j = 1; j<W + 1; j++){
			std::cout << map[i][j] << ' ';
		}
		std::cout << '\n';
	}
}
void CreateMap(int num, std::vector<std::vector<int> > &cs_yws_map_){
	int L = (num * 2 + 1);
	int W = (num * 2 + 1);
	std::vector<std::vector<int> > map(L + 2, std::vector<int>(W + 2, 0));
	RandMap(L, W, map);
	for (int i = 1; i<L + 1; i++){
		std::vector<int> bufrow;
		for (int j = 1; j<W + 1; j++){
			bufrow.push_back(map[i][j]);
			//std::cout << map[i][j] << ' ';
		}
		cs_yws_map_.push_back(bufrow);
		//std::cout << '\n';
	}
	return;
}
