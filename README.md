#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 1000

typedef struct
{
    int barcode;
    char nom[50];
    float prix;
    int stock;
}Produit;

Produit produits[MAX];
int n = 0;

/* charger produits */

void charger()
{
    FILE *f = fopen("produits.txt","r");

    if(f==NULL)
    return;

    while(fscanf(f,"%d %s %f %d",
    &produits[n].barcode,
    produits[n].nom,
    &produits[n].prix,
    &produits[n].stock)!=EOF)
    {
        n++;
    }

    fclose(f);
}

/* sauvegarder */

void sauvegarder()
{
    FILE *f = fopen("produits.txt","w");

    for(int i=0;i<n;i++)
    {
        fprintf(f,"%d %s %.2f %d\n",
        produits[i].barcode,
        produits[i].nom,
        produits[i].prix,
        produits[i].stock);
    }

    fclose(f);
}

/* afficher produits */

void afficher()
{
    printf("\n===== LISTE PRODUITS =====\n");

    for(int i=0;i<n;i++)
    {
        printf("%d | %s | %.2f DA | stock:%d\n",
        produits[i].barcode,
        produits[i].nom,
        produits[i].prix,
        produits[i].stock);
    }
}

/* ajouter produit */

void ajouter()
{
    printf("Barcode: ");
    scanf("%d",&produits[n].barcode);

    printf("Nom: ");
    scanf("%s",produits[n].nom);

    printf("Prix: ");
    scanf("%f",&produits[n].prix);

    printf("Stock: ");
    scanf("%d",&produits[n].stock);

    n++;

    printf("Produit ajoute\n");
}

/* recherche produit */

int rechercher(int code)
{
    for(int i=0;i<n;i++)
    {
        if(produits[i].barcode==code)
        return i;
    }

    return -1;
}

/* supprimer produit */

void supprimer()
{
    int code;

    printf("Barcode produit: ");
    scanf("%d",&code);

    int pos = rechercher(code);

    if(pos==-1)
    {
        printf("Produit introuvable\n");
        return;
    }

    for(int i=pos;i<n-1;i++)
    {
        produits[i]=produits[i+1];
    }

    n--;

    printf("Produit supprime\n");
}

/* facture */

void facture()
{
    FILE *f = fopen("facture.txt","w");

    int code;
    float total=0;

    printf("\n===== VENTE =====\n");

    while(1)
    {
        printf("Barcode (0 stop): ");
        scanf("%d",&code);

        if(code==0)
        break;

        int pos = rechercher(code);

        if(pos==-1)
        {
            printf("Produit non trouve\n");
            continue;
        }

        if(produits[pos].stock<=0)
        {
            printf("Stock vide\n");
            continue;
        }

        printf("%s %.2f DA\n",
        produits[pos].nom,
        produits[pos].prix);

        fprintf(f,"%s %.2f\n",
        produits[pos].nom,
        produits[pos].prix);

        total += produits[pos].prix;

        produits[pos].stock--;
    }

    fprintf(f,"TOTAL %.2f DA\n",total);

    fclose(f);

    printf("TOTAL = %.2f DA\n",total);

}

/* statistiques */

void statistiques()
{
    int totalStock=0;

    for(int i=0;i<n;i++)
    {
        totalStock+=produits[i].stock;
    }

    printf("\nNombre produits: %d\n",n);
    printf("Total stock: %d\n",totalStock);
}

/* menu */

void menu()
{

    printf("\n====== POS SYSTEM ======\n");
    printf("1 Ajouter produit\n");
    printf("2 Afficher produits\n");
    printf("3 Rechercher produit\n");
    printf("4 Supprimer produit\n");
    printf("5 Vente / Facture\n");
    printf("6 Statistiques\n");
    printf("7 Sauvegarder\n");
    printf("8 Quitter\n");

}

int main()
{
    int choix;
    int code;

    charger();

    while(1)
    {

        menu();

        scanf("%d",&choix);

        switch(choix)
        {

            case 1:
            ajouter();
            break;

            case 2:
            afficher();
            break;

            case 3:

            printf("Barcode: ");
            scanf("%d",&code);

            int pos = rechercher(code);

            if(pos==-1)
            printf("Produit non trouve\n");
            else
            printf("%s %.2f stock:%d\n",
            produits[pos].nom,
            produits[pos].prix,
            produits[pos].stock);

            break;

            case 4:
            supprimer();
            break;

            case 5:
            facture();
            break;

            case 6:
            statistiques();
            break;

            case 7:
            sauvegarder();
            break;

            case 8:
            sauvegarder();
            exit(0);

        }

    }

}
