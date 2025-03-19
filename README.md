#include <stdio.h>
#include <stdlib.h>
typedef struct date
{
    char j[3];
    char mo[3];
    char aaaa[5];
} D;
typedef struct produit
{
    char *np;
    char ref[100];
    float m;
    D dateachats;
} p;
typedef struct liste
{
    p cellule;
    struct list*next;
} l;
l*creer_liste_produit()
{
    return NULL;
}

int est_vide(l*head)
{
    if(head==NULL)
        head=1;
    else
    {
        head=0;
    }
    return head;
}

l*creer_produit()
{
    l*new=(l)malloc(sizeof(l));
    if(new==NULL)
        printf("erreur dallocation");
    else
    {
        printf("saisir la nom du profduit:\n");
        scanf("%s",&new->p.np);
        printf("saisir la reference:\n");
        scanf("%s",&new->p.ref);
        printf("saisir le montant:\n");
        scanf("%2.f",&new->p.m);
        printf("saisir la date jj\mm\aaaa\n");
        scanf("%s",&new->p.dateachats.j);
        scanf("%s",&new->p.dateachats.mo);
        scanf("%s",&new->p.dateachats.aaaa);
    }
    new->next=NULL;
    return new;
}
void afficher_produit(l*p)
{
    printf("le nom du produit est:%s\n",p->p.np);
    printf("la reference du produit est: %s\n",p->p.ref);
    printf("le montant du produit est: %s\n",p->p.m);
    printf("la date du produit est:%s\%s\%s\n",p->p.dateachats.j,p->p.dateachats.mo,p->p.dateachats.aaaa);

}

l*ajout_debut(l*head)
{
    l*newel;
    if(head!=NULL)
        head=newel->next;
    return head;
}

void ajouter_fin(l*head)
{
    l*newm=creer_produit();
    if(newm!=NULL)
    {
        if(head==NULL)
        {
            head=newm;
        }
        else
        {
            l*courant=head;
        }
        while(courant->next!=NULL)
        {
            courant=courant->next;
        }
        courant->next=newm;
    }

}
void afficher_produits(l*head)
{
    l*c=head;
    while(c!=NULL)
    {
        afficher_produit(c->cellule);
        printf("\n");
        c=c->next;
    }
}
int longueur(l*head)
{
    int longueur=0;
    l*temp=head;
    while(temp!=NULL)
    {
        longueur++;
        temp=temp->next;
    }
    return longueur;
}
void filtrer_date(l*head,date dt)
{
    l*temp=head;
    printf("le produit achete le %s %s %s:\ n",dt.j,dt.mo,dt.aaaa);
    while(temp!=NULL)
    {
        if(strcmp (temp->cellule.dateachats.j,dt.j)==0&&strcmp(temp->cellule.dateachats.mo,dt.mo)==0&&strcmp(temp->cellule.dateachats.aaaa,dt.aaaa)==0)
        {
            afficher_produit(temp->cellule);
            printf("\n");
        }
        temp=temp->next;
    }
}
l*supprimer_debut(l*head)
{
    if(head==NULL)
        printf("la liste est vide\n");
    else
    {
        l*temp=head;
        head=head->next;
        free(temp->cellule.np);
        free(temp);
    }
    return head;
}
void supprimer_fin(Liste **tete)
{
    if (*tete == NULL)
    {
        printf("La liste est vide.\n");
        return;
    }
    if ((*tete)->suiv == NULL)
    {
        free((*tete)->cellule.Nom_produit);
        free(*tete);
        *tete = NULL;
    }
    else
    {
        Liste *temp = *tete;
        while (temp->suiv->suiv != NULL)
        {
            temp = temp->suiv;
        }
        free(temp->suiv->cellule.Nom_produit);
        free(temp->suiv);
        temp->suiv = NULL;
    }
}
Liste* rembourser(Liste *liste, char *refer) {
    if (est_vide(liste)) {
        printf("La liste est vide, rien à rembourser.\n");
        return liste;
    }

    Liste *temp = liste;
     Liste *precedent = NULL;
     while (temp != NULL) {
        if (strcmp(temp->cellule.Reference, refer) == 0) {
            if (precedent == NULL) {
                return supprimer_debut(liste);
            } else if (temp->suiv == NULL) {
                 supprimer_fin(liste);
                return liste;
                } else {
                precedent->suiv = temp->suiv;
                free(temp->cellule.Nom_produit);
                free(temp);
                return liste;
                 }
        }
        precedent = temp;
        temp = temp->suiv;
    }

    printf("Produit avec la référence %s non trouvé.\n", refer);
    return liste;
}
void liberer_liste(Liste *tete) {
    Liste *temp;
    while (tete != NULL) {
        temp = tete;
        tete = tete->suiv;
        free(temp->cellule.Nom_produit); 
        free(temp);
    }
}
int main() {
    Liste *listeProduits = creer_liste_produits();
    int choix;
    char nom[100], reference[100], jour[3], mois[3], annee[5], refRembourse[100];
    float montant;
    Date date;
    Liste *nouveauProduit;

    do {
        printf("\nMenu:\n");
        printf("1. Ajouter un produit au debut\n");
        printf("2. Ajouter un produit a la fin\n");
        printf("3. Afficher les produits\n");
        printf("4. Longueur de la liste\n");
        printf("5. Filtrer par date\n");
        printf("6. Supprimer le premier produit\n");
        printf("7. Supprimer le dernier produit\n");
        printf("8. Rembourser un produit\n");
        printf("9. Quitter\n");
        printf("Votre choix: ");
        scanf("%d", &choix);
          getchar(); 
        switch (choix) {
            case 1:
                printf("Nom du produit: ");
                fgets(nom, sizeof(nom), stdin);
                nom[strcspn(nom, "\n")] = 0;
                printf("Reference: ");
                fgets(reference, sizeof(reference), stdin);
                reference[strcspn(reference, "\n")] = 0;
                printf("Montant: ");
                scanf("%f", &montant);
                getchar();
                printf("Jour: ");
                fgets(jour, sizeof(jour), stdin);
                jour[strcspn(jour, "\n")] = 0;
                printf("Mois: ");
                fgets(mois, sizeof(mois), stdin);
                mois[strcspn(mois, "\n")] = 0;
                printf("Annee: ");
                fgets(annee, sizeof(annee), stdin);
                annee[strcspn(annee, "\n")] = 0;

                strcpy(date.jour, jour);
                strcpy(date.mois, mois);
                strcpy(date.annee, annee);

                nouveauProduit = creer_produit(nom, reference, montant, date);
                listeProduits = ajouter_debut(listeProduits, nouveauProduit);
                printf("Produit ajouté au début.\n");
                break;
            case 2:
                 printf("Nom du produit: ");
                fgets(nom, sizeof(nom), stdin);
                nom[strcspn(nom, "\n")] = 0;
                printf("Reference: ");
                fgets(reference, sizeof(reference), stdin);
                reference[strcspn(reference, "\n")] = 0;
                printf("Montant: ");
                scanf("%f", &montant);
                getchar();
                printf("Jour: ");
                fgets(jour, sizeof(jour), stdin);
                jour[strcspn(jour, "\n")] = 0;
                printf("Mois: ");
                fgets(mois, sizeof(mois), stdin);
                mois[strcspn(mois, "\n")] = 0;
                printf("Annee: ");
                fgets(annee, sizeof(annee), stdin);
                annee[strcspn(annee, "\n")] = 0;

                strcpy(date.jour, jour);
                strcpy(date.mois, mois);
                strcpy(date.annee, annee);

                nouveauProduit = creer_produit(nom, reference, montant, date);
                ajouter_fin(&listeProduits, nouveauProduit);
                printf("Produit ajouté à la fin.\n");
                break;
            case 3:
                printf("Liste des produits :\n");
                afficher_produits(listeProduits);
                break;
            case 4:
                printf("Longueur de la liste : %d\n", longueur(listeProduits));
                break;
            case 5:
                printf("Jour: ");
                fgets(jour, sizeof(jour), stdin);
                jour[strcspn(jour, "\n")] = 0;
                printf("Mois: ");
                fgets(mois, sizeof(mois), stdin);
                mois[strcspn(mois, "\n")] = 0;
                printf("Annee: ");
                fgets(annee, sizeof(annee), stdin);
                annee[strcspn(annee, "\n")] = 0;
                strcpy(date.jour, jour);
                strcpy(date.mois, mois);
                strcpy(date.annee, annee);
                filtrer_date(listeProduits, date);
                break;
            case 6:
                listeProduits = supprimer_debut(listeProduits);
                printf("Premier produit supprimé.\n");
                break;
            case 7:
                supprimer_fin(&listeProduits);
                printf("Dernier produit supprimé.\n");
                break;
            case 8:
                printf("Reference du produit à rembourser: ");
                fgets(refRembourse, sizeof(refRembourse), stdin);
                refRembourse[strcspn(refRembourse, "\n")] = 0;
                listeProduits = rembourser(listeProduits, refRembourse);
                printf("Produit remboursé.\n");
                break;
            case 9:
                printf("Au revoir!\n");
                break;
            default:
                printf("Choix invalide. Veuillez réessayer.\n");
        }
    } while (choix != 9);
    liberer_liste(listeProduits);

    return 0;
}


