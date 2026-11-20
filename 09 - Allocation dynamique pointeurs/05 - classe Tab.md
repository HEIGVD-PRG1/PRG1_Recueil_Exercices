## Tableau dynamique

Soient le `main` proposé et le résultat attendu.

Implémenter la classe `Tab`, un tableau dynamique d'`int`, qui contient les propriétés privées suivantes

| Propriété      | Détails                                                 |
|:---------------|:--------------------------------------------------------|
| `int* _data`   | Un pointeur sur une zone mémoire réservée dynamiquement |
| `size_t _size` | La taille de la zone mémoire réservée dynamiquement     |

~~~cpp
int main() {

   const size_t n = 3;

   try {
      cout << "------------" << endl;
      cout << "   tab1     " << endl;
      cout << "------------" << endl;
      Tab tab1(n);
      for (size_t i = 0; i < n; ++i) {
         tab1.at(i) = static_cast<int>(i);
      }
      cout << tab1 << endl;
      cout << "size : " << tab1.size() << endl;
      cout << endl;

      cout << "------------" << endl;
      cout << "   tab2     " << endl;
      cout << "------------" << endl;
      Tab tab2(n);
      tab2 = tab1;
      cout << tab2 << endl;
      cout << "size : " << tab2.size() << endl;
      cout << endl;

      cout << "------------" << endl;
      cout << "   tab3     " << endl;
      cout << "------------" << endl;
      const Tab tab3(tab1);
      cout << tab3 << endl;
      cout << "size : " << tab3.size() << endl;
      cout << endl;

      cout << "------------" << endl;
      cout << "   [] / at  " << endl;
      cout << "------------" << endl;
      cout << "tab1[0]     = 1; " << endl;
      tab1[0] = 1;
      cout << "tab1[0]     : " << tab1[0] << endl;
      cout << endl;
      cout << "tab1.at(2)  = 2; " << endl;
      tab1.at(2) = 2;
      cout << "tab1.at(2)  : " << tab1.at(2) << endl;

      try {
         cout << "tab1[3]     : " << tab1[3] << endl;
         cout << "tab3.at(3)  : " << tab3.at(3) << endl;
      }
      catch (const out_of_range&) {
         cout << "exception : indice hors limites" << endl;
      }

      cout << endl;

   }
   catch (const bad_alloc&) {
      cout << "erreur : mémoire insuffisante" << endl;
   }

   cout << endl;
   cout << "fin de programme" << endl;
   return EXIT_SUCCESS;
}
~~~

~~~
------------
   tab1     
------------
[0, 1, 2]
size : 3

------------
   tab2     
------------
[0, 1, 2]
size : 3

------------
   tab3     
------------
[0, 1, 2]
size : 3

------------
   [] / at  
------------
tab1[0]     = 1; 
tab1[0]     : 1

tab1.at(2)  = 2; 
tab1.at(2)  : 2
tab1[3]     : 0
tab3.at(3)  : exception : indice hors limites


fin de programme
~~~

**NB** `tab1[3]` lit en dehors du tableau : `operator[]` ne vérifie pas l'indice
(contrairement à `at`). La valeur affichée est donc quelconque (ici `0`) : c'est un comportement indéfini.

<details>
<summary>Solution</summary>

~~~cpp
#include <iostream>
#include <cstdlib>
#include <stdexcept>
#include <new>

using namespace std;

//----------------------------------------------------------
class Tab {
public:
   Tab(size_t n);
   ~Tab();

   Tab(const Tab& other);
   Tab& operator= (const Tab& other);

   int& operator[] (size_t pos);
   const int& operator[] (size_t pos) const;

   int& at(size_t pos);
   const int& at(size_t pos) const;

   size_t size() const { return _size; }

private:
   size_t _size;
   int*   _data;
};

//----------------------------------------------------------
// opérateur externe : n'utilise que l'interface publique (size et [])
ostream& operator<< (ostream& os, const Tab& tab);

//----------------------------------------------------------
int main() {

   const size_t n = 3;

   try {
      cout << "------------" << endl;
      cout << "   tab1     " << endl;
      cout << "------------" << endl;
      Tab tab1(n);
      for (size_t i = 0; i < n; ++i) {
         tab1.at(i) = static_cast<int>(i);
      }
      cout << tab1 << endl;
      cout << "size : " << tab1.size() << endl;
      cout << endl;

      cout << "------------" << endl;
      cout << "   tab2     " << endl;
      cout << "------------" << endl;
      Tab tab2(n);
      tab2 = tab1;
      cout << tab2 << endl;
      cout << "size : " << tab2.size() << endl;
      cout << endl;

      cout << "------------" << endl;
      cout << "   tab3     " << endl;
      cout << "------------" << endl;
      const Tab tab3(tab1);
      cout << tab3 << endl;
      cout << "size : " << tab3.size() << endl;
      cout << endl;

      cout << "------------" << endl;
      cout << "   [] / at  " << endl;
      cout << "------------" << endl;
      cout << "tab1[0]     = 1; " << endl;
      tab1[0] = 1;
      cout << "tab1[0]     : " << tab1[0] << endl;
      cout << endl;
      cout << "tab1.at(2)  = 2; " << endl;
      tab1.at(2) = 2;
      cout << "tab1.at(2)  : " << tab1.at(2) << endl;

      try {
         cout << "tab1[3]     : " << tab1[3] << endl;
         cout << "tab3.at(3)  : " << tab3.at(3) << endl;
      }
      catch (const out_of_range&) {
         cout << "exception : indice hors limites" << endl;
      }

      cout << endl;

   }
   catch (const bad_alloc&) {
      cout << "erreur : mémoire insuffisante" << endl;
   }

   cout << endl;
   cout << "fin de programme" << endl;
   return EXIT_SUCCESS;
}

//----------------------------------------------------------
ostream& operator<< (ostream& os, const Tab& tab) {
   os << "[";
   for (size_t i = 0; i < tab.size(); ++i) {
      if (i != 0) os << ", ";
      os << tab[i];
   }
   os << "]";
   return os;
}

//----------------------------------------------------------
//    class Tab
//----------------------------------------------------------
Tab::Tab(size_t n) : _size(n), _data(new int[n]) {
}

//----------------------------------------------------------
Tab::Tab(const Tab& other) : Tab(other._size) {
   for (size_t i = 0; i < _size; ++i) {
      _data[i] = other._data[i];
   }
}

//----------------------------------------------------------
Tab::~Tab() {
   delete[] _data;
}

//----------------------------------------------------------
Tab& Tab::operator= (const Tab& other) {
   if (_size != other._size) {
      // la taille change : on copie other dans tmp, puis on échange
      // à la main le contenu de *this et de tmp ("copy and swap")
      Tab tmp(other);

      int* data_tmp = _data;
      _data         = tmp._data;
      tmp._data     = data_tmp;

      size_t size_tmp = _size;
      _size           = tmp._size;
      tmp._size       = size_tmp;

      // en fin de bloc, tmp est détruit et libère l'ancienne zone mémoire de *this
   } else if (_data != other._data) {
      // la taille ne change pas (et ce n'est pas le même objet) :
      // il suffit de copier les données
      for (size_t i = 0; i < _size; ++i) {
         _data[i] = other._data[i];
      }
   }
   return *this;
}

//----------------------------------------------------------
int& Tab::operator[] (size_t pos) {
   return _data[pos];
}

//----------------------------------------------------------
const int& Tab::operator[] (size_t pos) const {
   return _data[pos];
}

//----------------------------------------------------------
int& Tab::at(size_t pos) {
   if (pos >= _size)
      throw out_of_range("Tab::at(size_t pos)");
   return _data[pos];
}

//----------------------------------------------------------
const int& Tab::at(size_t pos) const {
   if (pos >= _size)
      throw out_of_range("Tab::at(size_t pos) const");
   return _data[pos];
}
~~~

</details>
