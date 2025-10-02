# 🔫🎒 Desafio Código da Ilha – Edição Free Fire

Bem-vindo ao **Desafio Código da Ilha – Edição Free Fire!**  
Neste desafio, você irá simular o gerenciamento de um **inventário de sobrevivência** em uma ilha misteriosa, utilizando a linguagem **C**.

A empresa **MateCheck** encarregou você de desenvolver o sistema de **mochila virtual** que ajudará os sobreviventes a se prepararem para escapar da ilha.  
O desafio é dividido em três níveis: **Novato**, **Aventureiro** e **Mestre**, cada um com mais complexidade e poder.


#include <stdio.h>
#include <string.h>
#include <stdbool.h>

#define MAX_ITENS 10  // Limite de itens que a mochila pode armazenar

// ---------------------------
// Criação da struct: definir uma struct chamada Item com os campos
// char nome[30], char tipo[20] e int quantidade.
// ---------------------------
typedef struct {
    char nome[30];   // Nome do item
    char tipo[20];   // Tipo do item (arma, munição, cura, etc.)
    int quantidade;  // Quantidade do item
} Item;

// ---------------------------
// Cadastro de itens: o sistema deve permitir que o jogador
// cadastre até 10 itens informando nome, tipo (ex: arma, munição e cura) e quantidade.
// ---------------------------
void adicionarItem(Item mochila[], int *totalItens) {
    if(*totalItens >= MAX_ITENS) {  // Verifica se a mochila está cheia
        printf("Mochila cheia! Remova algum item antes de adicionar.\n");
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
    
    // Listagem dos itens registrados com seus dados: deve ocorrer após cada operação
    listarItens(mochila, *totalItens);
}

// ---------------------------
// Remoção de itens: permitir que o jogador exclua um item da mochila, informando seu nome.
// ---------------------------
void removerItem(Item mochila[], int *totalItens) {
    if(*totalItens == 0) {
        printf("Mochila vazia! Nada para remover.\n");
        return;
    }
    
    char nomeBusca[30];
    printf("Digite o nome do item que deseja remover: ");
    scanf(" %[^\n]", nomeBusca);
    
    bool encontrado = false; // Controle com flag para indicar se item foi encontrado
    for(int i = 0; i < *totalItens; i++) {
        if(strcmp(mochila[i].nome, nomeBusca) == 0) {
            encontrado = true;
            // Remove o item movendo todos os seguintes uma posição para trás
            for(int j = i; j < *totalItens - 1; j++) {
                mochila[j] = mochila[j+1];
            }
            (*totalItens)--;
            printf("Item removido com sucesso!\n");
            break;
        }
    }
    if(!encontrado) {
        printf("Item não encontrado na mochila.\n");
    }
    
    listarItens(mochila, *totalItens);
}

// ---------------------------
// Busca sequencial: implementar uma função de busca sequencial
// que localize um item na mochila com base no nome e exiba seus dados.
// ---------------------------
void buscarItem(Item mochila[], int totalItens) {
    if(totalItens == 0) {
        printf("Mochila vazia! Nada para buscar.\n");
        return;
    }
    
    char nomeBusca[30];
    printf("Digite o nome do item que deseja buscar: ");
    scanf(" %[^\n]", nomeBusca);
    
    bool encontrado = false; // Flag para indicar se item foi encontrado
    for(int i = 0; i < totalItens; i++) {
        if(strcmp(mochila[i].nome, nomeBusca) == 0) {
            encontrado = true;
            printf("\nItem encontrado!\n");
            printf("Nome: %s | Tipo: %s | Quantidade: %d\n", mochila[i].nome, mochila[i].tipo, mochila[i].quantidade);
            break;
        }
    }
    if(!encontrado) {
        printf("Item não encontrado na mochila.\n");
    }
}

// ---------------------------
// Listagem dos itens registrados: o que deve ocorrer após cada operação
// ---------------------------
void listarItens(Item mochila[], int totalItens) {
    printf("\n--- Itens na Mochila ---\n");
    if(totalItens == 0) {
        printf("Mochila vazia!\n");
        return;
    }
    for(int i = 0; i < totalItens; i++) {
        printf("%d. Nome: %s | Tipo: %s | Quantidade: %d\n", i+1, mochila[i].nome, mochila[i].tipo, mochila[i].quantidade);
    }
}

// ---------------------------
// Menu interativo com switch e do-while
// Permite adicionar, remover, listar e buscar itens.
// ---------------------------
int main() {
    Item mochila[MAX_ITENS];  // Vetor estático com capacidade para até 10 itens
    int totalItens = 0;
    int opcao;
    
    do {
        printf("\n=== Sistema de Mochila Free Fire ===\n");
        printf("1. Adicionar item\n");
        printf("2. Remover item\n");
        printf("3. Listar itens\n");
        printf("4. Buscar item por nome\n");
        printf("0. Sair\n");
        printf("Escolha uma opção: ");
        scanf("%d", &opcao);
        
        switch(opcao) {
            case 1:
                adicionarItem(mochila, &totalItens); // Adiciona item à mochila
                break;
            case 2:
                removerItem(mochila, &totalItens);   // Remove item da mochila
                break;
            case 3:
                listarItens(mochila, totalItens);    // Lista itens da mochila
                break;
            case 4:
                buscarItem(mochila, totalItens);     // Busca item na mochila
                break;
            case 0:
                printf("Saindo do sistema. Boa sorte na ilha!\n");
                break;
            default:
                printf("Opção inválida! Tente novamente.\n");
        }
        
    } while(opcao != 0);  // Repete o menu até o usuário sair
    
    return 0;
}

> Equipe de Ensino – MateCheck
