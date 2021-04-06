# Trabalho de Programação — Pesquisa Completa & Dividir e Conquistar
### Eduardo dos Santos Lopes
### Analise e Complexidade de Algoritmos - PPCA — IFES Campus Serra
### 05-04-2021

## Problema 10908 - Largest Square
Esse problema pede para encontrar em uma matriz recebida o maior quadrado possível com os mesmos caracteres, tendo como centro o input informado com a posição X,Y.

Exemplo de Input:
~~~~
1
7 10 4
abbbaaaaaa
abbbaaaaaa
abbbaaaaaa
aaaaaaaaaa
aaaaaaaaaa
aaccaaaaaa
aaccaaaaaa
1 2
2 4
4 6
5 2
~~~~

A primeira linha informa o número de casos de testes que deverão ser resolvidos.
~~~~
1
~~~~
A segunda linha é o cabeçalho do caso de testes a seguir.
Ela contém: número de linhas da matriz; número de colunas da matriz; número de consultas a serem realizas nessa matriz
~~~~
7 10 4
~~~~
As próximas linhas contém a matriz base do caso de teste - de acordo com o informado no cabeçalho.
~~~~
abbbaaaaaa
abbbaaaaaa
abbbaaaaaa
aaaaaaaaaa
aaaaaaaaaa
aaccaaaaaa
aaccaaaaaa
~~~~
Por fim temos as consultas a serem realizas e respondidas - de acordo com o informado no cabeçalho.
~~~~
1 2
2 4
4 6
5 2
~~~~

## Solução do problema
Para solucionar este problema após receber os dados, faço a separação do cabeçalho, da matriz e das consultas. Cada consulta deve ser resolvida de maneira individual.

Busco o caracter na posição informada como centro. Exemplo:
~~~~
posição: 1,2
caracter: b
matriz - centro identificado:
..........
..b.......
..........
..........
..........
..........
..........
~~~~
Faço a verificação se é possível expandir o quadrado.
A verificação consiste de duas etapas:
~~~~
Verifico se existe dentro da matriz o quadrado maior
Verifico se o quadrado recortado possuí todos os caracteres iguais ao centro
~~~~
Caso as posições existam, faço o corte na matriz, retornando as linhas que representam o quadrado imediatamente maior que o anterior.
Exemplo das próximas posições verificadas no quadrado maior que o anterior:
~~~~
.bbb......
.bbb......
.bbb......
..........
..........
..........
..........
~~~~
Como as posições existem:
exemplo das linhas a serem testadas no quadrado maior que o anterior:
~~~~
bbb
bbb
bbb
~~~~
exemplo das próximas posições verificadas no quadrado maior que o anterior:
~~~~
abbba.....
abbba.....
abbba.....
aaaaa.....
..........
..........
..........
~~~~
Como as posições superiores não existem nessa nova tentativa a solução dessa tentativa é o maior quadrado encontrado na tentativa anterior

## Submissão ao Online Judge

Submeti o problema ao site online judge e recebi o veredito "ACCEPTED". 
Segue imagem abaixo com os resultados que obtive.
A imagem contém todos os algoritmos que obtive o veredito "ACCEPTED"

![Veredito](./10908-veredito.png)
 
~~~python
def retorna_maior_quadrado(matrix,posicao_caractere):
    #inicializa as variaveis de controle
    i = 1
    maior_quadrado = 1
    linha_ini= 0
    linha_fim=0
    coluna_ini = 0
    coluna_fim = 0 
    continuar = True
    #busca o caracter dentro da matriz para fazer as comparações
    caractere = matrix[posicao_caractere[0]][posicao_caractere[1]]
    while(continuar):
        #configura possibilidades
        #range da nova possibilidade de quadrado
        linha_ini = posicao_caractere[0]-i
        linha_fim = posicao_caractere[0]+i
        coluna_ini = posicao_caractere[1]-i
        coluna_fim = posicao_caractere[1]+i
        #verifica se algum estourou o limite da matrix
        if(linha_ini<0 or coluna_ini<0 or linha_fim>=len(matrix) or coluna_fim>=len(matrix[0])):
            #fora dos limites da matrix
            return maior_quadrado
        #Loop em todas as linha recortadas
        while(linha_ini<=linha_fim):
            linha = matrix[linha_ini][coluna_ini:coluna_fim+1]
            #verifica se a linha tem todos os caracteres iguais ao caractere informado
            if(len(linha)==linha.count(caractere)):
                #linha aceita
                linha_ini+=1
            else:
                #O quadrado anterior foi o maior, pois essa nova tentativa de maior quadrado falhou na validação
                return maior_quadrado
        #váriavel que controla o maior quadrado encontrado
        maior_quadrado+=2
        #variável que controla o corte a ser executado para determinar o inicio e fim da linha e o inicio e fim da coluna
        i+=1

def soluciona_problema_largest_square(caso_teste):
    #processa o cabeçalho desse caso de teste
    cabecalho = caso_teste.pop(0)
    M = int(cabecalho.split(" ")[0])
    N = int(cabecalho.split(" ")[1])
    O = int(cabecalho.split(" ")[2])
    grid = caso_teste[:M]
    #separa em um lista cada uma das consultas na matriz
    lista_l_s = caso_teste[M:M+O]
    #imprime o cabeçalho
    print(str(M)+" " +str(N)+ " "+ str(O))    
    #loop para fazer cada uma das consultas na matriz
    for i in lista_l_s:
        r = i.split(" ")[0]
        s = i.split(" ")[1]
        #chama o método que retorna o maior quadrado tendo como centro a posição R,S (que significam a posicão X,Y dentro da matriz)
        resultado = retorna_maior_quadrado(grid,[int(r),int(s)])
        print(resultado)

if __name__ == "__main__":
    #lista que guarda todos os testes
    todos_testes= list()
    #recebe o número de casos de testes que vão ser executados
    numero_testes = int(input(""))
    #loop para receber cada um dos casos de testes
    for i in range(numero_testes):
        casos_testes= list()
        #recebe o cabeçalho do problema
        cabecalho = input("")
        #número de linhas da matriz
        M = cabecalho.split(" ")[0]
        #número de colunas da matriz
        N = cabecalho.split(" ")[1]
        #número de consultas a serem realizadas nessa matriz
        O = cabecalho.split(" ")[2]
        casos_testes.append(cabecalho)
        grid = list()
        #loop para receber cada linha que compõem a matriz
        for i in range(int(M)):
            linha_grid = input("")
            casos_testes.append(linha_grid)
        lista_l_s = list()
        #loop para receber cada uma das consultas de mariores quadrados
        for i in range(int(O)):
            l_s = input("")
            casos_testes.append(l_s)
        todos_testes.append(casos_testes)
    
    for caso_teste in todos_testes:
        #faz a solução de cada um dos casos de testes
        soluciona_problema_largest_square(caso_teste)
~~~

#### Análise de complexidade do algoritmo

A complexidade para o pior caso do algoritmo foi calculada como:

Separei por trechos:
Esse trecho conta com whiles aninhados.
~~~~python
def retorna_maior_quadrado(matrix,posicao_caractere):
    #inicializa as variaveis de controle
    i = 1
    maior_quadrado = 1
    linha_ini= 0
    linha_fim=0
    coluna_ini = 0
    coluna_fim = 0 
    continuar = True
    #busca o caracter dentro da matriz para fazer as comparações
    caractere = matrix[posicao_caractere[0]][posicao_caractere[1]]
    while(continuar):
        #configura possibilidades
        #range da nova possibilidade de quadrado
        linha_ini = posicao_caractere[0]-i
        linha_fim = posicao_caractere[0]+i
        coluna_ini = posicao_caractere[1]-i
        coluna_fim = posicao_caractere[1]+i
        #verifica se algum estourou o limite da matrix
        if(linha_ini<0 or coluna_ini<0 or linha_fim>=len(matrix) or coluna_fim>=len(matrix[0])):
            #fora dos limites da matrix
            return maior_quadrado
        #Loop em todas as linha recortadas
        while(linha_ini<=linha_fim):
            linha = matrix[linha_ini][coluna_ini:coluna_fim+1]
            #verifica se a linha tem todos os caracteres iguais ao caractere informado
            if(len(linha)==linha.count(caractere)):
                #linha aceita
                linha_ini+=1
            else:
                #O quadrado anterior foi o maior, pois essa nova tentativa de maior quadrado falhou na validação
                return maior_quadrado
        #váriavel que controla o maior quadrado encontrado
        maior_quadrado+=2
        #variável que controla o corte a ser executado para determinar o inicio e fim da linha e o inicio e fim da coluna
        i+=1
~~~~
Complexidade: 6n + 2n**2

Trecho 2:
~~~~ python
def soluciona_problema_largest_square(caso_teste):
    #processa o cabeçalho desse caso de teste
    cabecalho = caso_teste.pop(0)
    M = int(cabecalho.split(" ")[0])
    N = int(cabecalho.split(" ")[1])
    O = int(cabecalho.split(" ")[2])
    grid = caso_teste[:M]
    #separa em um lista cada uma das consultas na matriz
    lista_l_s = caso_teste[M:M+O]
    #imprime o cabeçalho
    print(str(M)+" " +str(N)+ " "+ str(O))    
    #loop para fazer cada uma das consultas na matriz
    for i in lista_l_s:
        r = i.split(" ")[0]
        s = i.split(" ")[1]
        #chama o método que retorna o maior quadrado tendo como centro a posição R,S (que significam a posicão X,Y dentro da matriz)
        resultado = retorna_maior_quadrado(grid,[int(r),int(s)])
        print(resultado)

~~~~
7+ n*(3 + 6n + 2n**2)

Trecho 3:
~~~~ python
if __name__ == "__main__":
    #lista que guarda todos os testes
    todos_testes= list()
    #recebe o número de casos de testes que vão ser executados
    numero_testes = int(input(""))
    #loop para receber cada um dos casos de testes
    for i in range(numero_testes):
        casos_testes= list()
        #recebe o cabeçalho do problema
        cabecalho = input("")
        #número de linhas da matriz
        M = cabecalho.split(" ")[0]
        #número de colunas da matriz
        N = cabecalho.split(" ")[1]
        #número de consultas a serem realizadas nessa matriz
        O = cabecalho.split(" ")[2]
        casos_testes.append(cabecalho)
        grid = list()
        #loop para receber cada linha que compõem a matriz
        for i in range(int(M)):
            linha_grid = input("")
            casos_testes.append(linha_grid)
        lista_l_s = list()
        #loop para receber cada uma das consultas de mariores quadrados
        for i in range(int(O)):
            l_s = input("")
            casos_testes.append(l_s)
        todos_testes.append(casos_testes)
    
    for caso_teste in todos_testes:
        #faz a solução de cada um dos casos de testes
        soluciona_problema_largest_square(caso_teste)
~~~~
2+n*(8+2n+2n)+n*(7+ n*(3 + 6n + 2n**2))


<div class="math">
\begin{equation}
T(n) = 2+n*(8+2n+2n)+n*(7+ n*(3 + 6n + 2n**2))
\end{equation}

Simplificando temos: 

\begin{equation}
T(n) =   2 n^4 + 6 n^3 + 7 n^2 + 15 n + 2
\end{equation}

Simplificando, temos:

\begin{equation}
T(n) = O(2n^4)
\end{equation}

</div>