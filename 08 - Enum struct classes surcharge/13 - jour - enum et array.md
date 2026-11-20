# Jour de la semaine

Soient les deux déclarations

~~~cpp
enum class            Day { ERROR,   LUNDI,   MARDI,   MERCREDI,   JEUDI,   VENDREDI,   SAMEDI,   DIMANCHE};
const array<string, 8> DAY {"ERROR", "LUNDI", "MARDI", "MERCREDI", "JEUDI", "VENDREDI", "SAMEDI", "DIMANCHE"};
~~~  

Écrire les deux fonctions

- **string_to_day** : reçoit un jour en *string* et retourne l'équivalent en *enum*
- **day_to_string** : reçoit un jour en *enum* et retourne l'équivalent en *string*

Le cas échéant, retourne *enum ERROR* ou *string "ERROR"* selon la fonction.

**NB** ne pas utiliser de *switch* ni le *find* de la librairie *algorithm*.

<details>
<summary>Solution</summary>

~~~cpp
string day_to_string (Day d) {
   if (d >= Day::LUNDI and d <= Day::DIMANCHE)
      return DAY[static_cast<size_t>(d)];

   return DAY[static_cast<size_t>(Day::ERROR)];
}
~~~

~~~cpp
Day string_to_day (const string& s) {
   for (size_t i=1; i<DAY.size(); ++i)
      if (DAY[i] == s)
         return static_cast<Day>(i);
   return Day::ERROR;
}
~~~
</details>

