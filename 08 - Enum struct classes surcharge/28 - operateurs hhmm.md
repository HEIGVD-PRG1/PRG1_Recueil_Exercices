# Opérateurs sur hh:mm 🕔

Soient la déclaration et le code suivants

~~~cpp
struct HeureMinute {
   int heure;
   int minute;
};

HeureMinute saisie(const string& msg) {
   HeureMinute hm;
   bool erreur;
   do {
      cout << msg;
      cin >> hm;
      erreur = cin.fail();
      cin.clear();
      cin.ignore(numeric_limits<streamsize>::max(), '\n');
   } while (erreur);
   return hm;
}

HeureMinute hm1 = {12, 34};
HeureMinute hm2 = {21, 43};

cout << hm1 << " <  " << hm2 << "  : " << (hm1 <  hm2 ? "true" : "false") << endl;
cout << hm1 << " >  " << hm2 << "  : " << (hm1 >  hm2 ? "true" : "false") << endl;
cout << hm1 << " <= " << hm2 << "  : " << (hm1 <= hm2 ? "true" : "false") << endl;
cout << hm1 << " >= " << hm2 << "  : " << (hm1 >= hm2 ? "true" : "false") << endl;
cout << hm1 << " == " << hm2 << "  : " << (hm1 == hm2 ? "true" : "false") << endl;
cout << hm1 << " != " << hm2 << "  : " << (hm1 != hm2 ? "true" : "false") << endl;

cout << hm1 << " +  " << hm2 << "  : " << (hm1 +  hm2) << endl;
cout << hm1 << " +  " << 44  << "     : " << (hm1 +  44 ) << endl;
cout << 44  << "    +  " << hm1 << "  : " << (44  +  hm1) << endl;

cout << endl;
HeureMinute hm3 = saisie("heure [hh:mm]: ");
cout << "votre saisie : " << hm3 << endl;
~~~

Écrire le code nécessaire afin que le code produise ceci.

~~~
12:34 <  21:43  : true
12:34 >  21:43  : false
12:34 <= 21:43  : true
12:34 >= 21:43  : false
12:34 == 21:43  : false
12:34 != 21:43  : true
12:34 +  21:43  : 34:17
12:34 +  44     : 13:18
44    +  12:34  : 13:18

heure [hh:mm]: 12 34
heure [hh:mm]: 12:34
votre saisie : 12:34

~~~

<details>
<summary>Solution</summary>

~~~cpp
bool operator< (const HeureMinute& lhs, const HeureMinute& rhs) {
   if (lhs.heure != rhs.heure)
      return lhs.heure < rhs.heure;
   return lhs.minute < rhs.minute;
}

bool operator> (const HeureMinute& lhs, const HeureMinute& rhs) {
   return rhs < lhs;
}

bool operator<= (const HeureMinute& lhs, const HeureMinute& rhs) {
   return !(rhs < lhs);
}

bool operator>= (const HeureMinute& lhs, const HeureMinute& rhs) {
   return !(lhs < rhs);
}

bool operator== (const HeureMinute& lhs, const HeureMinute& rhs) {
   return lhs.heure   == rhs.heure
      and lhs.minute  == rhs.minute;
}

bool operator!= (const HeureMinute& lhs, const HeureMinute& rhs) {
   return !(lhs == rhs);
}

HeureMinute operator+ (const HeureMinute& lhs, const HeureMinute& rhs) {
   return { lhs.heure + rhs.heure + (lhs.minute + rhs.minute) / 60,
            (lhs.minute + rhs.minute) % 60 };
}

HeureMinute operator+ (const HeureMinute& hm, int minute) {
   return hm + HeureMinute{0, minute};
}

HeureMinute operator+ (int minute, const HeureMinute& hm) {
   return hm + minute;
}

ostream& operator<< (ostream& os, const HeureMinute& h) {
   return os << h.heure << ":" << h.minute;
}

istream& operator>> (istream& is, HeureMinute& h) {
   char c;
   is >> h.heure;

   // is.setstate(ios::failbit) met le flux en état d'échec (états du flux, ch. 7) :
   // l'appelant le détecte avec cin.fail(), comme pour une saisie de type incorrect.
   is >> c;
   if (c != ':') {
      is.setstate(ios::failbit);
   }

   is >> h.minute;
   if (h.minute >= 60) {
      is.setstate(ios::failbit);
   }

   return is;
}
~~~

------------------------------------------------------------

</details>

🤔 Pourrions-nous *simplifier* l'algorithme des opérateurs < et ==

<details>
<summary>Solution</summary>

En profitant de la comparaison lexicographique des structures comme
*vector*, *array*, *pair*, etc ... 👍

Choisir toutefois la structure la plus économe en mémoire (complexité spatiale).

~~~cpp
bool operator< (const HeureMinute& lhs, const HeureMinute& rhs) {
   return pair<int, int>{lhs.heure, lhs.minute} < pair<int, int>{rhs.heure, rhs.minute};
}

bool operator== (const HeureMinute& lhs, const HeureMinute& rhs) {
   return pair<int, int>{lhs.heure, lhs.minute} == pair<int, int>{rhs.heure, rhs.minute};
}
~~~


</details>
