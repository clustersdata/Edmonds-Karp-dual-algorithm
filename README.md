# Edmonds-Karp-dual-algorithm

Edmonds Karp dual algorithm

#define M 211

#define LIM 99999999

//【最小费用最大流】Edmonds Karp对偶算法,复杂度 O(V^3E^2)。返回最大流,输入图

//容量矩阵g[M][M],费用矩阵w[M][M],取非零值表示有边，s为源点，t为汇点，f[M][M]返

//回流量矩阵，minw返回最小费用。

int g[M][M],w[M][M],minw,f[M][M];

int DualityEdmondsKarp(int n,int s,int t){	

	int i,j,k,c,quit,flow=0;
  
	int best[M],prev[M];
  
	minw=0;
  
	for (i=1;i<=n;i++) {
  
		for (j=1;j<=n;j++){
    
			f[i][j]=0;
      
			if (g[i][j]) {g[j][i]=0;w[j][i]=-w[i][j];}
      
		}
    
	}
  
	while (1) {
  
		for (i=1;i<=n;i++) best[i]=LIM;
    
		best[s]=0;
    
		do {
    
			quit=1;
      
			for (i=1;i<=n;i++){
      
				if (best[i]<LIM)
        
					for (j=1;j<=n;j++){
          
						if (f[i][j]<g[i][j] && best[i]+w[i][j]<best[j]){
            
							best[j]=best[i]+w[i][j];
              
							prev[j]=i;
              
							quit=0;
              
						}
            
					}
          
			}
      
		}while(!quit);
    
		if (best[t]>=LIM) break;
    
		c=LIM;
    
		j=t;
    

		while (j!=s) {
    
			i=prev[j];
      
			if (c>g[i][j]-f[i][j]) c=g[i][j]-f[i][j];
      
			j=i;
      
		}
    
		j=t;
    
		while (j!=s) {
    
			i=prev[j];
      
			f[i][j]+=c;
      
			f[j][i]=-f[i][j];
      
			j=i;
      
		}
    
		flow+=c; minw+=c*best[t];
    
	}
  
	return flow;
  
}

