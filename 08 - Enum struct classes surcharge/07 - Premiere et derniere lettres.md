# Première et dernière lettres d'une string

Complétez le code ci-dessous en écrivant la fonction `premiere_et_derniere_lettres`
qui retourne la première et la dernière lettre de la
chaîne de caractères (supposée non vide) reçue en paramètre.

~~~cpp
#include <iostream>
#include <string>
#include <utility>
using namespace std;

// insérez ici votre code

int main() {
   string s = "Hello";
   pair<char, char> lettres = premiere_et_derniere_lettres(s);
   cout << lettres.first << " -> " << lettres.second << endl;
}
~~~

<details>
<summary>Solutions</summary>

Plusieurs solutions sont possibles. La plus simple consiste à utiliser `std::pair`,
défini dans `<utility>`.

~~~cpp
pair<char, char> premiere_et_derniere_lettres(const string& s) {
   return {s.front(), s.back()};
}
~~~

Les deux valeurs sont ensuite lues avec `.first` et `.second`.

Mais attention, cette approche ne fonctionne que si l'on doit retourner exactement 2 valeurs,
et les noms `first` et `second` ne disent rien sur leur contenu.
Une solution plus générale consiste à définir une structure nommée servant uniquement à retourner les valeurs.
On choisit alors le nom de chaque membre.

~~~cpp
struct Lettres {
   char premiere;
   char derniere;
};

Lettres premiere_et_derniere_lettres(const string& s) {
   return {s.front(), s.back()};
}
~~~

Avec cette solution, le programme principal utilise le type `Lettres` et les noms de ses membres :

~~~cpp
int main() {
   string s = "Hello";
   Lettres lettres = premiere_et_derniere_lettres(s);
   cout << lettres.premiere << " -> " << lettres.derniere << endl;
}
~~~

</details>
