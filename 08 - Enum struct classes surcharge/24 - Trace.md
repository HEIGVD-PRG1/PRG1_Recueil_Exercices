# Trace

Soient la classe suivante et les fonctions.
Les objets n'ont pas d'identifiant : seule la lettre (`data`) est affichée.

~~~cpp
class Trace;
ostream& operator<< (ostream& os, const Trace& t);   // déclaré avant la classe, qui l'utilise

class Trace {
public:
   Trace()                   : data('a')  { cout << "C0(" << *this << ") "; }
   Trace(char data)          : data(data) { cout << "Ci(" << *this << ") "; }
   Trace(const Trace& other)              { this->data = other.data;
                                            cout << "CC(" << other << "->" << *this << ") "; }
   ~Trace()                               { cout << "D("  << *this << ") "; }
   Trace& operator= (const Trace& other)  { this->data = other.data;
                                            cout << "=C(" << other << "->" << *this << ") ";
                                            return *this; }
   char getData() const                   { return data; }
private:
   char data;
};

ostream& operator<< (ostream& os, const Trace& t) {
   return os << t.getData();
}

Trace f1 (char i)         { cout << "f1 : "; return Trace(i); }
Trace f2 (Trace t)        { cout << "f2 : "; return t;        }
Trace f3 (const Trace& t) { cout << "f3 : "; return t;        }
~~~

Que produisent les déclarations ou instructions suivantes ?

<br>

~~~cpp
// no 1
Trace trace1a;
~~~

<details>
<summary>Solution</summary>

~~~
C0(a)
~~~

------------------------------------------------------------

</details>

~~~cpp
// no 2
Trace trace1b('a');
~~~

<details>
<summary>Solution</summary>

~~~
Ci(a)
~~~

------------------------------------------------------------

</details>

~~~cpp
// no 3
Trace trace1c(trace1a);
~~~

<details>
<summary>Solution</summary>

~~~
CC(a->a)
~~~

------------------------------------------------------------

</details>

~~~cpp
// no 4
Trace trace1d(Trace('b'));
~~~

<details>
<summary>Solution</summary>

~~~
Ci(b)
~~~

------------------------------------------------------------

</details>

~~~cpp
// no 5
Trace trace1e(f1('c'));
~~~

<details>
<summary>Solution</summary>

~~~
f1 : Ci(c)
~~~

**Note**

- `f1('c')` crée un objet `Trace` avec le caractère `c` en utilisant le constructeur `Trace(char data)`, affichant `Ci(c)`.
- L'objet `Trace` temporaire retourné par `f1('c')` est directement utilisé pour initialiser `trace1e` sans nécessiter de copie explicite grâce à l'optimisation du compilateur RVO (Return Value Optimization).

------------------------------------------------------------

</details>

~~~cpp
// no 6
Trace trace1f(f2(trace1a));
~~~

<details>
<summary>Solution</summary>

~~~
CC(a->a) f2 : CC(a->a) D(a)
~~~

- passage de `trace1a` par valeur (copie) au paramètre `t` de `f2` => premier `CC(a->a)`
- utilisation du retour de `f2` (copie de `t`) pour construire `trace1f` => second `CC(a->a)`
- fin de `f2` et destruction du paramètre `t` => `D(a)`

------------------------------------------------------------

</details>

~~~cpp
// no 7
Trace trace1g(f3(trace1a));
~~~

<details>
<summary>Solution</summary>

~~~
f3 : CC(a->a)
~~~

- le paramètre `t` (référence vers `trace1a`) est directement utilisé pour construire `trace1g` => `CC(a->a)`

------------------------------------------------------------

</details>

~~~cpp
// no 8
vector<Trace> vect(3, Trace('b'));
~~~

<details>
<summary>Solution</summary>

~~~
Ci(b) CC(b->b) CC(b->b) CC(b->b) D(b)
~~~

- création de l'objet temporaire `Trace('b')` => `Ci(b)`
- 3 copies dans le vecteur => `CC(b->b) CC(b->b) CC(b->b)`
- suppression de l'objet temporaire => `D(b)`

Note

~~~
vect.size()     => 3
vect.capacity() => 3
~~~

------------------------------------------------------------

</details>

~~~cpp
// no 9
vect.push_back(trace1a);
~~~

<details>
<summary>Solution</summary>

~~~
CC(a->a) CC(b->b) CC(b->b) CC(b->b) D(b) D(b) D(b)
~~~

- mise à jour de la capacité (3 => 6) et de la taille (3 => 4) du vecteur 
- construction par copie du nouvel élément => `CC(a->a)`
- construction par copie des 3 éléments dans le nouvel emplacement mémoire => `CC(b->b) CC(b->b) CC(b->b)`
- suppression des 3 éléments dans l'ancien emplacement mémoire => `D(b) D(b) D(b)`

------------------------------------------------------------

</details>

~~~cpp
// no 10
Trace trace2a('b');
trace2a = trace1a;
~~~

<details>
<summary>Solution</summary>

~~~
Ci(b) =C(a->a)
~~~

- construction de `trace2a('b')` => `Ci(b)`
- affectation de `trace1a` dans `trace2a`, qui contient maintenant `'a'` => `=C(a->a)`

------------------------------------------------------------

</details>

~~~cpp
// no 11
trace2a = f1('c');
~~~

<details>
<summary>Solution</summary>

~~~
f1 : Ci(c) =C(c->c) D(c)
~~~

- appel de `f1` avec `'c'` comme paramètre
- construction dans la fonction de l'objet retourné => `Ci(c)`
- affectation dans `trace2a` du retour de `f1` => `=C(c->c)`
- destruction de l'objet temporaire retourné par `f1` => `D(c)`

------------------------------------------------------------

</details>

~~~cpp
// no 12
trace2a = f2(trace1a);
~~~

<details>
<summary>Solution</summary>

~~~
CC(a->a) f2 : CC(a->a) =C(a->a) D(a) D(a)
~~~

- appel de `f2` et passage par valeur (copie) de `trace1a` => premier `CC(a->a)`
- retour du paramètre `t` (copie) => second `CC(a->a)`
- affectation dans `trace2a` => `=C(a->a)`
- suppression de l'objet utilisé pour le retour, puis du paramètre `t` => `D(a) D(a)`

------------------------------------------------------------

</details>

~~~cpp
// no 13
trace2a = f3(trace1a);
~~~

<details>
<summary>Solution</summary>

~~~
f3 : CC(a->a) =C(a->a) D(a)
~~~

- passage du paramètre par ref constante `const Trace& t`
- retour du paramètre `t` (copie) => `CC(a->a)`
- affectation dans `trace2a` => `=C(a->a)`
- suppression de l'objet utilisé pour le retour => `D(a)`

------------------------------------------------------------

</details>

~~~cpp
// no 14
{
   Trace trace1b('a');
}
~~~

<details>
<summary>Solution</summary>

~~~
Ci(a) D(a)
~~~

- construction de `trace1b('a')` => `Ci(a)`
- sortie du bloc `{..}`, donc destruction de l'objet => `D(a)`

------------------------------------------------------------

</details>

~~~cpp
// no 15
{
   vector<Trace> v(3);
}
~~~

<details>
<summary>Solution</summary>

~~~
C0(a) C0(a) C0(a) D(a) D(a) D(a)
~~~

- construction de 3 objets pour le vecteur (constructeur par défaut) => `C0(a) C0(a) C0(a)`
- sortie du bloc `{..}`, donc destruction des 3 objets => `D(a) D(a) D(a)`

------------------------------------------------------------

</details>

~~~cpp
// no 16
{
   vector<Trace> v(2, Trace('a'));
}
~~~

<details>
<summary>Solution</summary>

~~~
Ci(a) CC(a->a) CC(a->a) D(a) D(a) D(a)
~~~

- construction de l'objet temporaire `Trace('a')` => `Ci(a)`
- copie de cet objet dans les 2 éléments du vecteur => `CC(a->a) CC(a->a)`
- destruction de l'objet temporaire => `D(a)`
- sortie du bloc `{..}`, donc destruction des objets du vecteur => `D(a) D(a)`

------------------------------------------------------------

</details>

~~~cpp
// no 17
{
   vector<Trace> v(2, trace1a);
}
~~~

<details>
<summary>Solution</summary>

~~~
CC(a->a) CC(a->a) D(a) D(a)
~~~

- copie de `trace1a` dans les éléments du vecteur => `CC(a->a) CC(a->a)`
- sortie du bloc `{..}`, donc destruction des objets du vecteur => `D(a) D(a)`

------------------------------------------------------------

</details>

------------------------------------------------------------
<details>
<summary>code complet</summary>

~~~cpp
#include <iostream>
#include <vector>

using namespace std;

class Trace;
ostream& operator<< (ostream& os, const Trace& t);   // déclaré avant la classe, qui l'utilise

class Trace {
public:
   Trace()                   : data('a')  { cout << "C0(" << *this << ") "; }
   Trace(char data)          : data(data) { cout << "Ci(" << *this << ") "; }
   Trace(const Trace& other)              { this->data = other.data;
                                            cout << "CC(" << other << "->" << *this << ") "; }
   ~Trace()                               { cout << "D("  << *this << ") "; }
   Trace& operator= (const Trace& other)  { this->data = other.data;
                                            cout << "=C(" << other << "->" << *this << ") ";
                                            return *this; }
   char getData() const                   { return data; }
private:
   char data;
};

ostream& operator<< (ostream& os, const Trace& t) {
   return os << t.getData();
}

Trace f1(char i)         { cout << "f1 : "; return Trace(i); }
Trace f2(Trace t)        { cout << "f2 : "; return t;        }
Trace f3(const Trace& t) { cout << "f3 : "; return t;        }

int main() {

   // no 1
   cout << "no  1 : ";
   Trace trace1a;
   cout << endl;

   // no 2
   cout << "no  2 : ";
   Trace trace1b('a');
   cout << endl;

   // no 3
   cout << "no  3 : ";
   Trace trace1c(trace1a);
   cout << endl;

   // no 4
   cout << "no  4 : ";
   Trace trace1d(Trace('b'));
   cout << endl;

   // no 5
   cout << "no  5 : ";
   Trace trace1e(f1('c'));
   cout << endl;

   // no 6
   cout << "no  6 : ";
   Trace trace1f(f2(trace1a));
   cout << endl;

   // no 7
   cout << "no  7 : ";
   Trace trace1g(f3(trace1a));
   cout << endl;

   // no 8
   cout << "no  8 : ";
   vector<Trace> vect(3, Trace('b'));
   cout << "\t" << vect.size() << " / " << vect.capacity();
   cout << endl;

   // no 9
   cout << "no  9 : ";
   vect.push_back(trace1a);
   cout << "\t" << vect.size() << " / " << vect.capacity();
   cout << endl;

   // no 10
   cout << "no 10 : ";
   Trace trace2a('b');
   trace2a = trace1a;
   cout << endl;

   // no 11
   cout << "no 11 : ";
   trace2a = f1('c');
   cout << endl;

   // no 12
   cout << "no 12 : ";
   trace2a = f2(trace1a);
   cout << endl;

   // no 13
   cout << "no 13 : ";
   trace2a = f3(trace1a);
   cout << endl;

   // no 14
   cout << "no 14 : ";
   {
      Trace trace1b('a');
   }
   cout << endl;

   // no 15
   cout << "no 15 : ";
   {
      vector<Trace> v(3);
   }
   cout << endl;

   // no 16
   cout << "no 16 : ";
   {
      vector<Trace> v(2, Trace('a'));
   }
   cout << endl;

   // no 17
   cout << "no 17 : ";
   {
      vector<Trace> v(2, trace1a);
   }
   cout << endl;

   cout << endl;
   cout << "fin de programme" << endl;
}
~~~

~~~
no  1 : C0(a) 
no  2 : Ci(a) 
no  3 : CC(a->a) 
no  4 : Ci(b) 
no  5 : f1 : Ci(c) 
no  6 : CC(a->a) f2 : CC(a->a) D(a) 
no  7 : f3 : CC(a->a) 
no  8 : Ci(b) CC(b->b) CC(b->b) CC(b->b) D(b) 	3 / 3
no  9 : CC(a->a) CC(b->b) CC(b->b) CC(b->b) D(b) D(b) D(b) 	4 / 6
no 10 : Ci(b) =C(a->a) 
no 11 : f1 : Ci(c) =C(c->c) D(c) 
no 12 : CC(a->a) f2 : CC(a->a) =C(a->a) D(a) D(a) 
no 13 : f3 : CC(a->a) =C(a->a) D(a) 
no 14 : Ci(a) D(a) 
no 15 : C0(a) C0(a) C0(a) D(a) D(a) D(a) 
no 16 : Ci(a) CC(a->a) CC(a->a) D(a) D(a) D(a) 
no 17 : CC(a->a) CC(a->a) D(a) D(a) 

fin de programme
D(a) D(a) D(b) D(b) D(b) D(a) D(a) D(c) D(b) D(a) D(a) D(a) 
~~~

**Note** : l'ordre de destruction des éléments d'un `vector` dépend de la bibliothèque standard.
Ce résultat est celui de clang (macOS), qui détruit les éléments du dernier au premier.
Avec g++, la dernière ligne commence par `D(a) D(b) D(b) D(b) D(a)` (éléments détruits du premier au dernier).

</details>

