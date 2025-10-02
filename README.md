# 🔫🎒 Desafio Código da Ilha – Edição Free Fire

Bem-vindo ao **Desafio Código da Ilha – Edição Free Fire!**  
Neste desafio, você irá simular o gerenciamento de um **inventário de sobrevivência** em uma ilha misteriosa, utilizando a linguagem **C**.

A empresa **MateCheck** encarregou você de desenvolver o sistema de **mochila virtual** que ajudará os sobreviventes a se prepararem para escapar da ilha.  
O desafio é dividido em três níveis: **Novato**, **Aventureiro** e **Mestre**, cada um com mais complexidade e poder.


#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

#define MAX_ITENS 10  // Limite de itens no vetor

// ---------------------------
// Structs
// ---------------------------
typedef struct {
    char nome[30];   // Nome do item
    char tipo[20];   // Tipo (arma, munição, cura etc.)
    int quantidade;  // Quantidade
} Item;

// Nó da lista encadeada
typedef struct No {
    Item dados;
    struct No* proximo;
} No;

// ---------------------------
// Variáveis globais
// ---------------------------
int comparacoesSequencial = 0;
int comparacoesBinaria = 0;

// ---------------------------
// Funções para VETOR
// ---------------------------
void listarItensVetor(Item mochila[], int totalItens) {
    printf("\n--- Itens na Mochila (Vetor) ---\n");
    if(totalItens == 0) {
        printf("Mochila vazia!\n");
        return;
    }
    for(int i = 0; i < totalItens; i++) {
        printf("%d. Nome: %s | Tipo: %s | Quantidade: %d\n", i+1,
               mochila[i].nome, mochila[i].tipo, mochila[i].quantidade);
    }
}

void inserirItemVetor(Item mochila[], int *totalItens) {
    if(*totalItens >= MAX_ITENS) {
        printf("Mochila cheia!\n");
        return;
    }
    printf("Digite o nome do item: ");
    scanf(" %[^\n]", mochila[*totalItens].nome);
    printf("Digite o tipo do item: ");
    scanf(" %[^\n]", mochila[*totalItens].tipo);
    printf("Digite a quantidade: ");
    scanf("%d", &mochila[*totalItens].quantidade);

    (*totalItens)++;
    printf("Item adicionado com sucesso!\n");
    listarItensVetor(mochila, *totalItens);
}

void removerItemVetor(Item mochila[], int *totalItens) {
    if(*totalItens == 0) {
        printf("Mochila vazia!\n");
        return;
    }
    char nomeBusca[30];
    printf("Digite o nome do item que deseja remover: ");
    scanf(" %[^\n]", nomeBusca);

    bool encontrado = false;
    for(int i = 0; i < *totalItens; i++) {
        if(strcmp(mochila[i].nome, nomeBusca) == 0) {
            encontrado = true;
            for(int j = i; j < *totalItens-1; j++) {
                mochila[j] = mochila[j+1];
            }
            (*totalItens)--;
            printf("Item removido com sucesso!\n");
            break;
        }
    }
    if(!encontrado) printf("Item não encontrado.\n");
    listarItensVetor(mochila, *totalItens);
}

void buscarSequencialVetor(Item mochila[], int totalItens) {
    if(totalItens == 0) {
        printf("Mochila vazia!\n");
        return;
    }
    char nomeBusca[30];
    printf("Digite o nome do item: ");
    scanf(" %[^\n]", nomeBusca);

    bool encontrado = false;
    comparacoesSequencial = 0;
    for(int i = 0; i < totalItens; i++) {
        comparacoesSequencial++;
        if(strcmp(mochila[i].nome, nomeBusca) == 0) {
            encontrado = true;
            printf("Item encontrado: %s | %s | %d\n",
                   mochila[i].nome, mochila[i].tipo, mochila[i].quantidade);
            break;
        }
    }
    if(!encontrado) printf("Item não encontrado.\n");
    printf("Comparações realizadas (sequencial): %d\n", comparacoesSequencial);
}

// Ordenação por Bubble Sort
void ordenarVetor(Item mochila[], int totalItens) {
    Item temp;
    for(int i = 0; i < totalItens-1; i++) {
        for(int j = 0; j < totalItens-1-i; j++) {
            if(strcmp(mochila[j].nome, mochila[j+1].nome) > 0) {
                temp = mochila[j];
                mochila[j] = mochila[j+1];
                mochila[j+1] = temp;
            }
        }
    }
    printf("Itens ordenados por nome!\n");
}

// Busca binária
void buscarBinariaVetor(Item mochila[], int totalItens) {
    if(totalItens == 0) {
        printf("Mochila vazia!\n");
        return;
    }
    char nomeBusca[30];
    printf("Digite o nome do item: ");
    scanf(" %[^\n]", nomeBusca);

    int inicio = 0, fim = totalItens-1, meio;
    bool encontrado = false;
    comparacoesBinaria = 0;

    while(inicio <= fim) {
        meio = (inicio+fim)/2;
        comparacoesBinaria++;
        int cmp = strcmp(mochila[meio].nome, nomeBusca);
        if(cmp == 0) {
            encontrado = true;
            printf("Item encontrado: %s | %s | %d\n",
                   mochila[meio].nome, mochila[meio].tipo, mochila[meio].quantidade);
            break;
        } else if(cmp < 0) {
            inicio = meio + 1;
        } else {
            fim = meio - 1;
        }
    }
    if(!encontrado) printf("Item não encontrado.\n");
    printf("Comparações realizadas (binária): %d\n", comparacoesBinaria);
}

// ---------------------------
// Funções para LISTA ENCADEADA
// ---------------------------
void listarItensLista(No* inicio) {
    printf("\n--- Itens na Mochila (Lista Encadeada) ---\n");
    if(inicio == NULL) {
        printf("Mochila vazia!\n");
        return;
    }
    int i = 1;
    while(inicio != NULL) {
        printf("%d. Nome: %s | Tipo: %s | Quantidade: %d\n", i++,
               inicio->dados.nome, inicio->dados.tipo, inicio->dados.quantidade);
        inicio = inicio->proximo;
    }
}

void inserirItemLista(No** inicio) {
    No* novo = (No*) malloc(sizeof(No));
    if(!novo) {
        printf("Erro de memória!\n");
        return;
    }
    printf("Digite o nome do item: ");
    scanf(" %[^\n]", novo->dados.nome);
    printf("Digite o tipo do item: ");
    scanf(" %[^\n]", novo->dados.tipo);
    printf("Digite a quantidade: ");
    scanf("%d", &novo->dados.quantidade);

    novo->proximo = *inicio;
    *inicio = novo;
    printf("Item adicionado!\n");
    listarItensLista(*inicio);
}

void removerItemLista(No** inicio) {
    if(*inicio == NULL) {
        printf("Mochila vazia!\n");
        return;
    }
    char nomeBusca[30];
    printf("Digite o nome do item: ");
    scanf(" %[^\n]", nomeBusca);

    No *atual = *inicio, *anterior = NULL;
    while(atual != NULL && strcmp(atual->dados.nome, nomeBusca) != 0) {
        anterior = atual;
        atual = atual->proximo;
    }
    if(atual == NULL) {
        printf("Item não encontrado.\n");
        return;
    }
    if(anterior == NULL) {
        *inicio = atual->proximo;
    } else {
        anterior->proximo = atual->proximo;
    }
    free(atual);
    printf("Item removido!\n");
    listarItensLista(*inicio);
}

void buscarItemLista(No* inicio) {
    if(inicio == NULL) {
        printf("Mochila vazia!\n");
        return;
    }
    char nomeBusca[30];
    printf("Digite o nome do item: ");
    scanf(" %[^\n]", nomeBusca);

    comparacoesSequencial = 0;
    while(inicio != NULL) {
        comparacoesSequencial++;
        if(strcmp(inicio->dados.nome, nomeBusca) == 0) {
            printf("Item encontrado: %s | %s | %d\n",
                   inicio->dados.nome, inicio->dados.tipo, inicio->dados.quantidade);
            printf("Comparações realizadas (sequencial): %d\n", comparacoesSequencial);
            return;
        }
        inicio = inicio->proximo;
    }
    printf("Item não encontrado.\n");
    printf("Comparações realizadas (sequencial): %d\n", comparacoesSequencial);
}

// ---------------------------
// MENU PRINCIPAL
// ---------------------------
int main() {
    Item mochila[MAX_ITENS]; 
    int totalItens = 0;
    No* inicio = NULL;

    int estrutura, opcao;

    do {
        printf("\n=== Sistema de Mochila ===\n");
        printf("Escolha a estrutura:\n1. Vetor\n2. Lista Encadeada\n0. Sair\n");
        scanf("%d", &estrutura);

        if(estrutura == 1) {
            do {
                printf("\n--- Mochila com Vetor ---\n");
                printf("1. Inserir item\n2. Remover item\n3. Listar\n4. Busca Sequencial\n5. Ordenar\n6. Busca Binária\n0. Voltar\n");
                scanf("%d", &opcao);

                switch(opcao) {
                    case 1: inserirItemVetor(mochila, &totalItens); break;
                    case 2: removerItemVetor(mochila, &totalItens); break;
                    case 3: listarItensVetor(mochila, totalItens); break;
                    case 4: buscarSequencialVetor(mochila, totalItens); break;
                    case 5: ordenarVetor(mochila, totalItens); break;
                    case 6: buscarBinariaVetor(mochila, totalItens); break;
                }
            } while(opcao != 0);

        } else if(estrutura == 2) {
            do {
                printf("\n--- Mochila com Lista Encadeada ---\n");
                printf("1. Inserir item\n2. Remover item\n3. Listar\n4. Buscar (sequencial)\n0. Voltar\n");
                scanf("%d", &opcao);

                switch(opcao) {
                    case 1: inserirItemLista(&inicio); break;
                    case 2: removerItemLista(&inicio); break;
                    case 3: listarItensLista(inicio); break;
                    case 4: buscarItemLista(inicio); break;
                }
            } while(opcao != 0);
        }

    } while(estrutura != 0);

    printf("Saindo do sistema. Boa sorte no jogo!\n");
    return 0;
}
