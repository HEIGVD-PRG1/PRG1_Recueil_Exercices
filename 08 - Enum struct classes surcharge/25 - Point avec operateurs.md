# Point avec opérateurs
Reprenez la classe Point de l'exercice [03-01](19%20-%20Point%20avec%20constructeurs.md) et faites les modifications / ajouts / suppressions nécessaires afin que le code ci-dessous s'exécute correctement et affiche le résultat ci-après.

~~~cpp
#include <iostream>

using namespace std;

int main() {

   Point p1(1.2, 2.4);
   Point p2(3., 4.2);

   cout << "p1" << p1 << ", p2" << p2 << endl;

   cout << "p1 + p2 = " << p1 + p2 << endl;
   cout << "p2 + p1 = " << p2 + p1 << endl;

   cout << "p1 * 2. = " << p1 * 2. << endl;
   cout << "3. * p1 = " << 3. * p1 << endl;

   cout << (p1 == p2 ? "p1 == p2" : "p1 != p2") << endl;
   Point p3(p1);
   cout << (p1 == p3 ? "p1 == p3" : "p1 != p3") << endl;
}
~~~

~~~text
p1(1.2,2.4), p2(3,4.2)
p1 + p2 = (4.2,6.6)
p2 + p1 = (4.2,6.6)
p1 * 2. = (2.4,4.8)
3. * p1 = (3.6,7.2)
p1 != p2
p1 == p3
~~~

<details>
<summary>Solution</summary>

~~~cpp
#include <iostream>

using namespace std;

class Point {
public:
   Point();
   Point(double x, double y);

   double getX() const;
   double getY() const;

private:
   double x, y;
};

Point::Point() : Point(0., 0.) {}

Point::Point(double x, double y) : x(x), y(y) {}

double Point::getX() const {
   return x;
}

double Point::getY() const {
   return y;
}

// Opérateurs externes : ils n'accèdent qu'à l'interface publique (getters)

ostream& operator<< (ostream& out, const Point& pt) {
   return out << "(" << pt.getX() << "," << pt.getY() << ")";
}

Point operator+ (const Point& pt1, const Point& pt2) {
   return Point(pt1.getX() + pt2.getX(), pt1.getY() + pt2.getY());
}

Point operator* (const Point& pt, double d) {
   return Point(pt.getX() * d, pt.getY() * d);
}

Point operator* (double d, const Point& pt) {
   return pt * d;
}

bool operator== (const Point& lhs, const Point& rhs) {
   return lhs.getX() == rhs.getX() and lhs.getY() == rhs.getY();
}

int main() {

   Point p1(1.2, 2.4);
   Point p2(3., 4.2);

   cout << "p1" << p1 << ", p2" << p2 << endl;

   cout << "p1 + p2 = " << p1 + p2 << endl;
   cout << "p2 + p1 = " << p2 + p1 << endl;

   cout << "p1 * 2. = " << p1 * 2. << endl;
   cout << "3. * p1 = " << 3. * p1 << endl;

   cout << (p1 == p2 ? "p1 == p2" : "p1 != p2") << endl;
   Point p3(p1);
   cout << (p1 == p3 ? "p1 == p3" : "p1 != p3") << endl;
}
~~~

</details>
