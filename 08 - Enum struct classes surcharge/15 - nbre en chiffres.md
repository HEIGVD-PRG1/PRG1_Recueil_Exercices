# Nombre en chiffres

Soit la déclaration

~~~cpp
enum class Chiffre { ZERO,  UN,  DEUX,  TROIS,  QUATRE,  CINQ,  SIX,  SEPT,  HUIT,  NEUF };
~~~

Écrire le code nécessaire afin de traduire une valeur positive ou nulle reçue en `int` en un `vector<Chiffre>` de chiffres correspondants.<br>
L'opérateur `<<` pour un `vector<Chiffre>` affiche les chiffres en toutes lettres.

~~~
cout << nbreToEnums(123);       // UN DEUX TROIS
~~~

<details>
<summary>Solution</summary>

~~~cpp
enum class             Chiffre { ZERO,  UN,  DEUX,  TROIS,  QUATRE,  CINQ,  SIX,  SEPT,  HUIT,  NEUF };
const array<string, 10> CHIFFRE {"ZERO","UN","DEUX","TROIS","QUATRE","CINQ","SIX","SEPT","HUIT","NEUF" };

ostream& operator<< (ostream& os, Chiffre chiffre) {
   os << CHIFFRE[static_cast<size_t>(chiffre)];
   return os;
}

ostream& operator<< (ostream& os, const vector<Chiffre>& tab) {
   for (size_t i = 0; i < tab.size(); ++i) {
      if (i != 0) os << " ";
      os << tab[i];
   }
   return os;
}

vector<Chiffre> nbreToEnums(int valeur) {

   // compter les chiffres de la valeur (au moins 1, pour 0)
   size_t nbre_chiffres = 1;
   for (int reste = valeur / 10; reste != 0; reste /= 10) {
      ++nbre_chiffres;
   }

   // remplir depuis la fin : le chiffre des unités est valeur % 10
   vector<Chiffre> result(nbre_chiffres);
   for (size_t i = nbre_chiffres; i > 0; --i) {
      result[i - 1] = static_cast<Chiffre>(valeur % 10);
      valeur /= 10;
   }

   return result;
}
~~~

-----------------------------------------------------

</details>
