# Surcharge des opérateurs
Soit MonEntier une classe encapsulant un entier (de type int) sans en fournir
ni setter ni getter.

Les opérateurs `==`, `!=`, `+` et `+=` sont des membres de la classe.
L'affichage se fait avec une méthode `afficher()`.

Compléter le code ci-après de telle sorte que, à l'exécution, celui-ci produise le résultat suivant :

~~~text
n = 0
n = 1
false
true
n = 6
n = 6
n = 12
~~~

~~~cpp
int main() {
   MonEntier m0, m1(1), m2 = 5;
   const MonEntier m3{7};

   m0.afficher();
   m1.afficher();

   cout << (m0 == m1 ? "true" : "false") << endl;
   cout << (m0 != m1 ? "true" : "false") << endl;

   (m1 + m2).afficher();
   m1 += m2;
   m1.afficher();
   (m3 + m2).afficher();
}
~~~

<details>
<summary>Solution</summary>

~~~cpp
#include <iostream>
using namespace std;

class MonEntier {
public:
   MonEntier(int x = 0);

   bool       operator== (const MonEntier& rhs) const;
   bool       operator!= (const MonEntier& rhs) const;
   MonEntier  operator+  (const MonEntier& rhs) const;
   MonEntier& operator+= (const MonEntier& rhs);

   void afficher() const;

private:
   int x;
};

int main() {
   MonEntier m0, m1(1), m2 = 5;
   const MonEntier m3{7};

   m0.afficher();
   m1.afficher();

   cout << (m0 == m1 ? "true" : "false") << endl;
   cout << (m0 != m1 ? "true" : "false") << endl;

   (m1 + m2).afficher();
   m1 += m2;
   m1.afficher();
   (m3 + m2).afficher();
}

// Un opérateur membre a accès aux attributs privés de rhs :
// pas besoin de getter.

MonEntier::MonEntier(int x) : x(x) {}

bool MonEntier::operator== (const MonEntier& rhs) const {
   return x == rhs.x;
}

bool MonEntier::operator!= (const MonEntier& rhs) const {
   return not (*this == rhs);
}

// const : nécessaire pour m3 + m2, car m3 est constant
MonEntier MonEntier::operator+ (const MonEntier& rhs) const {
   return MonEntier(x + rhs.x);
}

MonEntier& MonEntier::operator+= (const MonEntier& rhs) {
   x += rhs.x;
   return *this;
}

void MonEntier::afficher() const {
   cout << "n = " << x << endl;
}
~~~

**Notes**

- `MonEntier m2 = 5;` fonctionne grâce au constructeur `MonEntier(int x = 0)`, qui permet de construire un `MonEntier` à partir d'un `int`.
- `operator+` retourne un nouvel objet et ne modifie pas `*this` (d'où le `const`), alors que `operator+=` modifie `*this` et le retourne par référence.

</details>
