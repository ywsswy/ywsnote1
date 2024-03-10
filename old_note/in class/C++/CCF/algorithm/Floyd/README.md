#include<iostream>
#include<map>
#include<set>
using namespace std;
#define Max 9
class MGraph{
public:
    int numVertexes;
    int matrix[Max][Max];
};
void Floyd(MGraph &G , int P[][Max], int D[][Max]){
	int v , w , k;
	for(v = 0 ; v < G.numVertexes ; v++){
		for(w = 0 ; w < G.numVertexes ; w++){
			D[v][w] = G.matrix[v][w];
			P[v][w] = w;
		}
	}
	for(k = 0 ; k < G.numVertexes ; k++){
		for(v = 0 ; v < G.numVertexes ; v++){
			for(w = 0 ; w < G.numVertexes ; w++){
				if(D[v][w] > D[v][k] + D[k][w]){
					D[v][w] = D[v][k] + D[k][w];
					P[v][w] = P[v][k];
				}
			}
		}
	}
}
int main(){
	freopen("in2.txt","r",stdin);
	MGraph G;
	G.numVertexes = Max;
	for(int i = 0;i<Max;i++){
		for(int j = 0;j<Max;j++){
			G.matrix[i][j] = 65535;
		}
		G.matrix[i][i] = 0;
	}
	int gn = 0;
	cin >> gn;
	for(int i = 0;i<gn;i++){
		int x = 0;
		int y = 0;
		int l = 0;
		cin >> x >> y >> l;
		G.matrix[x][y] = l;
		G.matrix[y][x] = l;
	}
	int D[Max][Max];
	int P[Max][Max];
	Floyd(G,P,D);
	return 0;
}
