import random #Importa a biblioteca Random para tratar de aleatoriedade.

field = [
  ["-"] * 5,
  ["-"] * 5,
  ["-"] * 5,
  ["-"] * 5,
  ["-"] * 5,
]
#As linhas 3 à 9 criam uma matriz colocando o caractere "-" multiplicado por 5 em cada linha criando o campo do jogo.
adj = 0 #Cria a variável adj que irá contar a quantidade de bombas adjascentes ao espaço selecionado. 
unrevealed = 20 #Esta linha contará de forma decrescente os espaços não revelados informando se o jogo continuará ou não.

positions = [(x, y) for x in range(5) for y in range(5)] #Criará uma matriz x,y na qual será sorteados os locais onde ficarão as bombas.
for _ in range(5): #Fará a iteração 5 vezes na matriz. 
   x, y = random.choice(positions) #Escolherá uma coordenada x,y para inserir a bomba.
   field[x][y] = 'b' #Trocará a coordenada por 'b' representando a bomba.
   positions.remove((x, y)) #Removerá a posição da matriz evitando que a bomba seja posta em local repetido. 

print('\n'.join([''.join(['-' if cell == 'b' else cell for cell in row]) for row in field]))
#Essa linha tem a função de imprimir o campo atualizado na tela, substituindo as bombas ('b') por traços ('-'). 
#for row in field: Este é um loop que percorre cada linha no campo (field). Em cada iteração, row é uma lista representando uma linha no campo. 
#[''.join(['-' if cell == 'b' else cell for cell in row]) for row in field]. Dentro deste loop, há uma expressão de lista que percorre cada célula (cell) em cada linha (row) do campo. Para cada célula, verifica-se se ela contém uma bomba ('b'), se for uma bomba, substituímos 'b' por '-', caso contrário mantemos o valor original da célula, o resultado é uma lista que representa uma linha do campo, onde as bombas foram substituídas por traços.
#'\n'.join(...) Finalmente, usamos '\n'.join(...) para juntar todas as linhas do campo em uma única string, separando-as com novas linhas ('\n'). Isso cria uma representação do campo como uma única string com várias linhas.

while (unrevealed != 0): #Repete o código enquanto a variável unrevealed for diferente de 0, ou seja, enquanto houverem espaços sem bomba a serem revelados. Visto que a matriz é de 5x5 são 25 espaços, levando em conta que 5 são bombas restam 20 espaços a serem revelados

 guess = input("Selecione primeiro a linha e depois a coluna com espaços e sem vírgula (ex: 1 2): ") #Guarda o palpite do jogador.
 x, y = map(int, guess.split()) #map é uma função que aplica uma função a cada item de um iterável, no caso a variável guess vai receber dois valores distintos em uma única entrada. Enquanto o .split() vai dividir os valores em 2 variáveis x e y referentes às coordenadas, o map vai aplicar a função int() às duas, transformando-as em números inteiros para a comparação. 
 x -= 1
 y -= 1
 #As linhas 30 e 31 subtraem 1 do valor inserido pelo usuário, isto porque a iteração das linhas e colunas em código se dará entre 0 e 4, porém em linguagem humana o jogador escolherá entre 1 e 5, portanto a conversão para iteração se dará em código poupando o jogador do cálculo.
 if (x < 1 or x > 5) or (y < 1 or y > 5) or guess.isdigit():
        print("Digite apenas números entre 1 e 5")
        continue
#As linhas 33 a 35 verificam se os valores de x e y estão dentro do intervalo permitido e se não foi inserida letra ao invés de número com a função isdigit().
 if field[x][y] == 'b':
    print("Você perdeu!")
    break
    #37 à 39 verificam se as coordenadas contêm 'b' que representam bombas, informando ao usuário que ele perdeu e usando o break para encerrar o código. 
 else:
    for dx in range(-1, 2):
        for dy in range(-1, 2):
            nx, ny = x + dx, y + dy
            if 0 <= nx < 5 and 0 <= ny < 5 and field[nx][ny] == 'b':
                adj += 1
   #42 à 46 fará uma iteração de y dentro de uma iteração de x verificando os valores possíveis ao redor da coordenada escolhida. o dx e o dy são responsáveis pela iteração enquanto o nx e ny verificam às próximas coordenadas atualizando nx e ny. A linha 45 verifica se a coordenada adjascente está dentro do campo e se possui bomba fazendo a variável adj na linha 46 aumentar em 1 para cada bomba próxima da coordenada.             
    unrevealed -= 1 #Como não se trata de bomba, caso tratado nas linhas 37 à 39, o sistema irá decrementar a variável unrevealed para evitar um loop infinito na linha 26.
    field[x][y] = str(adj) #Irá converter o valor de adj em string adicionando-o na coordenada escolhida.
    adj = 0 #Zera o valor da variável para ser alterado novamente no próximo movimento.
    print('\n'.join([''.join(['-' if cell == 'b' else cell for cell in row]) for row in field])) #Imprime o campo atualizado ainda escondendo as bombas como na linha 24, porém o adj que foi inserido na linha 49 não foi tratado, imprimindo assim o campo atualizado com a quantidade de bombas adjascentes à coordenada escolhida.

if (unrevealed == 0): #Se o unrevealed é zerado significa que o campo foi decrementado de 20 até 0 sem nenhuma bomba ter sido encontrada na linha 37, caracterizando vitória.
  print("Você venceu!")
