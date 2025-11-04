# Réels - révision

Pour chacune des lignes de code suivantes, indiquer la valeur afficher, à défault la raison de l'erreur.

On suppose que le système utilise le modèle de données LP64.

| Type         | Bit |
|--------------|:---:|
| `float`      |  32 |
| `double`     |  64 |
| `long double`|  64 |

~~~cpp
// 1
cout << round(floor(-9.8) / ceil(-4.9));
~~~

<details>
<summary>Solution</summary>

`3`

</details>

~~~cpp
// 2
double x = numeric_limits<double>::max();
cout << 2 * x / x;
~~~

<details>
<summary>Solution</summary>

Priorité des opérateurs de gauche à droite<br>
`2 * x` => débordement => `*inf*`

</details>

~~~cpp
// 3
if (1 / 3. == 0.3333333333333333)
   cout << "egalité" << endl;
else
   cout << "pas d'egalité" << endl;
~~~

<details>
<summary>Solution</summary>

`pas d'egalité` car en réalité <br>
`0.33333333333333331483 == 0.3333333333333333`

</details>

~~~cpp
// 4
// coder ceci correctement de manière à résoudre ce problème correctement pour des double

if ( /* votre réponse */ )
   cout << "egalité" << endl;
else
   cout << "pas d'egalité" << endl;
~~~

<details>
<summary>Solution</summary>

~~~cpp
if ( fabs(1 / 3. - 0.3333333333333333) <= numeric_limits<double>::epsilon())
   cout << "egalité" << endl;
else
   cout << "pas d'egalité" << endl;
~~~

</details>

~~~cpp
// 5
if ( double(1 / 3.) == float(1 / 3.))
   cout << "egalité" << endl;
else
   cout << "pas d'egalité" << endl;
~~~

<details>
<summary>Solution</summary>

`pas d'egalité`

~~~cpp
cout << setprecision(20) << double(1 / 3.) << endl; // 0.33333333333333331483
cout << setprecision(20) << float (1 / 3.) << endl; // 0.3333333432674407959
~~~

</details>

~~~cpp
// 5
float reel = 3.14159e42;
cout << reel << endl;
~~~

<details>
<summary>Solution</summary>

`inf`

~~~cpp
cout << numeric_limits<float>::max()   << endl; // 3.40282e+38
cout << numeric_limits<float>::min()   << endl; // 1.17549e-38
cout << numeric_limits<double>::max()  << endl; // 1.79769e+308
cout << numeric_limits<double>::min()  << endl; // 2.22507e-308
~~~

</details>

~~~cpp
// 6
float reel = 1e7 + 1.01;
cout << fixed << reel << endl;
~~~

<details>
<summary>Solution</summary>

`10000001.000000`

le `+ 1.01` n'est pas dans les chiffres significatifs

~~~cpp
cout << setprecision(10) << fixed << reel << endl; // 10000001.0000000000
~~~

</details>
