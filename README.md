# Caixeiro Viajante em DFS

O **problema do caixeiro-viajante** é ligado diretamente ao problema do **ciclo halmitoniano**. Esse problema consiste num vendedor de caixa, que deve visitar N cidades, saber qual o melhor caminho tomar passando por cada cidade exatamente uma vez e voltando de onde partiu com o menor custo possivel.

## Sobre o codigo

O codigo usa de força bruta por um codigo _DFS (Depth-First Search)_ para encontrar o melhor caminho usando a **TSPLIB brasil58**.

O codigo le o arquivo nomeado como _"entrada.txt"_ com os dados padroes da **TSPLIB**.

```
    FILE *fd;
    char buf[20];

     /* Leitura do arquivo de entrada.*/
    fd=fopen("entrada.txt","r");
    do{
	fscanf(fd,"%s",buf);
	if(strcmp(buf,"DIMENSION:")==0)
	    fscanf(fd," %d",&N);
	else if(strcmp(buf,"DIMENSION")==0)
	    fscanf(fd," : %d",&N);
    }while(strcmp(buf,"EDGE_WEIGHT_SECTION")!=0);

```

Apos isso aloca memoria para os vetores e principalmente para uma matriz NxN, espelhada pela sua diagonal principal que simboliza todas as opçoes possiveis de movimentaçao.

```
ciclo=(int*)malloc(N*sizeof(int));
    explorado=(int*)malloc(N*sizeof(int));
    melhor_ciclo=(int*)malloc(N*sizeof(int));
    g=(int**)malloc(N*sizeof(int*));
    for(int i=0;i<N;i++)
	g[i]=(int*)malloc(N*sizeof(int));
    
    for(int i=0;i<N;i++){
	g[i][i]=0;
	for(int j=i+1;j<N;j++){
	    fscanf(fd,"%d",&g[i][j]);
	    g[j][i]=g[i][j];
	}
    }
```

Exemplo de uma matriz de 4 cidades:

$$\begin{matrix}
0 & 1 & 1 & 1\\
1 & 0 & 2 & 2\\
1 & 2 & 0 & 3\\
1 & 2 & 3 & 0
\end{matrix}$$

Entao o codigo chama uma função baseada em **DFS** que ao inves de usar o sistema de cores _"Branco"_, _"Cinza"_ e _"Preto"_, utiliza a marcação de vertice _"explorado"_ e _"nao explorado"_ onde no retorno o vertice é marcado como nao explorado para assim poder fazer todas combinaçoes possiveis.


```
void dfs(int v, int nivel)
{
    int dist;
    explorado[v]=1;
    ciclo[nivel]=v;
    if(nivel==N-1){
	
	dist=medeciclo(ciclo);
	if(dist<min){
	    min=dist;
	    for(int j=0;j<N;j++) 
		melhor_ciclo[j]=ciclo[j];
	}
    }
    for(int i=0;i<N;i++){
	if(!explorado[i]){
	    dfs(i,nivel+1);
    	    explorado[i]=0;
	}
    }
}
```

E por fim imprime os resultados.

_Resultado com N=14:_
```
0
4
10
3
6
2
8
9
7
5
13
11
1
12

real    17m33,952s
user    17m33,905s
sys     0m0,005s
```

## Tempo de execução

O grande problema do codigo do caixeiro viajante é seu tempo de execuçao. O codigo tem um custo de N! sendo N o numero de cidades.

Utilizando a funçao time do linux foi possivel monitorar esse crescimento:

N   | Tempo de execução
:---------: | :------:
<9 | 0m0,005s
 9 | 0m0,028s
10 | 0m0,105s
11 | 0m0,427s
12 | 0m4,335s
13 | 1m7,845s
14 | 17m33,025s



## Referencias
 - Cormen, Thomas H, et al. Introduction to Algorithms. 2009.
 - TSPLIB [http://elib.zib.de/pub/mp-testdata/tsp/tsplib/tsplib.html]
 