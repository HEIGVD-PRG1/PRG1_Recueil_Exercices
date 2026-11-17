# struct - fonction est_bissextile
Écrire une fonction *est_bissextile* qui prend en paramètre une *Date* et retourne *true* si l'année de la date reçue en paramètre est bissextile, *false* dans le cas contraire.

Une année est bissextile si :<br>
   - l'année est divisible par 400; ou<br>
   - l'année est divisible par 4 mais pas par 100

~~~cpp
struct Date {
   int jour;
   int mois;
   int annee;
};
~~~


<details>
<summary>Solutions</summary>

~~~cpp
// à noter
// paramètre en référence constante pour
// pouvoir passer une constante en paramètre

bool est_bissextile(const Date& d) {
   return d.annee % 400 == 0 or
          (d.annee % 4 == 0 and d.annee % 100 != 0);
}

~~~
</details>

