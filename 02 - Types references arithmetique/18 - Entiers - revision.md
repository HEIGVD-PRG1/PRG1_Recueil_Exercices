# Entiers - révision

Pour chacune des lignes de code suivantes, indiquer la valeur afficher, à défault la raison de l'erreur.

On suppose que le système utilise le modèle de données LP64.

| Type        | Bit |
|-------------|----:|
| `char`      |   8 |
| `short`     |  16 |
| `int`       |  32 |
| `long`      |  64 |
| `long long` |  64 |
| `void*`     |  64 |

~~~cpp
cout << numeric_limits<short>::max()          << endl; // 32767
cout << numeric_limits<unsigned short>::max() << endl; // 65535
cout << numeric_limits<unsigned int>::max()   << endl; // 4294967295
cout << numeric_limits<long>::max()           << endl; // 9223372036854775807
~~~

~~~cpp
// 1
signed short sh = numeric_limits<short>::max();
cout << sh;
~~~

<details>
<summary>Solution</summary>

`32767`

</details>

~~~cpp
// 2
unsigned short sh = numeric_limits<short>::max();
cout << sh;
~~~

<details>
<summary>Solution</summary>

`32767` : la valeur `numeric_limits<short>::max()` est représentable en `unsigned short`, elle est conservée telle quelle

</details>

~~~cpp
// 3
unsigned short sh = numeric_limits<unsigned short>::max();
cout << sh;
~~~

<details>
<summary>Solution</summary>

`2^16 - 1 => 65535`

</details>

~~~cpp
// 4
unsigned short sh = numeric_limits<unsigned short>::max() + 1;
cout << sh;
~~~

<details>
<summary>Solution</summary>

`65535 + 1 = 65536` mais affecté à un `short` => modulo 2^16 => `0`

</details>

~~~cpp
// 5
unsigned short sh = numeric_limits<unsigned short>::max();
cout << sh + 1;
~~~

<details>
<summary>Solution</summary>

`65535 + 1 = 65536 `

⚠️ Ceci étant une expression, le calcul se fait en `int` avec promotion `sh`

</details>

~~~cpp
// 6
unsigned short sh = -1;
cout << sh;
~~~

<details>
<summary>Solution</summary>

`65535`

</details>

~~~cpp
// 7
cout << "Wallis = " << 2/1 * 2/3 * 4/3 * 4/5 << endl;
~~~

<details>
<summary>Solution</summary>

`Résultat : 0`

⚠️ divisions entières

</details>

~~~cpp
// 8

// vérifier s'il y a débordement pour a + b
int a, b;
bool debordement = /* votre réponse ici */;
cout << "debordement : " << boolalpha << debordement;
~~~

<details>
<summary>Solution</summary>

~~~cpp
// même test que la slide « Prévenir un dépassement » : selon le signe de b,
// on compare a à la marge restante vers max() ou vers lowest()
bool debordement =
     (b >= 0 and a > numeric_limits<int>::max() - b) or
     (b <  0 and a < numeric_limits<int>::lowest() - b);
cout << "debordement : " << boolalpha << debordement;
~~~

</details>





