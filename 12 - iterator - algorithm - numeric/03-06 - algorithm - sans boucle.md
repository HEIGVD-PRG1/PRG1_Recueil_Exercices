## Vecteurs et Matrices (challenge)

### Objectif
- Utilisation de tableaux `vector<T>`
- Utilisation des `vector<T>::iterator`
- Utilisation intense de `algorithm` et `numeric` 🤔
- En conséquence, **aucune boucle explicite (`for`, `while`, `do .. while`) dans votre rendu !** 👍


### A faire
Développer une librairie générique mettant à disposition à minima les fonctionnalités listées

| Vocabulaire | Définition                                   | 
|-------------|----------------------------------------------| 
| vector      | tableau à 1 dimension `vector<T>`            |
| matrix      | tableau à 2 dimensions `vector< vector<T> >` | 

| Fonction      | Détail                                                                                                           |Notes | 
|:--------------|:-----------------------------------------------------------------------------------------------------------------|:----:| 
| operator<<    | Affiche un vecteur au format $[e_1, e_2, ..., e_n]$                                                              |  \*1 | 
| operator<<    | Affiche une matrice au format $[[v_1], [v_2], ..., [v_n]]$                                                       |  \*1 | 
| isSquare      | Retourne un booléen indiquant si la matrice est carrée                                                           |  \*2 | 
| isRegular     | Retourne un booléen indiquant si la matrice est régulière (taille des lignes identique)                          |  \*2 | 
| lineSizeMin   | Retourne la longueur du plus petit vecteur d’une matrice                                                         |      | 
| lineSizeMax   | Retourne la longueur du plus grand vecteur d’une matrice                                                         |      | 
| vectSumLine   | Retourne un vecteur contenant la somme des valeurs de chacune des lignes                                         |      | 
| vectSumColumn | Retourne un vecteur contenant la somme des valeurs de chacune des colonnes                                       |  \*3 | 
| vectSumMin    | Retourne le vecteur d’une matrice dont la somme des valeurs est la plus faible                                   |      | 
| shuffleMatrix | Mélange les vecteurs d’une matrice sans altérer les vecteurs.<br>La `seed` du générateur est basée sur l’heure   |  \*4 | 
| sortMatrix    | Trie dans l’ordre choisi une matrice en fonction de l’élément `min` d’un vecteur                                 |      | 

|  No | Notes                                                                         |
|:---:|:------------------------------------------------------------------------------|
| \*1 | En utilisant des itérateurs et sans utiliser « auto »                         |
| \*2 | Une matrice vide est réputée être carrée et régulière                         |
| \*3 | Les éléments absents (*matrice irrégulière*) sont ignorés => `T()`            |
| \*4 | [shuffle](http://www.cplusplus.com/reference/algorithm/shuffle/?kw=shuffle)   |

<br>

<details>
<summary> Ce programme <code>main.cpp</code> produit le résultat ci-après </summary>

~~~cpp
#include <cstdlib>
#include <iostream>
#include "matrix.h"

using namespace std;

//------------------------------------------------------------------
template <typename T>
void test(Matrix<T> matrix);

//------------------------------------------------------------------
int main(){

   using Matrix = Matrix<int>;

   //---------------------------------------------------------------
   // empty
   Matrix vide;                       // 0x0

   // regular
   Matrix reg1   = {{1}};             // 1x1
   Matrix reg2   = {{4, 3},           // 2x2
                     {1, 2}};
   Matrix reg3   = {{5, 2, 8},        // 3x3
                     {4, 3, 9},
                     {1, 6, 7}};

   // rectangular and regular
   Matrix reg10  = {{}};              // 1x0
   Matrix reg11  = {{},               // 2x0
                     {}};
   Matrix reg12  = {{1},              // 2x1
                     {2}};
   Matrix reg13  = {{1, 2, 3},        // 2x3
                     {4, 5, 6}};
   Matrix reg14  = {{1, 2},           // 3x2
                     {3, 4},
                     {5, 6}};

   // irregular
   Matrix ireg01 = {{1},
                     {}};
   Matrix ireg02 = {{},
                     {1}};
   Matrix ireg03 = {{},
                     {1, 2}};
   Matrix ireg04 = {{1, 2},
                    {}};
   Matrix ireg05 = {{},
                     {4, 3, 9},
                     {1, 6, 7}};
   Matrix ireg06 = {{5},
                     {4, 3, 9},
                     {1, 6, 7}};
   Matrix ireg07 = {{5, 2},
                     {4, 3, 9},
                     {1, 6, 7}};
   Matrix ireg08 = {{5, 2, 8},
                     {4, 3},
                     {1, 6, 7}};
   Matrix ireg09 = {{5, 2, 8},
                     {4},
                     {1, 6, 7}};
   Matrix ireg10 = {{5, 2, 8},
                     {},
                     {1, 6, 7}};
   Matrix ireg11 = {{5, 2, 8},
                     {4, 3, 9},
                     {1, 6}};
   Matrix ireg12 = {{5, 2, 8},
                     {4, 3, 9},
                     {1}};
   Matrix ireg13 = {{5, 2, 8},
                     {4, 3, 9},
                     {}};

   //---------------------------------------------------------------
   // test program
   //---------------------------------------------------------------
   vector<Matrix> matrix = {
      vide,   reg1,   reg2,   reg3,   reg10,  reg11,  reg12,  reg13,  reg14,
      ireg01, ireg02, ireg03, ireg04, ireg05, ireg06, ireg07, ireg08, ireg09, ireg10,
      ireg11, ireg12, ireg13};

   for_each(matrix.begin(), matrix.end(), test<int>);

   cout << endl;

   return EXIT_SUCCESS;
}

//------------------------------------------------------------------
template <typename T>
void test(Matrix<T> m){

   cout << "-------------------------------------------------------" << endl;
   cout << "matrix         : " << m                                  << endl;
   cout << "isSquare       : " << (isSquare<int>(m)  ? "yes" : "no") << endl;
   cout << "isRegular      : " << (isRegular<int>(m) ? "yes" : "no") << endl;
   cout << "totalLine      : " << vectSumLine<int>(m)                << endl;
   cout << "totalColumn    : " << vectSumColumn<int>(m)              << endl;
   cout << "vector sum min : " << vectorSumMin<int>(m)               << endl;

   shuffleMatrix(m);
   cout << "shuffle matrix : " << m << endl;

   sortMatrix<int>(m, SortOrder::UP);
   cout << "sort matrix    : " << m << endl;
}
~~~

</details>

<details>
<summary> <code>output</code> produit par le main </summary>

~~~
-------------------------------------------------------
matrix         : []
isSquare       : yes
isRegular      : yes
totalLine      : []
totalColumn    : []
vector sum min : []
shuffle matrix : []
sort matrix    : []
-------------------------------------------------------
matrix         : [[1]]
isSquare       : yes
isRegular      : yes
totalLine      : [1]
totalColumn    : [1]
vector sum min : [1]
shuffle matrix : [[1]]
sort matrix    : [[1]]
-------------------------------------------------------
matrix         : [[4, 3], [1, 2]]
isSquare       : yes
isRegular      : yes
totalLine      : [7, 3]
totalColumn    : [5, 5]
vector sum min : [1, 2]
shuffle matrix : [[1, 2], [4, 3]]
sort matrix    : [[1, 2], [4, 3]]
-------------------------------------------------------
matrix         : [[5, 2, 8], [4, 3, 9], [1, 6, 7]]
isSquare       : yes
isRegular      : yes
totalLine      : [15, 16, 14]
totalColumn    : [10, 11, 24]
vector sum min : [1, 6, 7]
shuffle matrix : [[4, 3, 9], [5, 2, 8], [1, 6, 7]]
sort matrix    : [[1, 6, 7], [5, 2, 8], [4, 3, 9]]
-------------------------------------------------------
matrix         : [[]]
isSquare       : no
isRegular      : yes
totalLine      : [0]
totalColumn    : []
vector sum min : []
shuffle matrix : [[]]
sort matrix    : [[]]
-------------------------------------------------------
matrix         : [[], []]
isSquare       : no
isRegular      : yes
totalLine      : [0, 0]
totalColumn    : []
vector sum min : []
shuffle matrix : [[], []]
sort matrix    : [[], []]
-------------------------------------------------------
matrix         : [[1], [2]]
isSquare       : no
isRegular      : yes
totalLine      : [1, 2]
totalColumn    : [3]
vector sum min : [1]
shuffle matrix : [[2], [1]]
sort matrix    : [[1], [2]]
-------------------------------------------------------
matrix         : [[1, 2, 3], [4, 5, 6]]
isSquare       : no
isRegular      : yes
totalLine      : [6, 15]
totalColumn    : [5, 7, 9]
vector sum min : [1, 2, 3]
shuffle matrix : [[4, 5, 6], [1, 2, 3]]
sort matrix    : [[1, 2, 3], [4, 5, 6]]
-------------------------------------------------------
matrix         : [[1, 2], [3, 4], [5, 6]]
isSquare       : no
isRegular      : yes
totalLine      : [3, 7, 11]
totalColumn    : [9, 12]
vector sum min : [1, 2]
shuffle matrix : [[3, 4], [5, 6], [1, 2]]
sort matrix    : [[1, 2], [3, 4], [5, 6]]
-------------------------------------------------------
matrix         : [[1], []]
isSquare       : no
isRegular      : no
totalLine      : [1, 0]
totalColumn    : [1]
vector sum min : []
shuffle matrix : [[], [1]]
sort matrix    : [[1], []]
-------------------------------------------------------
matrix         : [[], [1]]
isSquare       : no
isRegular      : no
totalLine      : [0, 1]
totalColumn    : [1]
vector sum min : []
shuffle matrix : [[], [1]]
sort matrix    : [[1], []]
-------------------------------------------------------
matrix         : [[], [1, 2]]
isSquare       : no
isRegular      : no
totalLine      : [0, 3]
totalColumn    : [1, 2]
vector sum min : []
shuffle matrix : [[], [1, 2]]
sort matrix    : [[1, 2], []]
-------------------------------------------------------
matrix         : [[1, 2], []]
isSquare       : no
isRegular      : no
totalLine      : [3, 0]
totalColumn    : [1, 2]
vector sum min : []
shuffle matrix : [[1, 2], []]
sort matrix    : [[], [1, 2]]
-------------------------------------------------------
matrix         : [[], [4, 3, 9], [1, 6, 7]]
isSquare       : no
isRegular      : no
totalLine      : [0, 16, 14]
totalColumn    : [5, 9, 16]
vector sum min : []
shuffle matrix : [[1, 6, 7], [4, 3, 9], []]
sort matrix    : [[], [1, 6, 7], [4, 3, 9]]
-------------------------------------------------------
matrix         : [[5], [4, 3, 9], [1, 6, 7]]
isSquare       : no
isRegular      : no
totalLine      : [5, 16, 14]
totalColumn    : [10, 9, 16]
vector sum min : [5]
shuffle matrix : [[4, 3, 9], [1, 6, 7], [5]]
sort matrix    : [[5], [1, 6, 7], [4, 3, 9]]
-------------------------------------------------------
matrix         : [[5, 2], [4, 3, 9], [1, 6, 7]]
isSquare       : no
isRegular      : no
totalLine      : [7, 16, 14]
totalColumn    : [10, 11, 16]
vector sum min : [5, 2]
shuffle matrix : [[1, 6, 7], [5, 2], [4, 3, 9]]
sort matrix    : [[5, 2], [1, 6, 7], [4, 3, 9]]
-------------------------------------------------------
matrix         : [[5, 2, 8], [4, 3], [1, 6, 7]]
isSquare       : no
isRegular      : no
totalLine      : [15, 7, 14]
totalColumn    : [10, 11, 15]
vector sum min : [4, 3]
shuffle matrix : [[5, 2, 8], [4, 3], [1, 6, 7]]
sort matrix    : [[4, 3], [1, 6, 7], [5, 2, 8]]
-------------------------------------------------------
matrix         : [[5, 2, 8], [4], [1, 6, 7]]
isSquare       : no
isRegular      : no
totalLine      : [15, 4, 14]
totalColumn    : [10, 8, 15]
vector sum min : [4]
shuffle matrix : [[5, 2, 8], [4], [1, 6, 7]]
sort matrix    : [[4], [1, 6, 7], [5, 2, 8]]
-------------------------------------------------------
matrix         : [[5, 2, 8], [], [1, 6, 7]]
isSquare       : no
isRegular      : no
totalLine      : [15, 0, 14]
totalColumn    : [6, 8, 15]
vector sum min : []
shuffle matrix : [[1, 6, 7], [], [5, 2, 8]]
sort matrix    : [[5, 2, 8], [], [1, 6, 7]]
-------------------------------------------------------
matrix         : [[5, 2, 8], [4, 3, 9], [1, 6]]
isSquare       : no
isRegular      : no
totalLine      : [15, 16, 7]
totalColumn    : [10, 11, 17]
vector sum min : [1, 6]
shuffle matrix : [[5, 2, 8], [1, 6], [4, 3, 9]]
sort matrix    : [[1, 6], [5, 2, 8], [4, 3, 9]]
-------------------------------------------------------
matrix         : [[5, 2, 8], [4, 3, 9], [1]]
isSquare       : no
isRegular      : no
totalLine      : [15, 16, 1]
totalColumn    : [10, 5, 17]
vector sum min : [1]
shuffle matrix : [[4, 3, 9], [5, 2, 8], [1]]
sort matrix    : [[1], [5, 2, 8], [4, 3, 9]]
-------------------------------------------------------
matrix         : [[5, 2, 8], [4, 3, 9], []]
isSquare       : no
isRegular      : no
totalLine      : [15, 16, 0]
totalColumn    : [9, 5, 17]
vector sum min : []
shuffle matrix : [[4, 3, 9], [5, 2, 8], []]
sort matrix    : [[], [5, 2, 8], [4, 3, 9]]
~~~

</details>

<details>
<summary> <code>solution</code> produit </summary>

~~~cpp
#include <ostream>
#include <vector>
#include <algorithm>
#include <numeric>
#include <functional>   // std::plus
#include <chrono>       // std::chrono::system_clock
#include <random>       // std::default_random_engine

enum class SortOrder {DOWN = 0, UP};

//------------------------------------------------------------------
template <typename T> using Vector = std::vector<T>;
template <typename T> using Matrix = std::vector< Vector<T> >;

//------------------------------------------------------------------
template <typename T>
std::ostream& operator<< (std::ostream& os, const std::vector<T>& v) {
   os << '[';
   std::for_each(v.cbegin(), v.cend(), [&os] (const T& e) { os << e << ", "; } );
   if (not v.empty())
      os << "\b\b";
   return os << "]";
}

//------------------------------------------------------------------
template <typename T>
size_t lineSizeMin(const Matrix<T>& m) {
   if (m.empty())
      return 0;

   return min_element(m.cbegin(),
                      m.cend(),
                      [] (const std::vector<T>& lhs, const std::vector<T>& rhs) {
                         return lhs.size() < rhs.size();
                      })->size();
}

//------------------------------------------------------------------
template <typename T>
size_t lineSizeMax (const Matrix<T>& m) {
   if (m.empty())
      return 0;
   return max_element(m.cbegin(),
                      m.cend(),
                      [] (const std::vector<T>& lhs, const std::vector<T>& rhs) {
                         return lhs.size() < rhs.size();
                      })->size();
}

//------------------------------------------------------------------
template <typename T>
bool isSquare (const Matrix<T>& m) {
   const typename Matrix<T>::size_type SIZE_M = m.size();
   return all_of(m.begin(), m.end(),
                 [SIZE_M](const std::vector<T>& v) { return v.size() == SIZE_M; });
}

//------------------------------------------------------------------
template <typename T>
bool isRegular (const Matrix<T>& m) {
   return lineSizeMin<T>(m) == lineSizeMax<T>(m);
}

//------------------------------------------------------------------
template <typename T>
std::vector<T> vectSumLine (const Matrix<T>& m) {
   std::vector<T> result(m.size());
   transform(m.begin(), m.end(), result.begin(),
             [] (const std::vector<T>& v) { return accumulate(v.begin(), v.end(), T()); });
   return result;
}

//------------------------------------------------------------------
template <typename T>
std::vector<T> addColumn (const std::vector<T>& lhs, const std::vector<T>& rhs) {
   std::vector<T> result (lhs);
   transform(rhs.cbegin(), rhs.cend(), lhs.cbegin(), result.begin(), std::plus<T>());
   return result;
}

   //------------------------------------------------------------------
template <typename T>
std::vector<T> vectSumColumn(const Matrix<T>& m) {
   if (m.empty())
      return std::vector<T>();
   return accumulate(m.begin(), m.end(), std::vector<T>(lineSizeMax<T>(m)), addColumn<T>);
}

//------------------------------------------------------------------
template <typename T>
bool sumMin (const std::vector<T>& L, const std::vector<T>& R) {
   return accumulate(L.cbegin(), L.cend(), T()) < accumulate(R.cbegin(), R.cend(), T());
}

//------------------------------------------------------------------
template <typename T>
std::vector<T> vectorSumMin (const Matrix<T>& m) {
   if (m.empty())
      return std::vector<T>();
   return *min_element(m.cbegin(), m.cend(), sumMin<T>);
}

//------------------------------------------------------------------
template <typename T>
void shuffleMatrix(Matrix<T>& m) {
   if (m.empty())
      return;
   unsigned seed = (unsigned)std::chrono::system_clock::now().time_since_epoch().count();
   shuffle(m.begin(), m.end(), std::default_random_engine(seed));
}

//------------------------------------------------------------------
template <typename T>
struct compareVect {

   SortOrder order;
   bool operator() (const std::vector<T>& lhs, const std::vector<T>& rhs) {

      if (lhs.empty() or rhs.empty())
         return true;

      switch(order) {
         case SortOrder::UP :
            return *max_element(lhs.cbegin(), lhs.cend()) < *max_element(rhs.cbegin(), rhs.cend());

         case SortOrder::DOWN :
            return *max_element(lhs.cbegin(), lhs.cend()) > *max_element(rhs.cbegin(), rhs.cend());
      }

      return false;
   }
};

//------------------------------------------------------------------
template <typename T>
void sortMatrix(Matrix<T>& m, SortOrder order = SortOrder::UP) {
   std::sort (m.begin(), m.end(), compareVect<T>{order});
}
~~~

</details>