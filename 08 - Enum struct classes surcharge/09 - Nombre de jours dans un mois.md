# Nombre de jours dans un mois

Soient la déclaration ci-dessous et la fonction `est_bissextile(..)` d'un exercice précédent.

~~~cpp
struct Date {
   int jour;
   int mois;
   int annee;
};
~~~


Écrire une fonction qui retourne le nombre de jours pour un mois et une année donnés.

La fonction reçoit en paramètre le *mois* et l'*année* et retourne le nombre de jours correspondant ou la valeur `0` en cas d'erreur.

**NB** Utiliser un `enum class` pour cette question.

<details>
<summary>Solutions</summary>

~~~cpp
int nbre_jours_mois(int mois, int annee) {

   enum class Liste_Mois {JANVIER = 1, FEVRIER, MARS, AVRIL, MAI, JUIN, JUILLET,
                          AOUT, SEPTEMBRE, OCTOBRE, NOVEMBRE, DECEMBRE};

   // NB : avec un "enum class", les valeurs s'écrivent avec l'opérateur "::"
   //      (Liste_Mois::JANVIER) et il n'y a pas de conversion automatique
   //      depuis un int : on convertit le mois avec static_cast.
   //      Un mois hors de 1..12 ne correspond à aucun case => default.

   switch (static_cast<Liste_Mois>(mois)) {
      case Liste_Mois::FEVRIER   : return est_bissextile(Date{1, mois, annee}) ? 29 : 28;
      case Liste_Mois::JANVIER   :
      case Liste_Mois::MARS      :
      case Liste_Mois::MAI       :
      case Liste_Mois::JUILLET   :
      case Liste_Mois::AOUT      :
      case Liste_Mois::OCTOBRE   :
      case Liste_Mois::DECEMBRE  : return 31;
      case Liste_Mois::AVRIL     :
      case Liste_Mois::JUIN      :
      case Liste_Mois::SEPTEMBRE :
      case Liste_Mois::NOVEMBRE  : return 30;
      default                    : return 0;
   }
}
~~~
</details>
