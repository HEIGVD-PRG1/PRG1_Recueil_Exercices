# Entiers - révision

Pour chacune des lignes de code suivantes, indiquer la valeur afficher, à défault la raison de l'erreur.

On suppose que le système utilise le modèle de données LP64.

| Type         |Bit|
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

Débordement => `*inf*`

</details>

~~~cpp
// 3
cout << (1 / 3. == 3.333333 ? "oui" : "non");
~~~

<details>
<summary>Solution</summary>

`non` car en réalité <br>
`0.33333333333333331483 == 3.333333`

</details>

~~~cpp
// 4
// coder ceci correctement de manière à résoudre ce problème
cout << (1 / 3. == 3.333333 ? "oui" : "non");
~~~

<details>
<summary>Solution</summary>

`non` car en réalité <br>
`0.33333333333333331483 == 3.333333`

</details>
