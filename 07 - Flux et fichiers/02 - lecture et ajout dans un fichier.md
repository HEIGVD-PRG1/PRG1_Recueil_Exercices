# Lecture et ajout de texte dans un fichier

Modifier le programme en C++ de l'exercice 01 pour faire ce qui suit :

- Demander à l'utilisateur de saisir le nom du fichier de sortie.
- Si le fichier existe déjà, le programme ne s'arrête plus comme dans l'exercice 01 : il affiche le contenu du fichier sur la console.
- Demander à l'utilisateur de saisir du texte et l'ajouter (*append*) à la fin du fichier de sortie. Le contenu existant ne doit pas être écrasé.
- Pour terminer la saisie, l'utilisateur pourra taper #exit# sur une ligne séparée ou utiliser Ctrl+D.
- S'assurer que le programme gère les erreurs d'ouverture, de lecture et d'écriture du fichier.

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

//------------------------------------------------------------
// Affiche le contenu du fichier s'il existe.
// Retourne false en cas d'erreur.
bool lire_fichier(const std::string& nom_fichier) {
    // Un fichier inexistant n'est pas une erreur : il sera créé
    if (!std::filesystem::exists(nom_fichier)) {
        std::cout << "Le fichier n'existe pas encore, il sera créé.\n";
        return true;
    }

    std::ifstream fichier_entree(nom_fichier);

    // Vérifier si l'ouverture du fichier a réussi
    if (!fichier_entree) {
        std::cerr << "Erreur : impossible d'ouvrir le fichier en lecture.\n";
        return false;
    }

    // Lire et afficher le fichier ligne par ligne.
    // Le test se fait sur getline : la boucle s'arrête dès que la lecture échoue.
    std::cout << "Contenu actuel du fichier :\n";
    std::string ligne;
    while (std::getline(fichier_entree, ligne)) {
        std::cout << ligne << '\n';
    }

    // La lecture doit s'être arrêtée à la fin du fichier, et non sur une erreur
    if (!fichier_entree.eof()) {
        std::cerr << "Erreur : la lecture du fichier a échoué.\n";
        return false;
    }

    return true;
}

//------------------------------------------------------------
// Indique si le fichier est non vide et ne se termine pas par '\n'.
// Dans ce cas, il faut ajouter un retour à la ligne avant d'écrire à la suite,
// sinon le premier texte saisi serait collé à la dernière ligne existante.
bool manque_retour_ligne(const std::string& nom_fichier) {
    std::error_code erreur;
    const auto taille = std::filesystem::file_size(nom_fichier, erreur);
    if (erreur || taille == 0) {
        return false;
    }

    std::ifstream fichier(nom_fichier, std::ios::binary);
    fichier.seekg(-1, std::ios::end);   // se placer sur le dernier caractère
    char dernier = '\n';
    fichier.get(dernier);

    return dernier != '\n';
}

//------------------------------------------------------------
// Ajoute le texte saisi par l'utilisateur à la fin du fichier.
// Retourne false en cas d'erreur.
bool ecrire_fichier(const std::string& nom_fichier) {
    bool ajouter_retour = manque_retour_ligne(nom_fichier);

    // Ouvrir le fichier en mode ajout (append) : le contenu existant est conservé
    std::ofstream fichier_sortie(nom_fichier, std::ios::app);

    // Vérifier si l'ouverture du fichier a réussi
    if (!fichier_sortie) {
        std::cerr << "Erreur : impossible d'ouvrir le fichier en écriture.\n";
        return false;
    }

    std::string texte;
    const std::string terminer = "#exit#";

    // Demander à l'utilisateur de saisir du texte
    std::cout << "Entrez le texte à ajouter au fichier (Ctrl+D ou #exit# pour terminer la saisie) :\n";
    while (std::getline(std::cin, texte) && texte != terminer) {
        if (ajouter_retour) {
            fichier_sortie << '\n';
            ajouter_retour = false;
        }
        // Écrire le texte dans le fichier
        fichier_sortie << texte << '\n';
    }

    // Fermer le fichier et vérifier que l'écriture a réussi
    fichier_sortie.close();
    if (!fichier_sortie) {
        std::cerr << "Erreur : l'écriture dans le fichier a échoué.\n";
        return false;
    }

    std::cout << "Le texte a été ajouté avec succès au fichier.\n";

    return true;
}

//------------------------------------------------------------
int main() {
    std::string nom_fichier;

    // Demander à l'utilisateur le nom du fichier où enregistrer le texte
    std::cout << "Entrez le nom du fichier où enregistrer le texte : ";
    std::getline(std::cin, nom_fichier);

    if (!lire_fichier(nom_fichier)) {
        return EXIT_FAILURE;
    }

    if (!ecrire_fichier(nom_fichier)) {
        return EXIT_FAILURE;
    }

    return EXIT_SUCCESS;
}
~~~

</details>
