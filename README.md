#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 500

typedef struct
{
    int barcode;
    char nom[50];
    float prix;
    int stock;

} Produit;

Produit produits[MAX];
int n = 0;

void chargerProduits()
{
    FILE *f;

    f = fopen("produits.txt","r");

    if(f == NULL)
        return;

    while(fscanf(f,"%d %s %f %d",
    &produits[n].barcode,
    produits[n].nom,
    &produits[n].prix,
    &produits[n].stock) != EOF)
    {
        n++;
    }

    fclose(f);
}

void sauvegarderProduits()
{
    FILE *f;

    f = fopen("produits.txt","w");

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

void afficherProduits()
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

int rechercherProduit(int code)
{
    for(int i=0;i<n;i++)
    {
        if(produits[i].barcode == code)
        return i;
    }

    return -1;
}

void ajouterProduit()
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

void supprimerProduit()
{

    int code;

    printf("Barcode produit: ");
    scanf("%d",&code);

    int pos = rechercherProduit(code);

    if(pos == -1)
    {
        printf("Produit non trouve\n");
        return;
    }

    for(int i=pos;i<n-1;i++)
    {
        produits[i] = produits[i+1];
    }

    n--;

    printf("Produit supprime\n");

}

void vente()
{

    int code;
    float total = 0;

    FILE *f = fopen("facture.txt","w");

    printf("\n===== VENTE =====\n");

    while(1)
    {

        printf("Barcode (0 stop): ");
        scanf("%d",&code);

        if(code == 0)
        break;

        int pos = rechercherProduit(code);

        if(pos == -1)
        {
            printf("Produit non trouve\n");
            continue;
        }

        if(pro
