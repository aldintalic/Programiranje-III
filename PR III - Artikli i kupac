#include<iostream>
using namespace std;

const char * crt = "\n----------------------------------------------------\n";
char* AlocirajNizKaraktera(const char* sadrzaj) {
	if (sadrzaj == nullptr)
		return nullptr;
	int vel = strlen(sadrzaj) + 1;
	char* temp = new char[vel];
	strcpy_s(temp, vel, sadrzaj);
	return temp;
}
template<class T1, class T2>
class Kolekcija {
	T1 * _elementi1;
	T2 * _elementi2;
	int * _trenutno;
	void Kopiraj(const Kolekcija<T1,T2>& original) {
		_trenutno = new int(*original._trenutno);
		_elementi1 = new T1[*_trenutno];
		_elementi2 = new T2[*_trenutno];

		for (int i = 0; i < *_trenutno; i++) {
			_elementi1[i] = original._elementi1[i];
			_elementi2[i] = original._elementi2[i];
		}

	}
	void Dealociraj() {
		delete[]_elementi1; _elementi1 = nullptr;
		delete[]_elementi2; _elementi2 = nullptr;
		delete _trenutno; _trenutno = nullptr;
	}
public:
	Kolekcija()
	{
		_elementi1 = nullptr;
		_elementi2 = nullptr;
		_trenutno = new int(0);
	}
	Kolekcija(const Kolekcija<T1,T2>& original){
		Kopiraj(original);
	}
	//1. potrebne funkcije	
	~Kolekcija()
	{
		Dealociraj();
	}
	void AddElement(const T1& obj1,const T2& obj2) {
		T1* temp1 = new T1[*_trenutno + 1];
		T2* temp2 = new T2[*_trenutno + 1];
		for (int i = 0; i < *_trenutno; i++) {
			temp1[i] = _elementi1[i];
			temp2[i] = _elementi2[i];
		}
		delete[] _elementi1; delete[] _elementi2;
		_elementi1 = temp1;
		_elementi2 = temp2;
		_elementi1[*_trenutno] = obj1;
		_elementi2[*_trenutno] = obj2;
		(*_trenutno)++;
	}
	T1 getElement1(int indeks) { return _elementi1[indeks]; }
	T2 getElement2(int indeks) { return _elementi2[indeks]; }
	T2* getElementi2() { return _elementi2; }
	friend ostream& operator<<<>(ostream&, Kolekcija<T1, T2>&);
	Kolekcija<T1, T2>& operator=(const Kolekcija<T1, T2>& original) {
		if (this != &original) {
			Dealociraj();
			Kopiraj(original);
		}
		return *this;
	}
	int getTrenutno() { return *_trenutno; }
};
template<class T1, class T2>
ostream& operator<<<>(ostream& ispis, Kolekcija<T1, T2>& kolekcija) {
	for (int i = 0; i < *kolekcija._trenutno; i++)
		ispis << kolekcija._elementi1[i] << "	" << kolekcija._elementi2[i] << endl;
	return ispis;
}

class Datum {
	int *_dan, *_mjesec, *_godina;
	void Kopiraj(int dan = 1, int mjesec = 1, int godina = 2000) {
		_dan = new int(dan);
		_mjesec = new int(mjesec);
		_godina = new int(godina);
	}
	void Dealociraj() {
		delete _dan; _dan = nullptr;
		delete _mjesec; _mjesec = nullptr;
		delete _godina; _godina = nullptr;
	}
public:
	Datum(int dan = 1, int mjesec = 1, int godina = 2000) {
		Kopiraj(dan, mjesec, godina);
	}
	Datum(const Datum& original){
		Kopiraj(*original._dan, *original._mjesec, *original._godina);
	}
	~Datum() {
		delete _dan; _dan = nullptr;
		delete _mjesec; _mjesec = nullptr;
		delete _godina; _godina = nullptr;
	}
	Datum& operator=(const Datum& original) {
		if (this != &original) {
			Dealociraj();
			Kopiraj(*original._dan, *original._mjesec, *original._godina);
		}
		return *this;
	}
	friend ostream& operator<< (ostream &COUT, const Datum &obj) {
		COUT << *obj._dan << " " << *obj._mjesec << " " << *obj._godina;
		return COUT;
	}
	//1. potrebne funkcije
};
class Osoba {
protected:
	char * _imePrezime;
	Datum * _datumRodjenja;
public:
	Osoba(const char * imePrezime, Datum datum) {
		_imePrezime = AlocirajNizKaraktera(imePrezime);
		_datumRodjenja = new Datum(datum);
	}
	Osoba(const Osoba & obj) {
		_imePrezime = AlocirajNizKaraktera(obj._imePrezime);
		_datumRodjenja = new Datum(*obj._datumRodjenja);
	}
	virtual ~Osoba() {
		delete[]_imePrezime; _imePrezime = nullptr;
		delete _datumRodjenja; _datumRodjenja = nullptr;
	}
	//1. potrebne funkcije
	Osoba& operator=(const Osoba& original) {
		if (this != &original) {
			delete[] _imePrezime; delete _datumRodjenja;
			_imePrezime = AlocirajNizKaraktera(original._imePrezime);
			_datumRodjenja = new Datum(*original._datumRodjenja);
		}
		return *this;
	}
	friend ostream& operator<<(ostream& ispis, const Osoba& osoba) {
		ispis << "Ime i prezime: " << osoba._imePrezime << endl;
		ispis << "Datum rodjenja: " << *osoba._datumRodjenja<< endl;
		return ispis;
	}
};

class Kupac:public Osoba {
	//svaki kupac ima popust koji mu se zaracunava prilikom svake kupovine
	//pocetni popust je 0.10%
	//svaka kupovina u iznosu 100 - 500KM povecava iznos popusta za 0.15%
	//svaka kupovina u iznosu 500 - 1000KM povecava iznos popusta za 0.25%
	//svaka kupovina u iznosu vecem od 1000KM povecava iznos popusta za 0.30%
	float _odobreniPopust;
public:
	//1. potrebne funkcije
	Kupac(const char* imePrezime, const Datum &datum):Osoba(imePrezime,datum){_odobreniPopust = 0.10;}
	virtual ~Kupac(){}
	Kupac& operator=(const Kupac& original) {
		if (this != &original) {
			Osoba::operator=(original);
			_odobreniPopust = original._odobreniPopust;
		}
		return *this;
	}
	friend ostream& operator<<(ostream& ispis,Kupac& kupac) {
		ispis << *static_cast<Osoba*>(&kupac) << endl;
		ispis << "Popust: " << kupac._odobreniPopust << endl;
		return ispis;
	}
	float GetPopust() { return _odobreniPopust; }
};
class Artikal {
	char * _barCode;
	char * _naziv;
	float _cijena;
public:
	Artikal(const char * naziv, float cijena, const char * barCode) {
		_naziv = AlocirajNizKaraktera(naziv);
		_cijena = cijena;
		_barCode = AlocirajNizKaraktera(barCode);
	}
	~Artikal() { 
		delete[]_naziv; _naziv = nullptr; 
		delete[] _barCode; _barCode = nullptr;
	}
	friend ostream& operator<< (ostream &COUT, const Artikal &obj) { COUT << obj._naziv << " " << obj._cijena << " KM"; return COUT; }
	//1. potrebne funkcije
	Artikal(const Artikal& original){
		_barCode = AlocirajNizKaraktera(original._barCode);
		_naziv= AlocirajNizKaraktera(original._naziv);
		_cijena = original._cijena;
	}
	Artikal& operator=(const Artikal& original) {
		if (this != &original) {
			delete[] _barCode; delete[] _naziv;
			_barCode = AlocirajNizKaraktera(original._barCode);
			_naziv = AlocirajNizKaraktera(original._naziv);
			_cijena = original._cijena;
		}
		return *this;
	}
	friend bool operator==(const Artikal&, const Artikal&);
	float GetCijena() { return _cijena; }
};
bool operator==(const Artikal& a1, const Artikal& a2) {
	if (strcmp(a1._barCode, a2._barCode) == 0)
		return true;
	return false;
}

char _brRacuna[] = "RN0000001";
char* GenerisiSljedeciBrojRacuna() {
	char* temp = new char[15];
	strcpy_s(temp, 3, "RN");
	int brojac = 2;
	while (_brRacuna[brojac] == '0') {
		temp[brojac] = _brRacuna[brojac];
		brojac++;
	}
	temp[brojac] = '\0';
	int vel = 10 - brojac;
	char* buffer = new char[vel];
	strcpy_s(buffer, brojac, _brRacuna + brojac);
	int broj = atoi(buffer);
	if(broj!=1)
	broj++;
	_itoa_s(broj, buffer, vel, 10);
	strcat_s(temp, 15, buffer);

	strcpy_s(_brRacuna, 10, temp);
	return _brRacuna;
}
float GetUkupno(float cijena, int kolicina) { return cijena * kolicina; }
class Racun {
	//format broja racuna: RN6005491
	//osigurati automatsko generisanje broja racuna pocevsi od RN0000001
	char _brojRacuna[10];
	Datum _datumKreiranja;
	//prvi argument je Artikal, a drugi kolicina/broj artikala koju je kupac uzeo
	Kolekcija<Artikal *, int> * _stavkeRacuna;
	Kupac * _kupac;
public:
	/*
	1. potrebne (de)konstruktor i druge funkcije
	2. DodajArtikal - dodaje novi artikal u listu artikala zajedno sa njegovom kolicinom. 
	Onemoguciti ponavljanje artikala na nacin da se uvecava samo kolicina ukoliko korisnik vise puta 
	pokusa dodati isti artikal.
	3. operator<< - ispisuje racun u formatu prikazanom na slici(nije obavezno da bude identican, ali je svakako pozeljno).
	Nakon ispisa svakog racuna, ukoliko su ispunjeni definisani uslovi, kupcu se uvecava popust.
	4. Sortiraj - sortira artikle (rastuci) po kolicini novca kojom ucestvuju u ukupnom iznosu racuna
	*/
	Racun(const char* brojRacuna, Datum& datum, Kupac* kupac) {
		int vel = strlen(brojRacuna) + 1;
		strcpy_s(_brojRacuna, vel, brojRacuna);
		_datumKreiranja = datum;
		_stavkeRacuna = new Kolekcija<Artikal*, int>;
		_kupac = kupac;
	}
	Racun(const Racun& original){
		int vel = strlen(original._brojRacuna) + 1;
		strcpy_s(_brojRacuna, vel, original._brojRacuna);
		_datumKreiranja = original._datumKreiranja;
		_stavkeRacuna = new Kolekcija<Artikal*, int>;
		for (int i = 0; i < original._stavkeRacuna->getTrenutno(); i++)
			_stavkeRacuna->AddElement(new Artikal(*original._stavkeRacuna->getElement1(i)), original._stavkeRacuna->getElement2(i));	
	
		_kupac = original._kupac;
	}
	void DodajArtikal(const Artikal& artikal, int kolicina) {
		_stavkeRacuna->AddElement(new Artikal(artikal), kolicina);
	}
	friend ostream& operator<<(ostream& ispis, const Racun& racun) {

		ispis << "FITStore::Racun:: " << racun._brojRacuna << endl;
		ispis << "RB  Artikal\t  Cijena\tKolicina\tUkupno" << endl;
		float ukupno = 0, suma = 0;
		for (int i = 0; i < racun._stavkeRacuna->getTrenutno(); i++) {
			ukupno = GetUkupno(racun._stavkeRacuna->getElement1(i)->GetCijena(), racun._stavkeRacuna->getElement2(i));
			suma += ukupno;
			ispis << i + 1 << ". " << *racun._stavkeRacuna->getElement1(i) << "	" << racun._stavkeRacuna->getElement2(i) <<"\t\t"<<ukupno<< endl;
		}
		ispis << "Suma: " <<suma<<" KM"<< endl;
		ispis << "Popust: " <<racun._kupac->GetPopust()<<" KM"<< endl;
		ispis << "Za platiti: " <<suma-suma*racun._kupac->GetPopust()<<" KM"<< endl;
		return ispis;
	}
	void Sortiraj() {
		bool prolaz = true;
		int vel = _stavkeRacuna->getTrenutno() - 1;
		while (prolaz){
			prolaz = false;
			for (int i = 0; i < vel; i++){
				float ukupno1 = GetUkupno(_stavkeRacuna->getElement1(i)->GetCijena(), _stavkeRacuna->getElement2(i));
				float ukupno2 = GetUkupno(_stavkeRacuna->getElement1(i+1)->GetCijena(), _stavkeRacuna->getElement2(i+1));
				if (ukupno1 < ukupno2) {
					Artikal temp1 = *_stavkeRacuna->getElement1(i);
					int temp2 = _stavkeRacuna->getElement2(i);
					*_stavkeRacuna->getElement1(i) = *_stavkeRacuna->getElement1(i + 1);
					_stavkeRacuna->getElementi2()[i] = _stavkeRacuna->getElementi2()[i + 1];
					*_stavkeRacuna->getElement1(i + 1) = temp1;
					_stavkeRacuna->getElementi2()[i + 1] = temp2;
					prolaz = true;
				}
			}
			vel--;
		}
	}
};




int main() {

	/****************************************************************************
	1. SVE KLASE TREBAJU POSJEDOVATI ADEKVATAN DESTRUKTOR
	2. NAMJERNO IZOSTAVLJANJE KOMPLETNIH I/ILI POJEDINIH DIJELOVA DESTRUKTORA KOJI UZROKUJU RUNTIME ERROR CE BITI OZNACENO KAO "RE"
	3. SPASAVAJTE PROJEKAT KAKO BI SE SPRIJECILO GUBLJENJE URADJENOG ZADATKA
	4. PROGRAMSKI CODE SE TAKODJER NALAZI U FAJLU CODE_.TXT
	5. NAZIV I LISTA PARAMETARA FUNKCIJA MORAJU BITI IDENTICNI ONIMA KOJI SU KORI�TENI U TESTNOM CODE-U.
	6. NIJE POTREBNO BACATI IZUZETKE.
	****************************************************************************/
#pragma region TestiranjeDatuma
	Datum danas(26, 11, 2015);
	Datum sutra(danas);
	Datum prekosutra;
	prekosutra = danas;
	cout << danas << endl << sutra << endl << prekosutra << crt;
#pragma endregion
#pragma region TestiranjeKolekcije
	Kolekcija<int, int> kolekcija1;
	int brojElemenata = 15;
	for (int i = 0; i < brojElemenata; i++)
		kolekcija1.AddElement(i, i);
	cout << kolekcija1.getElement1(0) << " " << kolekcija1.getElement2(0) << endl;
	cout << kolekcija1 << endl;
	Kolekcija<int, int> kolekcija2;
	kolekcija2 = kolekcija1;
	cout << kolekcija2 << crt;
	if (kolekcija1.getTrenutno() == kolekcija2.getTrenutno())
		cout << "ISTI BROJ ELEMENATA" << endl;
	Kolekcija<int, int> kolekcija3(kolekcija2);
	cout << kolekcija3 << crt;
#pragma endregion
#pragma region TestiranjeKupca
	Kupac jasmin("Jasmin Azemovic", Datum(15, 2, 1980));
	Kupac adel("Adel Handzic", Datum(15, 10, 1981));
	Kupac jasmin2 = jasmin;
	cout << jasmin << endl << jasmin2 << endl;
	jasmin = jasmin2;
	cout << jasmin << endl << jasmin2 << endl;
#pragma endregion
#pragma region TestiranjeArtikla
	Artikal sokCppyNarandza("Cappy narandza", 2.10, "699511236545");
	Artikal cokoladaMilkaMilkins("Milka Milkins", 2.55, "622445875463");
	Artikal jogurtMeggle("Meggle jogurt", 2.55, "155338774458");

	cout << sokCppyNarandza << cokoladaMilkaMilkins << jogurtMeggle << endl;
	sokCppyNarandza = cokoladaMilkaMilkins;
	cout << sokCppyNarandza << endl;
	if (sokCppyNarandza == cokoladaMilkaMilkins)
		cout << "Artikli su isti" << endl;
#pragma endregion
#pragma region TestiranjeRacuna
	/*GenerisiSljedeciBrojRacuna - globalna funkcija koja generise i vraca naredni broj racuna u prethodno definisanom 
	formatu*/
	Racun racun1(GenerisiSljedeciBrojRacuna(), danas, &jasmin);

	racun1.DodajArtikal(cokoladaMilkaMilkins, 15);
	racun1.DodajArtikal(sokCppyNarandza, 20);
	racun1.DodajArtikal(jogurtMeggle, 20);

	cout << racun1 << endl;

	Racun racun2(GenerisiSljedeciBrojRacuna(), sutra, &jasmin);
	racun2.DodajArtikal(cokoladaMilkaMilkins, 15);
	racun2.DodajArtikal(sokCppyNarandza, 20);
	racun2.DodajArtikal(jogurtMeggle, 20);
	cout << racun2 << endl;

	Racun racun3(racun2);
	cout << racun3 << endl;
	racun3.Sortiraj();
	cout << racun3;
#pragma endregion
	system("pause>0");
	return 0;
}
//popust mozete izracunati kao procenat od ukupne sume racuna (pozeljna opcija) ili 
//kao vrijednost u KM-ovima (kao u nastavku)
/*
FITStore::Racun::RN000001
| RB Artikal Cijena Kolicina Ukupno |
| 1. Milka Milkins 2.55 15 38.25 |
| 2. Cappy narandza 2.1 20 42 |
| 3. Meggle jogurt 2.55 20 51 |
| Suma:131.25KM
| Popust : 0.1KM
| Za platiti : 131.15KM


FITStore::Racun::RN000002
| RB Artikal Cijena Kolicina Ukupno |
| 1. Milka Milkins 2.55 15 38.25 |
| 2. Cappy narandza 2.1 20 42 |
| 3. Meggle jogurt 2.55 20 51 |
| Suma:131.25KM
| Popust : 0.25KM
| Za platiti : 131KM


FITStore::Racun::RN000002
| RB Artikal Cijena Kolicina Ukupno |
| 1. Milka Milkins 2.55 15 38.25 |
| 2. Cappy narandza 2.1 20 42 |
| 3. Meggle jogurt 2.55 20 51 |
| Suma:131.25KM
| Popust : 0.4KM
| Za platiti : 130.85KM
*/
