# Surcharge des opérateurs

Écrire la classe `Jour` de telle sorte que le programme `main` fourni produise l'output attendu.

- Les jours sont représentés par un `enum class JOUR` déclaré dans la classe.
- Les 6 opérateurs de comparaison sont écrits à la main : ne pas utiliser l'opérateur `<=>`.
- Le nom du jour est obtenu avec une méthode `toString()`, utilisée par l'opérateur `<<`.

~~~cpp
#include <iostream>
#include <cstdlib>
#include <array>
#include <string>
#include <iomanip>

using namespace std;

//------------------------------------------------------------
// class Jour
// - déclaration
// - définitions
//------------------------------------------------------------

//------------------------------------------------------------
int main() {
    cout << std::left;

    Jour j1(4);
    Jour j2;

    cout << setw(20) << "Jour()             => " << Jour()                    << endl;
    cout << setw(20) << "Jour(JOUR)         => " << Jour(Jour::JOUR::mardi)   << endl;
    cout << setw(20) << "Jour(int)          => " << Jour(3)                   << endl;
    cout << setw(20) << "Jour(const Jour&)  => " << Jour(j1)                  << endl;
    cout << setw(20) << "operator=          => " << (j2 = j1)                 << endl;

    cout << endl;

    cout << setw(20) << "Jour(-3)           => " << Jour(-3)                  << endl;
    cout << setw(20) << "Jour(5)            => " << Jour( 5)                  << endl;
    cout << setw(20) << "Jour(10)           => " << Jour(10)                  << endl;

    j1 = Jour(Jour::JOUR::mercredi);

    cout << endl;
    cout << "j1 = " << j1                                                     << endl;
    cout << "j2 = " << j2                                                     << endl;
    cout << j1 << " <  " << j2 << " => " << (j1 <  j2 ? "true" : "false")     << endl;
    cout << j1 << " <= " << j2 << " => " << (j1 <= j2 ? "true" : "false")     << endl;
    cout << j1 << " >  " << j2 << " => " << (j1 >  j2 ? "true" : "false")     << endl;
    cout << j1 << " >= " << j2 << " => " << (j1 >= j2 ? "true" : "false")     << endl;
    cout << j1 << " == " << j2 << " => " << (j1 == j2 ? "true" : "false")     << endl;
    cout << j1 << " != " << j2 << " => " << (j1 != j2 ? "true" : "false")     << endl;

    return EXIT_SUCCESS;
}
~~~

<details>
<summary>Output</summary>

~~~text
Jour()             => Lundi
Jour(JOUR)         => Mardi
Jour(int)          => Jeudi
Jour(const Jour&)  => Vendredi
operator=          => Vendredi

Jour(-3)           => Vendredi
Jour(5)            => Samedi
Jour(10)           => Jeudi

j1 = Mercredi
j2 = Vendredi
Mercredi <  Vendredi => true
Mercredi <= Vendredi => true
Mercredi >  Vendredi => false
Mercredi >= Vendredi => false
Mercredi == Vendredi => false
Mercredi != Vendredi => true
~~~
</details>

<details>
<summary>Solution</summary>

~~~cpp
#include <iostream>
#include <cstdlib>
#include <array>
#include <string>
#include <iomanip>

using namespace std;

//------------------------------------------------------------
// noms des jours, dans l'ordre de l'enum class Jour::JOUR
const array<string, 7> NOMS_JOURS =
    {"Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"};

//------------------------------------------------------------
// reste de la division par 7, toujours entre 0 et 6 (aussi pour n < 0)
int mod7(int n) {
    int reste = n % 7;
    return reste < 0 ? reste + 7 : reste;
}

//------------------------------------------------------------
class Jour {
public:
    enum class JOUR {lundi, mardi, mercredi, jeudi, vendredi, samedi, dimanche};

    Jour()              : Jour(0)                           {}
    Jour(JOUR j)        : jour(j)                           {}
    Jour(int j)         : jour(static_cast<JOUR>(mod7(j)))  {}
    Jour(const Jour& j) : jour(j.jour)                      {}
    Jour& operator= (const Jour& j)                         { jour = j.jour; return *this; }

    bool operator <  (const Jour& j) const { return      this->jour <  j.jour;      }
    bool operator <= (const Jour& j) const { return not (this->jour >  j.jour);     }
    bool operator >  (const Jour& j) const { return      j.jour     <  this->jour;  }
    bool operator >= (const Jour& j) const { return not (this->jour <  j.jour);     }
    bool operator == (const Jour& j) const { return      this->jour == j.jour;      }
    bool operator != (const Jour& j) const { return not (this->jour == j.jour);     }

    string toString() const { return NOMS_JOURS[static_cast<size_t>(jour)]; }

private:
    JOUR jour;
};

//------------------------------------------------------------
ostream& operator<<(ostream& os, const Jour& j) {
    return os << j.toString();
}

//------------------------------------------------------------
int main() {
    cout << std::left;

    Jour j1(4);
    Jour j2;

    cout << setw(20) << "Jour()             => " << Jour()                    << endl;
    cout << setw(20) << "Jour(JOUR)         => " << Jour(Jour::JOUR::mardi)   << endl;
    cout << setw(20) << "Jour(int)          => " << Jour(3)                   << endl;
    cout << setw(20) << "Jour(const Jour&)  => " << Jour(j1)                  << endl;
    cout << setw(20) << "operator=          => " << (j2 = j1)                 << endl;

    cout << endl;

    cout << setw(20) << "Jour(-3)           => " << Jour(-3)                  << endl;
    cout << setw(20) << "Jour(5)            => " << Jour( 5)                  << endl;
    cout << setw(20) << "Jour(10)           => " << Jour(10)                  << endl;

    j1 = Jour(Jour::JOUR::mercredi);

    cout << endl;
    cout << "j1 = " << j1                                                     << endl;
    cout << "j2 = " << j2                                                     << endl;
    cout << j1 << " <  " << j2 << " => " << (j1 <  j2 ? "true" : "false")     << endl;
    cout << j1 << " <= " << j2 << " => " << (j1 <= j2 ? "true" : "false")     << endl;
    cout << j1 << " >  " << j2 << " => " << (j1 >  j2 ? "true" : "false")     << endl;
    cout << j1 << " >= " << j2 << " => " << (j1 >= j2 ? "true" : "false")     << endl;
    cout << j1 << " == " << j2 << " => " << (j1 == j2 ? "true" : "false")     << endl;
    cout << j1 << " != " << j2 << " => " << (j1 != j2 ? "true" : "false")     << endl;

    return EXIT_SUCCESS;
}
~~~

</details>
