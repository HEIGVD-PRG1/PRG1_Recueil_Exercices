# Contrôler et tester les saisies, gestionnaire d'étudiants

Modifier le programme de l'exercice précédent (04, « Gestionnaire d'étudiants ») afin d'y ajouter un contrôle de l'état du flux `cin` et des valeurs saisies par l'utilisateur.

- L'âge de l'étudiant.e est un entier compris entre 1 et 100.
- Une note est une valeur réelle comprise entre 1 et 6.
- Une saisie qui contient autre chose que la valeur attendue (par exemple `4.9` pour l'âge ou `5abc` pour une note) est invalide.
- Si la saisie se termine (Ctrl+D), le programme affiche un message d'erreur et s'arrête.

Un exemple de l'exécution du programme avec les nouvelles fonctionnalités :

~~~
Entrez le nom de l'étudiant.e : Fouad Hanna
Entrez l'âge de l'étudiant.e : -5
Saisie invalide. Veuillez entrer un entier entre 1 et 100
f
Saisie invalide. Veuillez entrer un entier entre 1 et 100
21
Entrez la note en mathématiques (max. 6) : -2
Saisie invalide. Veuillez entrer une valeur entre 1 et 6
s
Saisie invalide. Veuillez entrer une valeur entre 1 et 6
4.9
Entrez la note en PRG1 (max. 6) : 33
Saisie invalide. Veuillez entrer une valeur entre 1 et 6
ee
Saisie invalide. Veuillez entrer une valeur entre 1 et 6
5.6
Nom               Age  Note Math     Note PRG1    Admis
-------------------------------------------------------
Fouad Hanna        21        4.9           5.6      Oui
-------------------------------------------------------
Moyenne : 5.25
Résultat : Admis
~~~

Note : privilégier l'utilisation des fonctions pour organiser le code.

<details>
<summary>Solution</summary>

~~~cpp
#include <cstdlib>
#include <iostream>
#include <iomanip>
#include <string>
#include <limits>
#include <sstream>

//------------------------------------------------------------
// Affiche une ligne du tableau, suivie d'une ligne de tirets
void afficher_ligne(const std::string& s_col1,
                    const std::string& s_col2,
                    const std::string& s_col3,
                    const std::string& s_col4,
                    const std::string& s_col5) {

    const int w_col1  = 15;
    const int w_col2  = 6;
    const int w_col3  = 11;
    const int w_col4  = 14;
    const int w_col5  = 9;
    const int w_total = w_col1 + w_col2 + w_col3 + w_col4 + w_col5;

    std::cout << std::setfill(' ')
              << std::left  << std::setw(w_col1) << s_col1
              << std::right << std::setw(w_col2) << s_col2
              << std::setw(w_col3) << s_col3
              << std::setw(w_col4) << s_col4
              << std::setw(w_col5) << s_col5
              << '\n';

    std::cout << std::setw(w_total) << std::setfill('-') << "" << '\n';
}

//------------------------------------------------------------
void afficher_resume(double moyenne, bool admis) {
    std::cout << "Moyenne : "  << moyenne                         << '\n';
    std::cout << "Résultat : " << (admis ? "Admis" : "Non admis") << '\n';
}

//------------------------------------------------------------
// Arrête le programme si la saisie est terminée (Ctrl+D),
// sinon la boucle de saisie tournerait sans fin
void verifier_fin_saisie() {
    if (std::cin.eof()) {
        std::cerr << "Erreur : fin de saisie inattendue.\n";
        std::exit(EXIT_FAILURE);
    }
}

//------------------------------------------------------------
std::string lire_string(const std::string& message) {
    std::cout << message;
    std::string s;
    std::getline(std::cin, s);
    verifier_fin_saisie();
    return s;
}

//------------------------------------------------------------
int lire_int(const std::string& message, int min, int max) {
    std::cout << message;

    int  i;
    bool valide;
    do {
        std::cin >> i;
        verifier_fin_saisie();

        // Valide si la lecture a réussi, si rien ne suit la valeur sur la ligne
        // (sinon "4.9" serait lu comme 4) et si la valeur est dans les bornes
        valide = !std::cin.fail() && std::cin.peek() == '\n' && i >= min && i <= max;
        if (!valide) {
            std::cin.clear();
            std::cout << "Saisie invalide. Veuillez entrer un entier entre " << min << " et " << max << '\n';
        }
        // Vider le reste de la ligne, y compris le '\n'
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
    } while (!valide);

    return i;
}

//------------------------------------------------------------
double lire_double(const std::string& message, double min, double max) {
    std::cout << message;

    double d;
    bool   valide;
    do {
        std::cin >> d;
        verifier_fin_saisie();

        valide = !std::cin.fail() && std::cin.peek() == '\n' && d >= min && d <= max;
        if (!valide) {
            std::cin.clear();
            std::cout << "Saisie invalide. Veuillez entrer une valeur entre " << min << " et " << max << '\n';
        }
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
    } while (!valide);

    return d;
}

//------------------------------------------------------------
double calculer_moyenne(double note_math, double note_prg1) {
    return (note_math + note_prg1) / 2.0;
}

//------------------------------------------------------------
bool est_admis(double moyenne) {
    const double min_admis = 3.8;
    return moyenne >= min_admis;
}

//------------------------------------------------------------
// Convertit un nombre réel en texte, avec le même affichage que cout
std::string double_to_str(double value) {
    std::ostringstream out_str;
    out_str << value;
    return out_str.str();
}

//------------------------------------------------------------
int main() {
    const int    age_min  = 1;
    const int    age_max  = 100;
    const double note_min = 1.0;
    const double note_max = 6.0;

    std::string nom       = lire_string("Entrez le nom de l'étudiant.e : ");
    int         age       = lire_int   ("Entrez l'âge de l'étudiant.e : ", age_min, age_max);
    double      note_math = lire_double("Entrez la note en mathématiques (max. 6) : ", note_min, note_max);
    double      note_prg1 = lire_double("Entrez la note en PRG1 (max. 6) : ", note_min, note_max);
    double      moyenne   = calculer_moyenne(note_math, note_prg1);
    bool        admis     = est_admis(moyenne);

    afficher_ligne("Nom", "Age", "Note Math", "Note PRG1", "Admis");
    afficher_ligne(nom, std::to_string(age), double_to_str(note_math), double_to_str(note_prg1), admis ? "Oui" : "Non");
    afficher_resume(moyenne, admis);

    return EXIT_SUCCESS;
}
~~~

</details>
