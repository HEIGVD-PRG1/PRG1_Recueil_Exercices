# Formatage avec cout, effet des fonctions de formatage

## Notions testées
- `std::setw()`
- `std::setfill()`
- `std::left`
- `std::right`
- `std::internal`
- `std::showpos`
- `std::noshowpos`
- Nombres négatifs
- Persistance des réglages de formatage du flux

Quel sera le résultat de l'exécution du programme suivant :

~~~cpp
#include <iostream>
#include <iomanip>
#include <cstdlib>

int main() {
    int num = -42;

    std::cout << " 1) " << std::setfill('x') << std::setw(6) << std::left     << num << '\n';
    std::cout << " 2) " << std::setfill('x') << std::setw(6) << std::right    << num << '\n';
    std::cout << " 3) " << std::setfill('x') << std::setw(6) << std::internal << num << '\n';
    std::cout << " 4) " << std::showpos      << std::setw(6) << std::right    << num << '\n';
    std::cout << " 5) " << std::noshowpos    << std::setw(6) << std::right    << num << '\n';
    std::cout << " 6) " << std::setfill('0') << std::setw(8) << std::right    << num << '\n';
    std::cout << " 7) " << std::setfill('.') << std::setw(8) << std::left     << num << '\n';

    return EXIT_SUCCESS;
}
~~~

<details>
<summary>Solution</summary>

~~~
 1) -42xxx
 2) xxx-42
 3) -xxx42
 4) xxx-42
 5) xxx-42
 6) 00000-42
 7) -42.....
~~~

Remarques :
- `setw` ne s'applique qu'à la prochaine valeur affichée ; `setfill`, `left`, `right`, `internal` et `showpos` restent actifs jusqu'à ce qu'on les change (le `'x'` est toujours utilisé aux lignes 4 et 5).
- `internal` place le remplissage entre le signe et le nombre (ligne 3).
- `showpos` ajoute un `+` devant les nombres positifs uniquement : il ne change rien pour -42 (lignes 4 et 5 identiques).

</details>
