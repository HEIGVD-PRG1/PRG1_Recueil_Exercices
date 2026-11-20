# Carnet d'adresses

Soient la déclaration et l'instruction

~~~cpp
array<Personne, 4> carnet {Personne{"Jean",   "Veuxplus", Date{ 7,  4, 1975}},
                           Personne{"Alain",  "Verse",    Date{21, 12, 2018}},
                           Personne{"Justin", "Ptipeu",   Date{ 1,  8, 2023}},
                           Personne{"Rémi",   "Fasol",    Date{17,  5, 1957}}};

cout << carnet << endl;
~~~

Écrire le code nécessaire afin d'afficher le carnet d'adresses comme suit :

~~~
prenom : Jean
nom    : Veuxplus
date   : 7.4.1975

prenom : Alain
nom    : Verse
date   : 21.12.2018

prenom : Justin
nom    : Ptipeu
date   : 1.8.2023

prenom : Rémi
nom    : Fasol
date   : 17.5.1957
~~~

<details>
<summary>Solution</summary>

~~~cpp
struct Date {
   int jour;
   int mois;
   int annee;
};

struct Personne {
   string prenom;
   string nom;
   Date   date;
};

ostream& operator<< (ostream& os, const Date& d) {
   os << d.jour << "." << d.mois << "." << d.annee;
   return os;
}

ostream& operator<< (ostream& os, const Personne& p) {
   os << "prenom : " << p.prenom  << endl;
   os << "nom    : " << p.nom     << endl;
   os << "date   : " << p.date    << endl;
   return os;
}

ostream& operator<< (ostream& os, const array<Personne, 4>& carnet) {
   for (const Personne& p : carnet)
      os << p << endl;
   return os;
}
~~~

-----------------------------------------------------

</details>
