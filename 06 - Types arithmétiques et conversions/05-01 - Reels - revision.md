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

`*inf*`

</details>
