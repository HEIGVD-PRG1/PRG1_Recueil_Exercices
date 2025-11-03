# Entiers <=> Réels - révision

Pour chacune des lignes de code suivantes, indiquer la valeur afficher, à défault la raison de l'erreur.

On suppose que le système utilise le modèle de données LP64.

| Type         | Bit | Type         | Bit |
|--------------|:---:|--------------|:---:|
| `char`       |   8 | `void*`      |  64 |
| `short`      |  16 | `float`      |  32 |
| `int`        |  32 | `double`     |  64 |
| `long`       |  64 | `long double`|  64 |
| `long long`  |  64 |

~~~cpp
// 1
cout << double(1 / 3);
~~~

<details>
<summary>Solution</summary>

`0`

⚠️ La conversion est fait après l'évaluation de l'expression.

</details>

~~~cpp
// 2
cout << (double)1 / 3;
~~~

<details>
<summary>Solution</summary>

`0.333333`

⚠️ seul le `1` est converti explicitement en `double` ce qui force le `3` à être converti implicitement.

</details>

~~~cpp
// 3
int entier = 1e42;
cout << entier << endl;
~~~

<details>
<summary>Solution</summary>

`46681208`

⚠️ `1e42` est un `double` qui est converti implicitement en `int`<br>
A cause du débordement, la valeur est différente.

</details>

~~~cpp
// 4
float reel = 1234567890;
cout << fixed << reel << endl;
~~~

<details>
<summary>Solution</summary>

`1234567936.000000`

⚠️ `entier` contient trop de chiffres significatifs pour le `float` qui n'en a que 7 ou 9 sur 32 bits.

</details>

~~~cpp
// 5
if (float(1234567890) == 1234567890)
   cout << "egalité";
else
   cout << "pas d'égalité";
~~~

<details>
<summary>Solution</summary>

`egalité`

⚠️ même problème que précédement mais pour réaliser le teste d'égalité avec l'opérateur `==`, l'opérande de droite en `int` est convertie en `float`. Ainsi, le problème ne voit pas !!

</details>

