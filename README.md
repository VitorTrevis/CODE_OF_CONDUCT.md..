import os
import random
import time


def criar_tabuleiro(tamanho):
    numeros = list(range(1, (tamanho * tamanho) // 2 + 1)) * 2
    random.shuffle(numeros)
    tabuleiro = [[0] * tamanho for _ in range(tamanho)]

    for i in range(tamanho):
        for j in range(tamanho):
            tabuleiro[i][j] = numeros.pop()

    return tabuleiro


def imprimir_tabuleiro(tabuleiro, tamanho, pares_encontrados, escolha1, escolha2):
    pares = list(range(127943, 129000))
    for i in range(tamanho):
        for j in range(tamanho):
            if (i, j) in pares_encontrados or escolha1[0] == i and escolha1[1] == j or escolha2[0] == i and escolha2[1] == j:
                print(chr(pares[tabuleiro[i][j]]), end=' ')
                print('\t', end=' ')
            else:
                print(str(i) + str(j) + '\t', end=' ')
        print()


def limpaTela():
    for _ in range(30):
        print()

def jogar_jogo_memoria(tamanho):
    tabuleiro = criar_tabuleiro(tamanho)
    pares_encontrados = set()
    tentativas = 0

    while len(pares_encontrados) < tamanho * tamanho:
        if tentativas != 0:
            time.sleep(1)
        limpaTela()
        imprimir_tabuleiro(tabuleiro, tamanho, pares_encontrados, (-1,-1), (-1,-1))
        numeros1 = input("Escolha a primeira carta (linha coluna): ")
        escolha1 = int(numeros1[0]), int(numeros1[1])
        numeros2 = input("Escolha a segunda carta (linha coluna): ")
        escolha2 = int(numeros2[0]), int(numeros2[1])
        imprimir_tabuleiro(tabuleiro, tamanho, pares_encontrados, escolha1, escolha2)
        if escolha1 == escolha2 or escolha1 in pares_encontrados or escolha2 in pares_encontrados:
            print("Escolha inválida. Tente novamente.")
            continue
        carta1 = tabuleiro[escolha1[0]][escolha1[1]]
        carta2 = tabuleiro[escolha2[0]][escolha2[1]]
        if carta1 == carta2:
            print("Par encontrado!")
            pares_encontrados.add(escolha1)
            pares_encontrados.add(escolha2)
        else:
            print("Cartas não coincidem. Tente novamente.")
        tentativas += 1
    print("Parabéns! Você encontrou todos os pares em {} tentativas.".format(tentativas))


if __name__ == "__main__":
    tamanho_tabuleiro = int(input("Digite o tamanho do tabuleiro (um número par): "))
    if tamanho_tabuleiro % 2 != 0:
        print("O tamanho do tabuleiro deve ser um número par.")
    else:
        jogar_jogo_memoria(tamanho_tabuleiro)
 
