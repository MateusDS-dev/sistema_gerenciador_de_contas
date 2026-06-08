#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct
{
    int conta;
    char nome[50];
    float saldo;
    int status;

} Registro;

void adicionar(FILE *arq)
{
    Registro pessoa;
    int pos;

    printf("\nPosicao do registro (0, 1, 2...): ");
    scanf("%d", &pos);

    fseek(arq, pos * sizeof(Registro), SEEK_SET);

    printf("Numero da conta: ");
    scanf("%d", &pessoa.conta);

    getchar();

    printf("Nome: ");
    fgets(pessoa.nome, 50, stdin);
    pessoa.nome[strcspn(pessoa.nome, "\n")] = '\0';

    printf("Saldo: ");
    scanf("%f", &pessoa.saldo);

    pessoa.status = 1;

    fwrite(&pessoa, sizeof(Registro), 1, arq);
    fflush(arq);

    printf("\nCliente salvo na posicao %d.\n", pos);
}

void buscar(FILE *arq)
{
    Registro pessoa;
    int numero;
    int achou = 0;

    printf("\nDigite a conta para consulta: ");
    scanf("%d", &numero);

    rewind(arq);

    while (fread(&pessoa, sizeof(Registro), 1, arq) == 1)
    {
        if (pessoa.conta == numero && pessoa.status == 1)
        {
            printf("\nConta : %d\n", pessoa.conta);
            printf("Nome  : %s\n", pessoa.nome);
            printf("Saldo : %.2f\n", pessoa.saldo);
            achou = 1;
            break;
        }
    }

    if (!achou)
    {
        printf("\nConta nao encontrada.\n");
    }
}

void alterarSaldo(FILE *arq)
{
    Registro pessoa;
    int numero;
    float valor;

    printf("\nConta para atualizar: ");
    scanf("%d", &numero);

    rewind(arq);

    while (fread(&pessoa, sizeof(Registro), 1, arq) == 1)
    {
        if (pessoa.conta == numero && pessoa.status == 1)
        {
            printf("Saldo atual: %.2f\n", pessoa.saldo);
            printf("Novo saldo: ");
            scanf("%f", &valor);

            pessoa.saldo = valor;

            fseek(arq, -sizeof(Registro), SEEK_CUR);
            fwrite(&pessoa, sizeof(Registro), 1, arq);
            fflush(arq);
            
            fseek(arq, 0, SEEK_CUR); 

            printf("\nSaldo updated com sucesso.\n");
            return;
        }
    }

    printf("\nConta nao encontrada.\n");
}

void excluirConta(FILE *arq)
{
    Registro pessoa;
    int numero;

    printf("\nConta para encerrar: ");
    scanf("%d", &numero);

    rewind(arq);

    while (fread(&pessoa, sizeof(Registro), 1, arq) == 1)
    {
        if (pessoa.conta == numero && pessoa.status == 1)
        {
            pessoa.status = 0;

            fseek(arq, -sizeof(Registro), SEEK_CUR);
            fwrite(&pessoa, sizeof(Registro), 1, arq);
            fflush(arq);
            
            fseek(arq, 0, SEEK_CUR); 

            printf("\nConta encerrada com sucesso.\n");
            return;
        }
    }

    printf("\nConta nao encontrada.\n");
}

void listarSequencial(FILE *arq)
{
    Registro pessoa;
    int encontrouAlgum = 0;

    printf("\n====== CLIENTES ======\n");
    while (fread(&pessoa, sizeof(Registro), 1, arq) == 1)
    {
        if (pessoa.status == 1 && pessoa.conta > 0)
        {
            printf("\nConta : %d\n", pessoa.conta);
            printf("Nome  : %s\n", pessoa.nome);
            printf("Saldo : %.2f\n", pessoa.saldo);
            encontrouAlgum = 1;
        }
    }
    
    if(!encontrouAlgum) {
        printf("\nNenhum cliente ativo encontrado.\n");
    }
    printf("\n======================\n");
}

int main()
{
    FILE *arquivo;
    int op;

    arquivo = fopen("dados.dat", "r+b");

    if (arquivo == NULL)
    {
        arquivo = fopen("dados.dat", "w+b");
        if (arquivo == NULL)
        {
            printf("Erro ao abrir/criar o arquivo.\n");
            return 1;
        }
    }

    do
    {
        printf("\n--- MENU DE MANUTENCAO ---\n");
        printf("1 - Cadastrar cliente em posicao especifica\n");
        printf("2 - Consultar cliente por conta\n");
        printf("3 - Atualizar saldo de um cliente\n");
        printf("4 - Encerrar conta (remover)\n");
        printf("5 - Listar todos os clientes\n");
        printf("6 - Restaurar leitura do inicio (rewind)\n");
        printf("7 - Encerrar\n");

        printf("\nOpcao: ");
        if (scanf("%d", &op) != 1) {
            while(getchar() != '\n'); 
            op = 0;
        }

        switch(op)
        {
            case 1:
                adicionar(arquivo);
                break;

            case 2:
                buscar(arquivo);
                break;

            case 3:
                alterarSaldo(arquivo);
                break;

            case 4:
                excluirConta(arquivo);
                break;

            case 5:
                rewind(arquivo);
                listarSequencial(arquivo);
                break;

            case 6:
                rewind(arquivo);
                printf("\n[rewind executado] Leitura restaurada para o inicio.\n");
                listarSequencial(arquivo);
                break;

            case 7:
                printf("\nPrograma encerrado.\n");
                break;

            default:
                printf("\nOpcao invalida. Tente novamente.\n");
        }

    } while (op != 7);

    fclose(arquivo);
    return 0;
}