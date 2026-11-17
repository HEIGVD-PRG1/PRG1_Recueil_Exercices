## Lecture et traitement d'une saisie mixte

### Objectif
- Lire plusieurs données de natures différentes sur une même ligne de saisie et traiter ces données.

### À faire
Écrire un programme qui demande à l'utilisateur de saisir sur une même ligne les informations suivantes :
- le type d'appareil
- le nombre de passagers (entier)
- la durée du vol en heures (réel)
- la destination (peut contenir des espaces)

Les informations sont données sur une seule ligne et séparées par des virgules (`,`).

Si la saisie ne contient pas les quatre informations, ou si le nombre de passagers ou la durée ne sont pas des nombres valides, le programme affiche un message d'erreur et s'arrête.

### Indication
Utiliser des `std::istringstream` pour ce problème.

### Exemples d'utilisation

~~~
Indiquer sur une seule ligne, séparés par des virgules :
- le type d'appareil
- le nombre de passagers
- la durée du vol (en heures)
- la destination
exemple : PC12,3,1.2,Paris

PC12,3,1.2,Paris

Avion       : PC12
Passagers   : 3
Durée       : 1.2 h
Destination : Paris
~~~

<details>
<summary>Solution</summary>

~~~cpp
#include <iostream>
#include <sstream>
#include <string>
#include <cstdlib>

//------------------------------------------------------------
// Convertit un texte en entier.
// Retourne false si le texte n'est pas un entier valide (ex. "3x").
bool convertir(const std::string& texte, int& valeur) {
    std::istringstream flux(texte);
    // La lecture doit réussir et il ne doit rester que des espaces
    return (flux >> valeur) && (flux >> std::ws).eof();
}

//------------------------------------------------------------
// Même chose pour un nombre réel
bool convertir(const std::string& texte, double& valeur) {
    std::istringstream flux(texte);
    return (flux >> valeur) && (flux >> std::ws).eof();
}

//------------------------------------------------------------
int main() {
    std::cout << "Indiquer sur une seule ligne, séparés par des virgules :\n";
    std::cout << "- le type d'appareil\n";
    std::cout << "- le nombre de passagers\n";
    std::cout << "- la durée du vol (en heures)\n";
    std::cout << "- la destination\n";
    std::cout << "exemple : PC12,3,1.2,Paris\n\n";

    // Lire toute la ligne saisie par l'utilisateur
    std::string ligne;
    std::getline(std::cin, ligne);
    std::istringstream flux(ligne);

    std::string appareil;
    std::string passagers_str;
    std::string duree_str;
    std::string destination;

    // Extraction des données séparées par des virgules.
    // flux >> std::ws retire les espaces éventuels après chaque virgule.
    // La destination est lue jusqu'à la fin de la ligne.
    bool lecture_ok = std::getline(flux >> std::ws, appareil, ',')
                   && std::getline(flux, passagers_str, ',')
                   && std::getline(flux, duree_str, ',')
                   && std::getline(flux >> std::ws, destination);

    if (!lecture_ok) {
        std::cerr << "Erreur : il faut quatre informations séparées par des virgules.\n";
        return EXIT_FAILURE;
    }

    // Conversion des chaînes en nombres
    int    passagers;
    double duree;
    if (!convertir(passagers_str, passagers) || !convertir(duree_str, duree)) {
        std::cerr << "Erreur : le nombre de passagers ou la durée n'est pas valide.\n";
        return EXIT_FAILURE;
    }

    // Affichage
    std::cout << '\n';
    std::cout << "Avion       : " << appareil    << '\n';
    std::cout << "Passagers   : " << passagers   << '\n';
    std::cout << "Durée       : " << duree       << " h\n";
    std::cout << "Destination : " << destination << '\n';

    return EXIT_SUCCESS;
}
~~~

</details>
