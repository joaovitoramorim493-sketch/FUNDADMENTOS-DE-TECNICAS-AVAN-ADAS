# FUNDADMENTOS-DE-TECNICAS-AVAN-ADAS
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Estrutura (Fundamento)
typedef struct {
    int id;
    char nome[50];
    float valor;
} Registro;

// Função com ponteiro (Técnica Avançada)
void preencherRegistro(Registro *r, int id, char *nome, float valor) {
    r->id = id;
    strcpy(r->nome, nome);
    r->valor = valor;
}

// Função recursiva (Técnica Avançada)
int fatorial(int n) {
    if (n <= 1) return 1;
    return n * fatorial(n - 1);
}

// Função que retorna ponteiro (Alocação Dinâmica)
Registro* criarVetor(int tamanho) {
    return (Registro*) malloc(tamanho * sizeof(Registro));
}

// Função para salvar em arquivo (Manipulação de Arquivos)
void salvarArquivo(Registro *vetor, int tamanho, char *nomeArquivo) {
    FILE *arquivo = fopen(nomeArquivo, "wb");
    if (arquivo) {
        fwrite(vetor, sizeof(Registro), tamanho, arquivo);
        fclose(arquivo);
        printf("Dados salvos em %s\n", nomeArquivo);
    }
}

// Função para ler do arquivo
Registro* lerArquivo(char *nomeArquivo, int *tamanho) {
    FILE *arquivo = fopen(nomeArquivo, "rb");
    if (!arquivo) return NULL;
    
    fseek(arquivo, 0, SEEK_END);
    *tamanho = ftell(arquivo) / sizeof(Registro);
    rewind(arquivo);
    
    Registro *vetor = (Registro*) malloc((*tamanho) * sizeof(Registro));
    fread(vetor, sizeof(Registro), *tamanho, arquivo);
    fclose(arquivo);
    
    return vetor;
}

int main() {
    printf("=== FUNDAMENTOS E TECNICAS AVANCADAS ===\n\n");
    
    // 1. Ponteiros e Alocação Dinâmica
    int n = 3;
    Registro *dados = criarVetor(n);
    
    // 2. Struct e Ponteiros
    preencherRegistro(&dados[0], 1, "Produto A", 10.5);
    preencherRegistro(&dados[1], 2, "Produto B", 20.0);
    preencherRegistro(&dados[2], 3, "Produto C", 15.75);
    
    // 3. Exibindo dados
    printf("Registros criados:\n");
    for (int i = 0; i < n; i++) {
        printf("ID: %d | Nome: %s | Valor: %.2f\n", 
               dados[i].id, dados[i].nome, dados[i].valor);
    }
    
    // 4. Recursão
    printf("\nFatorial de 5: %d\n", fatorial(5));
    
    // 5. Arquivos
    salvarArquivo(dados, n, "dados.dat");
    
    // 6. Lendo do arquivo
    int tamanhoLido;
    Registro *dadosLidos = lerArquivo("dados.dat", &tamanhoLido);
    
    if (dadosLidos) {
        printf("\nDados lidos do arquivo:\n");
        for (int i = 0; i < tamanhoLido; i++) {
            printf("ID: %d | Nome: %s | Valor: %.2f\n", 
                   dadosLidos[i].id, dadosLidos[i].nome, dadosLidos[i].valor);
        }
        free(dadosLidos);
    }
    
    // Liberando memória
    free(dados);
    
    printf("\n=== FIM ===\n");
    return 0;
}
