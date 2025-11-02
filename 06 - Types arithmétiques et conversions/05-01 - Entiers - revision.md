# Entiers - révision

Pour chacune des lignes de code suivantes, indiquer la valeur afficher, à défault la raison. 

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
// 1
signed short sh = numeric_limits<short>::max();
cout << sh;
~~~

<details>
<summary>Solution</summary>

~~~cpp
32767
~~~

</details>

~~~cpp
// 2
unsigned short sh = numeric_limits<short>::max();
cout << sh;
~~~

<details>
<summary>Solution</summary>

⚠️ A cause de l'affectation `umeric_limits<short>::max()` => 32767

</details>

~~~cpp
// 3
unsigned short sh = numeric_limits<unsigned short>::max();
cout << sh;
~~~

<details>
<summary>Solution</summary>

2^16 - 1 => 65535

</details>

~~~cpp
// 4
unsigned short sh = numeric_limits<unsigned short>::max() + 1;
cout << sh;
~~~

<details>
<summary>Solution</summary>

65535 + 1 = 65536 mais affecté à un `short` => modulo 2^16 => 0

</details>

~~~cpp
// 5
unsigned short sh = numeric_limits<unsigned short>::max();
cout << sh + 1;
~~~

<details>
<summary>Solution</summary>

65535 + 1

⚠️ Ceci étant une expression, le calcul se fait en `int` avec promotion `sh`

</details>

~~~cpp
// 6
unsigned short sh = -1;
cout << sh;
~~~

<details>
<summary>Solution</summary>

65535

</details>

~~~cpp
// 7
unsigned short sh = 1;
cout << -sh;
~~~

<details>
<summary>Solution</summary>

⚠️ par promotion à cause de l'opérateur unaire `-` le calcul se fait en `int` <br> et l'opérateur `<<` choisi est celui du `int` également

</details>

~~~cpp
// 8
unsigned short sh = 0;
cout << --sh;
~~~

<details>
<summary>Solution</summary>

⚠️ par promotion à cause de l'opérateur de pré-décrémentation `--` le calcul se fait en `int` <br> mais ce même opèrateur returne une référence sur `sh` et l'opérateur choisi `<<` est celui du `unsigned short` également

</details>

~~~cpp
// 9
cout << numeric_limits<long>::max()          << endl; // 9223372036854775807
cout << numeric_limits<unsigned int>::max()  << endl; // 4294967295

long     int a = 2;
unsigned int b = 1;

cout << b - a;
~~~

<details>
<summary>Solution</summary>

-1

⚠️ `a` est codé sur 64 bits et `b` sur 32 bits
Bien que `b`soit `unsigned`, sa valeur est toujours représentable sur un `long`même signé. `b` est converti en `long`

</details>

~~~cpp
// 10
Convertir à la main 0x32A en Octale
~~~

<details>
<summary>Solution</summary>

Résultat : 1452

1) convertir 0x32A en binaire
2) les grouper par 3 bits
3) interpréter ces 3 bits en valeurs octales

~~~
    Hexa      3    2    A
    Binaire 0011 0010 1010

    Binaire 001 100 101 010
    Octal    1   4   5   2
~~~

</details>

~~~cpp
// 11
cout << "Wallis = " << 2/1 * 2/3 * 4/3 * 4/5 << endl;
~~~

<details>
<summary>Solution</summary>

Résultat : 0

⚠️ divisions entières

</details>


