## Lecture valeur numérique ou texte

### Objectif
- Déterminer la nature de l'information à lire

### À faire
Écrire un programme simple permettant de lire une saisie utilisateur.
Selon le premier caractère saisi :
- un chiffre (`'0'` à `'9'`) => lire la valeur dans un `double`
- tout autre caractère => lire une `std::string`, y compris les espaces pouvant séparer plusieurs mots

Pour simplifier, on ne traite pas les nombres qui commencent par un signe (`-2.5`) ou par un espace : ils sont lus comme du texte.

Indication : `std::cin.peek()` permet de regarder le prochain caractère du buffer sans le retirer.

### Exemples d'utilisation

~~~
Saisissez une valeur numérique ou un mot : 3.14
Votre valeur : 3.14
~~~

~~~
Saisissez une valeur numérique ou un mot : un bateau
Votre mot : un bateau
~~~

<details>
<summary>Solution</summary>

~~~cpp
#include <iostream>
#include <cstdlib>
#include <string>

int main() {
    std::cout << "Saisissez une valeur numérique ou un mot : ";

    // Regarder le premier caractère sans le retirer du buffer
    int premier_car = std::cin.peek();

    if (premier_car >= '0' and premier_car <= '9') {
        double valeur;
        std::cin >> valeur;
        std::cout << "Votre valeur : " << valeur << '\n';
    }
    else {
        std::string mot;
        std::getline(std::cin, mot);
        std::cout << "Votre mot : " << mot << '\n';
    }

    return EXIT_SUCCESS;
}
~~~

</details>
