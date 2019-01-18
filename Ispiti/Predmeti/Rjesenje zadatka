#include<iostream>
#include<vector>
#include<string>
#include<regex>
using namespace std;
enum Predmeti { Matematika, Historija, Geografia, Hemija, Biologija, Engleski, Muzicko, Fizicko };
enum Razredi { I1, I2, I3, II1, II2, II3, III1, III2, III3, IV1, IV2, IV3 };
const char * ispisPredmeti[] = { "Matematika", "Historija", "Geografia", "Hemija", "Biologija", "Engleski", "Muzicko", "Fizicko" };
const char * ispisRazredi[] = { "I1", "I2", "I3", "II1", "II2", "II3", "III1", "III2", "III3", "IV1", "IV2", "IV3" };
const char* crt = { "\n----------------------------------------\n" };

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
	int _trenutno;
	void Kopiraj(const Kolekcija& original) {
		_trenutno = original._trenutno;
		_elementi1 = new T1[_trenutno];
		_elementi2= new T2[_trenutno];
		for (int i = 0; i < _trenutno; i++){
			_elementi1[i] = original._elementi1[i];
			_elementi2[i] = original._elementi2[i];
		}
	}
public:
	Kolekcija(){
		_elementi1 = nullptr;
		_elementi2 = nullptr;
		_trenutno = 0;
	}

	Kolekcija(const Kolekcija& original) { Kopiraj(original); }

	Kolekcija& operator=(const Kolekcija& original) {
		if (this != &original) {
			delete[]_elementi1;
			delete[]_elementi2;
			Kopiraj(original);
		}
		return *this;
	}

	~Kolekcija(){
		delete[]_elementi1;
		delete[]_elementi2;
	}

	void AddElement(const T1& obj1, const T2& obj2) {
		T1* temp1 = new T1[_trenutno+1];
		T2* temp2 = new T2[_trenutno+1];
		for (int i = 0; i < _trenutno; i++) {
			temp1[i] = _elementi1[i];
			temp2[i] = _elementi2[i];
		}
		temp1[_trenutno] = obj1;
		temp2[_trenutno] = obj2;
		delete[] _elementi1; delete[] _elementi2;
		_elementi1 = temp1;
		_elementi2 = temp2;
		_trenutno++;
	}

	void RemoveElement(int indeks) {
		if (indeks < 0 || indeks >= _trenutno)
			throw exception("Greska, nepostojeca lokacija.");
		T1* temp1 = new T1[_trenutno - 1];
		T2* temp2 = new T2[_trenutno - 1];
		int br = 0;
		for (int i = 0; i < _trenutno; i++) {
			if (i != indeks) {
				temp1[br] = _elementi1[i];
				temp2[br] = _elementi2[i];
				br++;
			}
		}
		delete[] _elementi1; delete[] _elementi2;
		_elementi1 = temp1;
		_elementi2 = temp2;
		_trenutno--;
}

	T1& GetElement1(int indeks) {
		if (indeks < 0 || indeks >= _trenutno)
			throw exception("Greska, nepostojeca lokacija.");
		return _elementi1[indeks];
	}
	
	T2& GetElement2(int indeks) {
		if (indeks < 0 || indeks >= _trenutno)
			throw exception("Greska, nepostojeca lokacija.");
		return _elementi2[indeks];
	}

	friend ostream& operator<<(ostream& ispis, const Kolekcija& k) {
		for (int i = 0; i < k._trenutno; i++)
			ispis << k._elementi1[i] << "  " << k._elementi2[i] << endl;
		return ispis;
	}

	int GetTrenutno() { return _trenutno; }
};

class Datum {
	int *_dan, *_mjesec, *_godina;
public:
	Datum(int dan = 1, int mjesec = 1, int godina = 2000) {
		_dan = new int(dan);
		_mjesec = new int(mjesec);
		_godina = new int(godina);
	}

	Datum(const Datum&obj) {
		_dan = new int(*obj._dan);
		_mjesec = new int(*obj._mjesec);
		_godina = new int(*obj._godina);
	}

	Datum& operator=(const Datum& obj) {
		if (this != &obj) {
			delete _dan; delete _mjesec; delete _godina;
			_dan = new int(*obj._dan);
			_mjesec = new int(*obj._mjesec);
			_godina = new int(*obj._godina);
		}
		return *this;
	}

	~Datum() {
		delete _dan; _dan = nullptr;
		delete _mjesec; _mjesec = nullptr;
		delete _godina; _godina = nullptr;
	}

	friend ostream& operator<< (ostream &COUT, const Datum &obj) {
		COUT << *obj._dan << " " << *obj._mjesec << " " << *obj._godina;
		return COUT;
	}

	bool operator==(const Datum& datum) {
		int lijevi = *_godina * 10000 + *_mjesec * 100 + *_dan;
		int desni= *datum._godina * 10000 + *datum._mjesec * 100 + *datum._dan;
		if (lijevi == desni)
			return true;
		return false;

	}
};

bool ProvjeriValidnostBrojaTelefona(string telefon) {//globalna funkcija
	//broj telefona treba biti u formatu: +387(61)-222-333
	string pravilo = "(\\+\\d{3}\\(\\d{2}\\)-\\d{3}-\\d{3})";
	if (regex_match(telefon, regex(pravilo)))
		return true;
	return false;
}

class Osoba {
protected:
	char * _imePrezime;
	Datum * _datumRodjenja;
	string _brojTelefona;
public:
	Osoba(const char * imePrezime, Datum datum) {
		_imePrezime = AlocirajNizKaraktera(imePrezime);
		_datumRodjenja = new Datum(datum);
		_brojTelefona = "";

	}
	
	Osoba(const Osoba & obj) :_brojTelefona(obj._brojTelefona) {
		_imePrezime = AlocirajNizKaraktera(obj._imePrezime);
		_datumRodjenja = new Datum(*obj._datumRodjenja);
		_brojTelefona = obj._brojTelefona;
	}
	
	Osoba& operator=(const Osoba& original) {
		if (this != &original) {
			delete[] _imePrezime; delete _datumRodjenja;
			_imePrezime = AlocirajNizKaraktera(original._imePrezime);
			_datumRodjenja = new Datum(*original._datumRodjenja);
			_brojTelefona = original._brojTelefona;
		}
		return *this;
	}

	virtual ~Osoba() { delete[]_imePrezime; delete _datumRodjenja; }

	bool setBrojTelefona(string brojTelefona) {
		if (ProvjeriValidnostBrojaTelefona(brojTelefona)) {
			_brojTelefona = brojTelefona;
			return true;
		}
		return false;
	}

	friend ostream& operator<<(ostream& ispis, const Osoba& o) {
		ispis <<endl<<endl<< "Ime i prezime: " << o._imePrezime << endl;
		ispis << "Datum rodjenja: " << *o._datumRodjenja<< endl;
		ispis << "Broj telefona: " << o._brojTelefona<< endl;
		return ispis;
	}
};

class Nastavnik :public Osoba {
	Kolekcija<Predmeti, Razredi> * _predmetiRazredi;

public:
	Nastavnik(const char* imePrezime,const Datum& datum):Osoba(imePrezime,datum),_predmetiRazredi(new Kolekcija<Predmeti,Razredi>){}

	Nastavnik(const Nastavnik& original):Osoba(original){
		_predmetiRazredi = new Kolekcija<Predmeti, Razredi>(*original._predmetiRazredi);
	}

	Nastavnik& operator=(const Nastavnik& original) {
		if (this != &original) {
			delete _predmetiRazredi;
			//Osoba::operator=(original);
			(*(Osoba*)(this)) = original;
			_predmetiRazredi = new Kolekcija<Predmeti, Razredi>(*original._predmetiRazredi);
		}
		return *this;
	}

	virtual ~Nastavnik(){
		delete _predmetiRazredi;
		_predmetiRazredi = nullptr;
	}

	bool ProvjeraZaPredmet(Predmeti predmet, Razredi razred) {
		int brojac = 0;
		for (int i = 0; i < _predmetiRazredi->GetTrenutno(); i++){
			if (_predmetiRazredi->GetElement1(i) == predmet)
				if (_predmetiRazredi->GetElement2(i) == razred)
					return false;
				else
					brojac++;
		}
		if (brojac > 2)
			return false;
		return true;
	}

	bool AddPredmetRazred(Predmeti predmet, Razredi razred) {
		if (!ProvjeraZaPredmet(predmet, razred))
			return false;
		_predmetiRazredi->AddElement(predmet, razred);
		return true;
	}

	int GetIndeksPredmeta(Predmeti predmet) {
		for (int i = 0; i < _predmetiRazredi->GetTrenutno(); i++)
			if (_predmetiRazredi->GetElement1(i) == predmet)
				return i;
		return -1;
	}

	bool RemovePredmetRazred(Predmeti predmet) {
		int indeks = GetIndeksPredmeta(predmet);
		if (indeks == -1)
			return false;
		_predmetiRazredi->RemoveElement(indeks);
		return true;
	}

	bool operator==(const Nastavnik& nastavnik) {
		if (strcmp(_imePrezime, nastavnik._imePrezime) == 0 && *_datumRodjenja==*nastavnik._datumRodjenja &&
			_brojTelefona==nastavnik._brojTelefona)
			return true;
		return false;
	}

	Kolekcija<Predmeti, Razredi>& getPredmetiRazredi() { return *_predmetiRazredi; }

	char* GetImePrezime() { return _imePrezime; }

	friend ostream& operator<<(ostream& ispis, const Nastavnik& n) {
		ispis << static_cast<Osoba>(n)<< endl;
		for (int i = 0; i < n._predmetiRazredi->GetTrenutno(); i++)
			ispis << ispisPredmeti[n._predmetiRazredi->GetElement1(i)] << " " << ispisRazredi[n._predmetiRazredi->GetElement2(i)] << endl;
		return ispis;
	}
};
class Ucenik : public Osoba {
	int _brojUDnevniku;
	Razredi _razred;
	vector<Nastavnik *> _nastavnici;
	Kolekcija<Predmeti, int> _predmetiOcjene;

public:
	Ucenik(const char* imePrezime, const Datum& datum,int brUDnevniku,Razredi razred):Osoba(imePrezime,datum){
		_brojUDnevniku = brUDnevniku;
		_razred = razred;
	}

	Ucenik(const Ucenik& original):Osoba(original){
		_brojUDnevniku = original._brojUDnevniku;
		_razred = original._razred;
		_nastavnici = original._nastavnici;
		_predmetiOcjene = original._predmetiOcjene;
	}

	Ucenik& operator=(const Ucenik& original) {
		if (this != &original) {
			(*(Osoba*)(this)) = original;
			_brojUDnevniku = original._brojUDnevniku;
			_razred = original._razred;
			_nastavnici = original._nastavnici;
			_predmetiOcjene = original._predmetiOcjene;
		}
		return *this;
	}

	bool DaLiPostoji(const Nastavnik& nastavnik) {
		for (int i = 0; i < _nastavnici.size(); i++)
			if (*_nastavnici[i] == nastavnik)
				return true;
		return false;
	}

	bool AddNastavnik(const Nastavnik& nastavnik) {
		if (DaLiPostoji(nastavnik))
			return false;
		if (const_cast<Nastavnik*>(&nastavnik)->getPredmetiRazredi().GetTrenutno() < 1)
			return false;
		_nastavnici.push_back(const_cast<Nastavnik*>(&nastavnik));
		return true;
	}

	bool ImaLiNastavnik(Predmeti predmet) {
		for (int i = 0; i < _nastavnici.size(); i++) {
			for (int j = 0; j < _nastavnici[i]->getPredmetiRazredi().GetTrenutno(); j++) {
				if (_nastavnici[i]->getPredmetiRazredi().GetElement1(j) == predmet)
					return true;
			}
		}
		return false;
	}

	bool AddPredmetOcjenu(Predmeti predmet, int ocjena) {
		if (!ImaLiNastavnik(predmet))
			return false;
		_predmetiOcjene.AddElement(predmet, ocjena);
		return true;
	}

	vector<Nastavnik*> & getNastavnici() { return _nastavnici; }

	Kolekcija<Predmeti, int>& getPredmetiOcjene() { return _predmetiOcjene; }

	float GetProsjekByNastavnik(const char* imePrezimeNastavnika) {
		int indeks = -1;
		for (int i = 0; i < _nastavnici.size(); i++) {
			if (strcmp(_nastavnici[i]->GetImePrezime(), imePrezimeNastavnika) == 0) {
				indeks = i;
				break;
			}
		}
		if (indeks == -1)
			return 0;
		float prosjek = 0;
		int brojac = 0;
		for (int i = 0; i < _nastavnici[indeks]->getPredmetiRazredi().GetTrenutno(); i++) {
			for (int j = 0; j < _predmetiOcjene.GetTrenutno(); j++) {
				if (_predmetiOcjene.GetElement1(j) == _nastavnici[indeks]->getPredmetiRazredi().GetElement1(i)) {
					prosjek += _predmetiOcjene.GetElement2(j);
					brojac++;
				}
			}
		}
		return prosjek / brojac;
	}

	friend ostream& operator<<(ostream& ispis,  Ucenik& u) {
		ispis << (*(Osoba*)(&u)) << endl;
		ispis << "Razred: " << ispisRazredi[u._razred] << endl;
		for (int i = 0; i < u._nastavnici.size(); i++)
			ispis << *u._nastavnici[i] << endl;
		for (int i = 0; i < u._predmetiOcjene.GetTrenutno(); i++)
			ispis << ispisPredmeti[u._predmetiOcjene.GetElement1(i)] << " -> " << u._predmetiOcjene.GetElement2(i) << endl;
		return ispis;
	}
};


void main() {

	/****************************************************************************
	1. SVE KLASE TREBAJU POSJEDOVATI ADEKVATAN DESTRUKTOR
	2. NAMJERNO IZOSTAVLJANJE KOMPLETNIH I/ILI POJEDINIH DIJELOVA DESTRUKTORA KOJI UZROKUJU RUNTIME ERROR CE BITI OZNACENO KAO "RE"
	3. SPASAVAJTE PROJEKAT KAKO BI SE SPRIJECILO GUBLJENJE URADJENOG ZADATKA
	4. PROGRAMSKI CODE SE TAKODJER NALAZI U FAJLU CODE_.TXT
	5. NAZIV I LISTA PARAMETARA FUNKCIJA MORAJU BITI IDENTICNI ONIMA KOJI SU KORIŠTENI U TESTNOM CODE-U.
	6. NIJE POTREBNO BACATI IZUZETKE. SVE FUNKCIJE VRACAJU TRUE ILI FALSE
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
	for (size_t i = 0; i < brojElemenata; i++)
		kolekcija1.AddElement(i, i);
	kolekcija1.RemoveElement(0);//uklanja element na osnovu lokacije
	cout << kolekcija1.GetElement1(0) << " " << kolekcija1.GetElement2(0) << endl;
	cout << kolekcija1 << endl;
	Kolekcija<int, int> kolekcija2;
	kolekcija2 = kolekcija1;
	cout << kolekcija2 << crt;
	if (kolekcija1.GetTrenutno() == kolekcija2.GetTrenutno())
		cout << "ISTI BROJ ELEMENATA" << endl;
	Kolekcija<int, int> kolekcija3(kolekcija2);
	cout << kolekcija3 << crt;
#pragma endregion

#pragma region TestiranjeNastavnika
	Nastavnik jasmin("Jasmin Azemovic", Datum(15, 2, 1980));
	Nastavnik adel("Adel Handzic", Datum(15, 10, 1981));
	
	/*NASTAVNIK MOZE PREDAVATI ISTI PREDMET, ALI TO MORA BITI U RAZLICITIM ODJELJENJIMA I NAJVISE U DVA RAZREDA 
	(NA DVIJE RAZLICITE GODINE)*/

	if (jasmin.AddPredmetRazred(Matematika, I1))
		cout << "Predmeti dodan" << endl;
	if (jasmin.AddPredmetRazred(Historija, I1))
		cout << "Predmeti dodan" << endl;

	if (jasmin.AddPredmetRazred(Geografia, I1))
		cout << "Predmeti dodan" << endl;

	if (jasmin.AddPredmetRazred(Geografia, I2))
		cout << "Predmeti dodan" << endl;

	if (adel.AddPredmetRazred(Historija, III1))
		cout << "Predmeti dodan" << endl;

	if (adel.RemovePredmetRazred(Historija))
		cout << "Predmet uklonjen!" << endl;

	Nastavnik goran(adel);
	if (goran == adel)
		cout << "Nastavnici identicni" << endl;
	goran = jasmin;
	if (goran == jasmin)
		cout << "Nastavnici identicni" << endl;

	Kolekcija<Predmeti, Razredi> & predmetiRazredi = jasmin.getPredmetiRazredi();

	if (!jasmin.setBrojTelefona("+38761111222"))
		cout << "Broj telefona nije dodan!" << endl;
	if (!jasmin.setBrojTelefona("+387-61-111-222"))
		cout << "Broj telefona nije dodan!" << endl;
	if (jasmin.setBrojTelefona("+387(61)-111-222"))
		cout << "Broj telefona uspjesno dodan!" << endl;
#pragma endregion

#pragma region TestiranjeUcenika
	Ucenik denis("Denis Music", Datum(8, 10, 1990), 3, III1);
	Ucenik elmin("Elmin Sudic", Datum(15, 6, 1993), 1, I1);
	Ucenik adil("Adil Joldic", Datum(13, 8, 1992), 2, II1);
	/*VODITI RACUNA DA SE DODAJU REFERENCE NA NASTAVNIKE KAKO BI SVI UCENICI DIJELILI ISTE INFORMACIJE O NASTAVNICIMA U SLUCAJU 
	DA SE NASTAVNIKU DODA NEKI PREDMET*/
	/*ZA USPJESNO DODAVANJE, NASTAVNIK MORA POSJEDOVATI (PREDAVATI) NAJMANJE JEDAN PREDMET U TOM RAZREDU */

		if (denis.AddNastavnik(jasmin))
		cout << "Nastavnik uspjesno dodan" << endl;
	/*ONEMOGUCITI DODAVANJE ISTIH NASTAVNIKA; POJAM ISTI PODRAZUMIJEVA IDENTICNE VRIJEDNOSTI SVIH ATRIBUTA*/
	if (denis.AddNastavnik(adel))
		cout << "Nastavnik uspjesno dodan" << endl;

	/*DA BI SE DODALA OCJENA ZA ODREDJENI PREDMET UCENIK MORA POSJEDOVATI NASTAVNIKA KOJI PREDAJE TAJ PREDMET*/
	if (denis.AddPredmetOcjenu(Matematika, 3))
		cout << "Ocjena uspjesno dodana" << endl;
	if (denis.AddPredmetOcjenu(Historija, 4))
		cout << "Ocjena uspjesno dodana" << endl;

	vector<Nastavnik*> & nastavnici = denis.getNastavnici();
	cout << "Ucenik Denis ima dodijeljena " << nastavnici.size() << " nastavnika" << endl;

	Kolekcija<Predmeti, int> & uspjeh = denis.getPredmetiOcjene();
	cout << "Denis ima " << uspjeh.GetTrenutno() << " polozenih predmeta" << endl;
	cout << "Prosjecna ocjena kod nastavnika Jasmin Azemovic je " << denis.GetProsjekByNastavnik("Jasmin Azemovic") << endl;

	Ucenik denis2(denis);
	cout << denis2 << endl;

#pragma endregion
	system("pause>0");
}
