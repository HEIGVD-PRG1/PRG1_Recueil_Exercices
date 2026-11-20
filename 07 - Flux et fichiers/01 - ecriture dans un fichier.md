# Sauvegarde de texte dans un fichier

Écrire un programme en C++ qui fait ce qui suit :

- Demander à l'utilisateur de saisir le nom du fichier de sortie.
- Si le fichier existe déjà, il ne doit pas être écrasé : le programme affiche un message d'erreur et s'arrête.
- Demander à l'utilisateur de saisir du texte et enregistrer le texte saisi dans le fichier de sortie. Répéter cette action tant que l'utilisateur n'a pas terminé.
- Pour terminer la saisie, l'utilisateur pourra taper #exit# sur une ligne séparée ou utiliser Ctrl+D.
- S'assurer que le programme gère les erreurs d'ouverture et d'écriture du fichier.

Ps : simulation du *EOF* (End of File)<br>
- Ctrl+D sur Unix (Linux et macOS).<br>
- Ctrl+Z sur Windows, en début de ligne, suivi d'Entrée.

<details>
<summary>Solution</summary>

~~~cpp
#include <iostream>
#include <fstream>
#include <string>
#include <cstdlib>
#include <filesystem>

int main() {
    std::string nom_fichier;

    // Demander à l'utilisateur le nom du fichier où enregistrer le texte
    std::cout << "Entrez le nom du fichier où enregistrer le texte : ";
    std::getline(std::cin, nom_fichier);

    // Refuser d'écraser un fichier existant (C++17 : std::filesystem)
    if (std::filesystem::exists(nom_fichier)) {
        std::cerr << "Erreur : le fichier existe déjà.\n";
        return EXIT_FAILURE;
    }

    // Ouvrir le fichier en mode écriture
    std::ofstream fichier_sortie(nom_fichier);

    // Vérifier si l'ouverture du fichier a réussi
    if (!fichier_sortie) {
        std::cerr << "Erreur : impossible d'ouvrir le fichier.\n";
        return EXIT_FAILURE;
    }

    std::string texte;
    const std::string terminer = "#exit#";

    // Demander à l'utilisateur de saisir du texte
    std::cout << "Entrez le texte à enregistrer dans le fichier (Ctrl+D ou #exit# pour terminer la saisie) :\n";
    while (std::getline(std::cin, texte) && texte != terminer) {
        // Écrire le texte dans le fichier
        fichier_sortie << texte << '\n';
    }

    // Fermer le fichier et vérifier que l'écriture a réussi
    fichier_sortie.close();
    if (!fichier_sortie) {
        std::cerr << "Erreur : l'écriture dans le fichier a échoué.\n";
        return EXIT_FAILURE;
    }

    std::cout << "Le texte a été enregistré avec succès dans le fichier.\n";

    return EXIT_SUCCESS;
}

~~~

</details>