# Formatage avec cout, gestionnaire d'étudiants

Écrire un programme en C++ qui permet à l'utilisateur de saisir les données d'un.e étudiant.e, de calculer sa moyenne et d'afficher un résumé de l'étudiant.e avec un formatage propre.

Le programme doit :

- Demander à l'utilisateur de saisir
  - le nom,
  - l'âge,
  - la note en mathématiques (sur 6), et
  - la note en PRG1 (sur 6) de l'étudiant.e.
- Calculer la moyenne de l'étudiant.e.
- Déterminer si l'étudiant.e est admis.e (si la moyenne est supérieure ou égale à 3.8).
- Afficher un tableau avec le nom, l'âge, les notes et le statut d'admission de l'étudiant.e, puis la moyenne et le résultat.

Un exemple de l'exécution du programme :

~~~
Entrez le nom de l'étudiant.e : Fouad Hanna
Entrez l'âge de l'étudiant.e : 19
Entrez la note en mathématiques (max. 6) : 5.2
Entrez la note en PRG1 (max. 6) : 5.3
Nom               Age  Note Math     Note PRG1    Admis
-------------------------------------------------------
Fouad Hanna        19        5.2           5.3      Oui
-------------------------------------------------------
Moyenne : 5.25
Résultat : Admis
~~~

Note : utiliser les fonctions de formatage et privilégier l'utilisation des fonctions pour organiser le code.

<details>
<summary>Solution</summary>

~~~cpp
#include <cstdlib>
#include <iostream>
#include <iomanip>
#include <string>
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
std::string lire_string(const std::string& message) {
    std::cout << message;
    std::string s;
    std::getline(std::cin, s);
    return s;
}

//------------------------------------------------------------
int lire_int(const std::string& message) {
    std::cout << message;
    int i;
    std::cin >> i;
    return i;
}

//------------------------------------------------------------
double lire_double(const std::string& message) {
    std::cout << message;
    double d;
    std::cin >> d;
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
    std::string nom       = lire_string("Entrez le nom de l'étudiant.e : ");
    int         age       = lire_int   ("Entrez l'âge de l'étudiant.e : ");
    double      note_math = lire_double("Entrez la note en mathématiques (max. 6) : ");
    double      note_prg1 = lire_double("Entrez la note en PRG1 (max. 6) : ");
    double      moyenne   = calculer_moyenne(note_math, note_prg1);
    bool        admis     = est_admis(moyenne);

    afficher_ligne("Nom", "Age", "Note Math", "Note PRG1", "Admis");
    afficher_ligne(nom, std::to_string(age), double_to_str(note_math), double_to_str(note_prg1), admis ? "Oui" : "Non");
    afficher_resume(moyenne, admis);

    return EXIT_SUCCESS;
}
~~~

</details>
