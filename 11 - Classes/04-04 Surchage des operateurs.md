# Surcharge des opérateurs

Ecrire la classe `Jour`  de telle sorte que le programme `main` fourni produise l'output attendu.

Comme l'exercice précédent mais cette fois en implémentant l'opérateur `<=>` et en minimisant le code autant que possible

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

    cout << setw(20) << "Jour()             => " << Jour()              << endl;
    cout << setw(20) << "Jour(JOUR)         => " << Jour(Jour::mardi)   << endl;
    cout << setw(20) << "Jour(int)          => " << Jour(3)             << endl;
    cout << setw(20) << "Jour(const Jour&)  => " << Jour(j1)            << endl;
    cout << setw(20) << "operator=          => " << (j2 = j1)           << endl;

    cout << endl;

    cout << setw(20) << "Jour(-3)           => " << Jour(-3)            << endl;
    cout << setw(20) << "Jour(5)            => " << Jour( 5)            << endl;
    cout << setw(20) << "Jour(10)           => " << Jour(10)            << endl;

    cout << "j1 = " << j1 << endl;
    cout << "j1 += 10            => " << (j1 += 10)                     << endl;
    cout << "j1 -= 10            => " << (j1 -= 10)                     << endl;

    cout << "--j1                => " << --j1                           << endl;
    cout << "j1--                => " << j1--                           << endl;

    cout << endl;
    cout << "j1 = " << j1                                               << endl;
    cout << "j2 = " << j2                                               << endl;
    cout << boolalpha << j1 << " <  " << j2 << " => " << (j1 <  j2)     << endl;
    cout << boolalpha << j1 << " <= " << j2 << " => " << (j1 <= j2)     << endl;
    cout << boolalpha << j1 << " >  " << j2 << " => " << (j1 >  j2)     << endl;
    cout << boolalpha << j1 << " >= " << j2 << " => " << (j1 >= j2)     << endl;
    cout << boolalpha << j1 << " == " << j2 << " => " << (j1 == j2)     << endl;
    cout << boolalpha << j1 << " != " << j2 << " => " << (j1 != j2)     << endl;

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
j1 = Vendredi
j1 += 10           => Lundi
j1 -= 10           => Vendredi
--j1               => Jeudi
j1--               => Jeudi

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
#include <compare>

using namespace std;

//------------------------------------------------------------
class Jour {
    friend ostream& operator<<(ostream& os, const Jour& j);

public:
    enum JOUR {lundi, mardi, mercredi, jeudi, vendredi, samedi, dimanche};

    Jour()                  : Jour(0)                            {}
    explicit Jour(JOUR j)   : Jour(static_cast<int>(j))          {}
    explicit Jour(int j)    : jour(static_cast<JOUR>(mod7(j)) )  {}

    Jour& operator+=(int n);
    Jour& operator-=(int n);

    Jour& operator++()      { return (*this += 1); }        // prefix
    Jour& operator--()      { return (*this -= 1); }        // prefix

    Jour  operator++(int);                                  // postfix
    Jour  operator--(int);                                  // postfix

    auto operator<=>(const Jour&) const = default;
    explicit operator string() const { return strJour[static_cast<size_t>(jour)]; }

private:
    static const array<const char*, 7> strJour;
    static int mod7 (int n) { return n % 7 < 0 ? n % 7 + 7 : n % 7; }

    JOUR jour;
};

//------------------------------------------------------------
const array<const char*, 7> Jour::strJour =
    {"Lundi", "Mardi", "Mercredi", "Jeudi", "Vendredi", "Samedi", "Dimanche"};

//------------------------------------------------------------
Jour& Jour::operator+=(int n) {
    this->jour = static_cast<JOUR>(mod7(static_cast<int>(jour) + n));
    return *this;
}

//------------------------------------------------------------
Jour& Jour::operator-=(int n) {
    this->jour = static_cast<JOUR>(mod7(static_cast<int>(jour) - n));
    return *this;
}

//------------------------------------------------------------
Jour Jour::operator++(int) {
    Jour old = *this;
    *this += 1;
    return old;
}

//------------------------------------------------------------
Jour Jour::operator--(int) {
    Jour old = *this;
    *this -= 1;
    return old;
}

//------------------------------------------------------------
ostream& operator<<(ostream& os, const Jour& j) {
    return os << string(j);
}

//------------------------------------------------------------
// canonic forms
Jour operator+ (Jour j, int  n) {return j+= n; }
Jour operator+ (int  n, Jour j) {return j+= n; }

Jour operator- (Jour j, int  n) {return j+=-n; }
Jour operator- (int  n, Jour j) {return j+=-n; }

//------------------------------------------------------------
int main() {
    cout << std::left;

    Jour j1(4);
    Jour j2;

    cout << setw(20) << "Jour()             => " << Jour()              << endl;
    cout << setw(20) << "Jour(JOUR)         => " << Jour(Jour::mardi)   << endl;
    cout << setw(20) << "Jour(int)          => " << Jour(3)             << endl;
    cout << setw(20) << "Jour(const Jour&)  => " << Jour(j1)            << endl;
    cout << setw(20) << "operator=          => " << (j2 = j1)           << endl;

    cout << endl;

    cout << setw(20) << "Jour(-3)           => " << Jour(-3)            << endl;
    cout << setw(20) << "Jour(5)            => " << Jour( 5)            << endl;
    cout << setw(20) << "Jour(10)           => " << Jour(10)            << endl;

    cout << "j1 = " << j1 << endl;
    cout << "j1 += 10            => " << (j1 += 10)                     << endl;
    cout << "j1 -= 10            => " << (j1 -= 10)                     << endl;

    cout << "--j1                => " << --j1                           << endl;
    cout << "j1--                => " << j1--                           << endl;

    cout << endl;
    cout << "j1 = " << j1                                               << endl;
    cout << "j2 = " << j2                                               << endl;
    cout << boolalpha << j1 << " <  " << j2 << " => " << (j1 <  j2)     << endl;
    cout << boolalpha << j1 << " <= " << j2 << " => " << (j1 <= j2)     << endl;
    cout << boolalpha << j1 << " >  " << j2 << " => " << (j1 >  j2)     << endl;
    cout << boolalpha << j1 << " >= " << j2 << " => " << (j1 >= j2)     << endl;
    cout << boolalpha << j1 << " == " << j2 << " => " << (j1 == j2)     << endl;
    cout << boolalpha << j1 << " != " << j2 << " => " << (j1 != j2)     << endl;

    return EXIT_SUCCESS;
}
~~~

</details>