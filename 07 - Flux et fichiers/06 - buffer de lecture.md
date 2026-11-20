## Buffer de lecture

### Objectif
- Compréhension du buffer de saisie

### À faire
Déterminer avec précision ce que contient le buffer d'entrée après chacune des instructions du programme ci-dessous.

L’utilisateur saisit dans l’ordre les informations suivantes lorsque le programme est en attente.
<br>**NB :** `_`  = espace et `↩︎` = enter

- `45_23_21.2↩︎`
- `4_1_94_3.94_123↩︎`
- `7_3.45n↩︎`
- `43a1↩︎`



| Instruction 	                                          |  Buffer           | Commentaire                              |
|--------------------------------------------------------|-------------------|------------------------------------------|
| `cout << "entrer des valeurs : ";`                     |                   |                                          |
| `cin.ignore(numeric_limits<streamsize>::max(), '\n');` |                   |                                          |
| `cout << endl;`                                        |                   |                                          |
| `cin  >> a;`                                           |                   |                                          |
| `cin  >> b;`                                           |                   |                                          |
| `cin.ignore(numeric_limits<streamsize>::max(), '\n');` |                   |                                          |
| `cin  >> x;`                                           |                   |                                          |
| `cin  >> y;`                                           |                   |                                          |
| `cout << "voulez-vous sauver [o/n] : ";`               |                   |                                          |
| `cin  >> c;`                                           |                   |                                          |
| `cout << "sauver : " << c << endl;`                    |                   |                                          |
| `cin  >> a;`                                           |                   |                                          |
| `cin  >> b;`                                           |                   |                                          |
| `cin  >> x;`                                           |                   |                                          |

**Documentation :** [cin.ignore](https://cplusplus.com/reference/istream/istream/ignore)

<details>
<summary>Programme complet</summary>

~~~cpp
#include <cstdlib>   // EXIT_SUCCESS
#include <iostream>  // cout et cin
#include <limits>    // numeric_limits<streamsize>::max()

using namespace std;

int main() {
   int      a, b;
   double   x, y;
   char     c;
   
   cout << "entrer des valeurs : ";   
   cin.ignore(numeric_limits<streamsize>::max(), '\n');
   cout << endl;
   cin  >> a;
   cin  >> b;
   cin.ignore(numeric_limits<streamsize>::max(), '\n');
   cin  >> x;
   cin  >> y;
   cout << "voulez-vous sauver [o/n] : ";
   cin  >> c;
   cout << "sauver : " << c << endl;
   cin  >> a;
   cin  >> b;
   cin  >> x;

   cout << "presser ENTER pour quitter";
   cin.ignore(numeric_limits<streamsize>::max(), '\n');
   return EXIT_SUCCESS;
}
~~~

</details>

<details>
<summary>Solutions</summary>

| Instruction 	                            |  Buffer           | Commentaire                              |
|------------------------------------------|-------------------|------------------------------------------|
| `cout << "entrer des valeurs : ";`       | `∅`               | le buffer d'entrée est initialisé ∅      |
| `cin.ignore(...);`                       | `∅`               | attend une saisie, puis `45_23_21.2↩︎` est perdu |
| `cout << endl;`                          | `∅`               | ne modifie pas le buffer d'entrée        |
| `cin  >> a;`                             | `_1_94_3.94_123↩︎`| `4_1_94_3.94_123↩︎` 4 => a                |
| `cin  >> b;`                             | `_94_3.94_123↩︎`  | `_1_94_3.94_123↩︎` 1 => b                 |
| `cin.ignore(...);`                       | `∅`               | les valeurs `_94_3.94_123↩︎` sont perdues|
| `cin  >> x;`                             | `_3.45n↩︎`        | `7_3.45n↩︎` 7 => x                        |
| `cin  >> y;`                             | `n↩︎`             | `3.45n↩︎` 3.45 => y                       |
| `cout << "voulez-vous sauver [o/n] : ";` | `n↩︎`             | ne modifie pas le buffer d'entrée        |
| `cin  >> c;`                             | `↩︎`              | `n↩︎` 'n' => c                            |
| `cout << "sauver : " << c << endl;`      | `↩︎`              | ne modifie pas le buffer d'entrée        |
| `cin  >> a;`                             | `a1↩︎`            | `↩︎` restant ignoré, attente de `43a1↩︎` : 43 => a |
| `cin  >> b;`                             | `a1↩︎`            | 'a' n'est pas un `int` : échec, b = 0, le flux est planté |
| `cin  >> x;`                             | `a1↩︎`            | flux planté : rien n'est lu, x garde sa valeur (7) |

### Conclusions

1. le buffer ne peut pas être traité s'il est vide (lu, traité, ...)
1. lorsque le buffer est vide, une lecture met l'exécution du programme en pause jusqu'à la saisie de valeur(s) suivie de ENTER `↩︎`
1. les espaces (et retours à la ligne) précédant une valeur sont ignorés (consommés)
1. la lecture s'arrête dès la rencontre d'un caractère incompatible avec le type considéré (12 345 => 12, 345 et 1a2 => 1, 'a', 2)
1. les valeurs non consommées restent dans le flux (dans cet exemple : lecture de 'n' dans c)
1. une valeur incompatible avec l'opérateur de flux (ex 'a' => int) fait planter le flux (la variable reçoit 0 depuis C++11), ce qui ne déclenche pas d'erreur
1. lorsque le flux est planté, les lectures sont ignorées (le flux n'est pas utilisable, ni même pour vider le flux : il faut d'abord appeler `cin.clear()`). Ici, le `cin.ignore` final ne fait rien et le programme se termine sans attendre ENTER
1. une bonne pratique est de **vider le flux après utilisation** y compris le `↩︎` restant<br>avec typiquement `cin.ignore(numeric_limits<streamsize>::max(), '\n');`

</details>

