# Pointeurs et const

Soient les déclarations suivantes :

~~~cpp
int i = 5;
const int j = 10;

int* ptr_i = &i;
const int* ptr_j = &j;

const int* ptr_i_1 = &i;
int* const ptr_i_2 = &i;
~~~

- Indiquez si les instructions suivantes sont correctes ou pas (génère une erreur) ? 
- Expliquez votre réponse, 
- Quelle serait la valeur de i et j si l'instruction est exécutée.


| # | Instruction         | Correcte ? (oui/non) | Explication |  i  |  j  |
|---| ------------------- | -------------------- | ----------- | ---:| ---:|
| 1 | `*ptr_i = 15;`      |                      |             |     |     |
| 2 | `int *ptr_j_1 = &j;`|                      |             |     |     |
| 3 | `ptr_j = ptr_i;`    |                      |             |     |     |
| 4 | `*ptr_i_1 = 25;`    |                      |             |     |     |
| 5 | `*ptr_i_2 = 30;`    |                      |             |     |     |
| 6 | `ptr_i_1 = &j;`     |                      |             |     |     |
| 7 | `ptr_i_2 = &j;`     |                      |             |     |     |



    

<details>
<summary>Solution</summary>

| # | Instruction           | Correcte | Explication                                                                                                   |  i  |  j  |
|---| --------------------- | ---------| ------------------------------------------------------------------------------------------------------------- | ---:| ---:|
| 1 | `*ptr_i = 15;`        |   oui    | `ptr_i` pointe vers `i`, qui est modifiable                                                                   |  15 |  10 |
| 2 | `int *ptr_j_1 = &j;`  |   non    | `&j` est l'adresse d'un `const int` : un `int*` permettrait de modifier `j`, qui est constant                 |     |     |
| 3 | `ptr_j = ptr_i;`      |   oui    | `ptr_j` n'est pas un pointeur constant, il peut pointer vers `i` (sans pouvoir le modifier)                   |   5 |  10 |
| 4 | `*ptr_i_1 = 25;`      |   non    | `ptr_i_1` est un pointeur vers un `const int` : la valeur pointée ne peut pas être modifiée par ce pointeur   |     |     |
| 5 | `*ptr_i_2 = 30;`      |   oui    | `ptr_i_2` est un pointeur constant, mais la valeur pointée (`i`) est modifiable                               |  30 |  10 |
| 6 | `ptr_i_1 = &j;`       |   oui    | `ptr_i_1` n'est pas un pointeur constant : il peut pointer vers `j`, qui ne sera pas modifié                  |   5 |  10 |
| 7 | `ptr_i_2 = &j;`       |   non    | `ptr_i_2` est un pointeur constant : il ne peut pas pointer ailleurs (de plus, `&j` est un `const int*`)      |     |     |


</details>
