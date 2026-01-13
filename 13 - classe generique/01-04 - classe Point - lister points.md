# classe générique Point - lister points 

Reprendre l'exercice précédant [01-03 - classe Point - quadrant](01-03%20-%20classe%20Point%20-%20quadrant.md)

Ajouter une fonction générique `listerPoint<T>` qui liste l'ensemble des points.

~~~
p1(1, 2)
p2(4, 2)
p3(9, 8)
p4(-1, 5)
p5(3, -1)
p6(7, 0)
~~~

<details>
<summary>Solution - listerPoints</summary>

~~~cpp
template <typename T>
void listerPoints(const vector<Point<T>>& v) {
   for (const Point<T>& p : v) {
      p.afficher();
      cout << endl;
   }
}

// appel
listerPoints(dessin);		// le vecteur tout entier
~~~

Avec des itérateurs
~~~cpp
template <typename Const_Iterateur>
void listerPoints(Const_Iterateur first, Const_Iterateur last) {
   for (Const_Iterateur iter = first; iter != last; ++iter) {
      iter->afficher();  // ou (*iter).afficher();
      cout << endl;
   }
}

// appel
listerPoints(dessin.begin(), dessin.end());   // des itérateurs sur le vecteur
~~~


</details>
