#include <iostream>
#include<vector>
#include<string>
#include<sstream>
#include<thread>
#include<mutex>
using namespace std;

#pragma warning(disable:4996)
const char *crt = "\n-------------------------------------------\n";
enum eRazred { PRVI = 1, DRUGI, TRECI, CETVRTI };
const float minimalan_prosjek = 4.5;

char * AlocirajNizKaraktera(const char * sadrzaj) {
	if (sadrzaj == nullptr)
		return nullptr;
	int vel = strlen(sadrzaj) + 1;
	char * temp = new char[vel];
	strcpy_s(temp, vel, sadrzaj);
	return temp;
}

template<class T1, class T2>
class Kolekcija {
	T1 * _elementi1;
	T2 * _elementi2;
	int * _trenutno;
	void Kopiraj(const Kolekcija& original) {
		_trenutno = new int(*original._trenutno);
		_elementi1 = new T1[*_trenutno];
		_elementi2 = new T2[*_trenutno];
		for (int i = 0; i < *_trenutno; i++){
			_elementi1[i] = original._elementi1[i];
			_elementi2[i] = original._elementi2[i];
		}
	}
public:
	Kolekcija() {
		_elementi1 = nullptr;
		_elementi2 = nullptr;
		_trenutno = new int(0);
	}

	Kolekcija(const Kolekcija& original) { Kopiraj(original); }

	Kolekcija& operator=(const Kolekcija& original) {
		if (this != &original) {
			delete[] _elementi1; delete[]_elementi2; delete _trenutno;
			Kopiraj(original);
		}
		return *this;
	}

	~Kolekcija() {
		delete[]_elementi1; _elementi1 = nullptr;
		delete[]_elementi2; _elementi2 = nullptr;
		delete _trenutno; _trenutno = nullptr;
	}

	T1& getElement1(int lokacija) { return _elementi1[lokacija]; }
	
	T2& getElement2(int lokacija) { return _elementi2[lokacija]; }
	
	int getTrenutno() { return *_trenutno; }
	
	void AddElement(const T1& obj1, const T2& obj2) {
		T1* temp1 = new T1[*_trenutno + 1];
		T2* temp2 = new T2[*_trenutno + 1];
		for (int i = 0; i < *_trenutno; i++){
			temp1[i] = _elementi1[i];
			temp2[i] = _elementi2[i];
		}
		temp1[*_trenutno] = obj1;
		temp2[*_trenutno] = obj2;
		delete[] _elementi1; delete[] _elementi2;
		_elementi1 = temp1;
		_elementi2 = temp2;
		(*_trenutno)++;
	}

	void SortOpadajuciByT2() {
		bool prolaz = true;
		int vel = *_trenutno - 1;
		while (prolaz){
			prolaz = false;
			for (int i = 0; i < vel; i++){
				if (_elementi2[i] < _elementi2[i + 1]) {
					swap(_elementi1[i], _elementi1[i + 1]);
					swap(_elementi2[i], _elementi2[i + 1]);
					prolaz = true;
				}
			}
			vel--;
		}
	}

	friend ostream& operator<< (ostream &COUT, Kolekcija &obj) {
		for (size_t i = 0; i < *obj._trenutno; i++)
			COUT << obj.getElement1(i) << " " << obj.getElement2(i) << endl;
		return COUT;
	}
};

class DatumVrijeme {
	int *_dan, *_mjesec, *_godina, *_sati, *_minuti;
	void Kopiraj(const DatumVrijeme& original) {
		_dan = new int(*original._dan);
		_mjesec = new int(*original._mjesec);
		_godina = new int(*original._godina);
		_sati = new int(*original._sati);
		_minuti = new int(*original._minuti);
	}
public:
	DatumVrijeme(int dan = 1, int mjesec = 1, int godina = 2000, int sati = 0, int minuti = 0) {
		_dan = new int(dan);
		_mjesec = new int(mjesec);
		_godina = new int(godina);
		_sati = new int(sati);
		_minuti = new int(minuti);
	}

	DatumVrijeme(const DatumVrijeme& original) { Kopiraj(original); }

	DatumVrijeme& operator=(const DatumVrijeme& original) {
		if (this != &original) {
			delete _dan; delete _mjesec; delete _godina; delete _sati; delete _minuti;
			Kopiraj(original);
		}
		return *this;
	}

	~DatumVrijeme() {
		delete _dan; _dan = nullptr;
		delete _mjesec; _mjesec = nullptr;
		delete _godina; _godina = nullptr;
		delete _sati; _sati = nullptr;
		delete _minuti; _minuti = nullptr;
	}

	friend ostream& operator<< (ostream &COUT, DatumVrijeme &obj) {
		COUT << *obj._dan << "." << *obj._mjesec << "." << *obj._godina << " " << *obj._sati << ":" << *obj._minuti << endl;
		return COUT;
	}

	char* ToCharArray() {
		stringstream ss;
		ss << *_dan << "." << *_mjesec << "." << *_godina << " " << *_sati << ":" << *_minuti << endl;
		return AlocirajNizKaraktera(ss.str().c_str());
	}

	int operator-(DatumVrijeme& datum) {
		int lijevi = *_godina * 100000000 + *_mjesec * 1000000 + *_dan * 10000 + *_sati * 100 + *_minuti;
		int desni= *datum._godina * 100000000 + *datum._mjesec * 1000000 + *datum._dan * 10000 + *datum._sati * 100 + *datum._minuti;
		return lijevi - desni;
	}
};

class Predmet {
	char * _naziv;
	int _ocjena;
	string _napomena;
	void Kopiraj(const Predmet& original) {
		_naziv = AlocirajNizKaraktera(original._naziv);
		_ocjena = original._ocjena;
		_napomena = original._napomena;
	}
public:
	Predmet(const char * naziv, int ocjena, string napomena = "") :_ocjena(ocjena), _napomena(napomena) {
		_naziv = AlocirajNizKaraktera(naziv);
	}

	Predmet(const Predmet& original) { Kopiraj(original); }

	Predmet& operator=(const Predmet& original) {
		if (this != &original) {
			delete[] _naziv;
			Kopiraj(original);
		}
		return *this;
	}

	~Predmet() {
		delete[] _naziv; _naziv = nullptr;
	}
	
	friend ostream& operator<< (ostream &COUT, Predmet &obj) {
		COUT << obj._naziv << " (" << obj._ocjena << ") " << obj._napomena << endl;
		return COUT;
	}
	
	char * GetNaziv() { return _naziv; }
	
	string GetNapomena() { return _napomena; }
	
	void DodajNapomenu(string napomena) {
		_napomena += " " + napomena;
	}

	int GetOcjena() { return _ocjena; }
};
struct Uspjeh {
	eRazred _razred;
	//datumvrijeme se odnosi na vrijeme evidentiranja polozenog predmeta
	Kolekcija<Predmet*, DatumVrijeme> * _predmeti;
public:
	Uspjeh(eRazred razred) :_razred(razred), _predmeti(nullptr) {}

	Uspjeh(const Uspjeh& original):_predmeti(new Kolekcija<Predmet*, DatumVrijeme>(*original._predmeti)),
	_razred(original._razred){}

	Uspjeh& operator=(const Uspjeh& original) {
		if (this != &original) {
			delete _predmeti;
			_razred = original._razred;
			_predmeti = new Kolekcija<Predmet*, DatumVrijeme>(*original._predmeti);
		}
		return *this;
	}

	~Uspjeh() {delete _predmeti; _predmeti = nullptr;}

	Kolekcija<Predmet*, DatumVrijeme> * GetPredmeti() { return _predmeti; }

	friend ostream& operator<< (ostream &COUT, const Uspjeh &obj) {
		COUT <<"Razred: "<< obj._razred <<endl<<endl;
		for (int i = 0; i < obj._predmeti->getTrenutno(); i++)
			cout << *obj._predmeti->getElement1(i) << "  " << obj._predmeti->getElement2(i) << endl;
		return COUT;
	}
};

mutex cuvar;

class Kandidat {
	char * _imePrezime;
	string _emailAdresa;
	string _brojTelefona;
	vector<Uspjeh *> _uspjeh;
	void Kopiraj(const Kandidat& original) {
		_imePrezime = AlocirajNizKaraktera(original._imePrezime);
		_emailAdresa = original._emailAdresa;
		_brojTelefona = original._brojTelefona;
		for (int i = 0; i < original._uspjeh.size(); i++)
			_uspjeh.push_back(new Uspjeh(*original._uspjeh[i]));
	}
public:
	Kandidat(const char * imePrezime, string emailAdresa, string brojTelefona) :_emailAdresa(emailAdresa), _brojTelefona(brojTelefona) {
		_imePrezime = AlocirajNizKaraktera(imePrezime);
	}

	Kandidat(const Kandidat& original) { Kopiraj(original); }

	Kandidat& operator=(const Kandidat& original) {
		if (this != &original) {
			delete[] _imePrezime;
			for (int i = 0; i < _uspjeh.size(); i++)
				delete _uspjeh[i];
			Kopiraj(original);
		}
		return *this;
	}

	~Kandidat() {
		delete[] _imePrezime; _imePrezime = nullptr;
		for (size_t i = 0; i < _uspjeh.size(); i++) {
			delete _uspjeh[i];
			_uspjeh[i] = nullptr;
		}
	}

	friend ostream& operator<< (ostream &COUT, Kandidat &obj) {
		COUT << obj._imePrezime << " " << obj._emailAdresa << " " << obj._brojTelefona << endl;
		for (size_t i = 0; i < obj._uspjeh.size(); i++)
			COUT << *obj._uspjeh[i] << endl;
		return COUT;
	}
	
	vector<Uspjeh *> GetUspjeh() { return _uspjeh; }

	int NadjiRazred(eRazred razred) {
		for (int i = 0; i < _uspjeh.size(); i++)
			if (_uspjeh[i]->_razred == razred)
				return i;
		return -1;
	}

	bool PostojiPredmet(Predmet& predmet,int indeks) {
		for (int i = 0; i < _uspjeh[indeks]->_predmeti->getTrenutno(); i++)
			if (strcmp(_uspjeh[indeks]->GetPredmeti()->getElement1(i)->GetNaziv(), predmet.GetNaziv()) == 0)
				return true;
		return false;
	}

	float GetProsjekZaRazred(int indeks) {
		float ukupno = 0;
		for (int i = 0; i < _uspjeh[indeks]->_predmeti->getTrenutno(); i++)
			ukupno += _uspjeh[indeks]->_predmeti->getElement1(i)->GetOcjena();
		return ukupno / _uspjeh[indeks]->_predmeti->getTrenutno();
		
	}

	void PosaljiPoruku(int indeks) {
		float prosjek = GetProsjekZaRazred(indeks);
		cuvar.lock();
		cout << "Evidentirali ste uspjeh za " << indeks << ". " << "razred" << endl;
		if (prosjek > minimalan_prosjek)
			cout << "Svaka cast za uspjeh " << prosjek << endl;
		cuvar.unlock();
	}

	bool AddPredmet(eRazred razred,  Predmet& predmet,  DatumVrijeme& datum) {
		int indeks = NadjiRazred(razred);
		if (indeks == -1) {
			_uspjeh.push_back(new Uspjeh(razred));
			indeks = _uspjeh.size() - 1;
		}
		if (_uspjeh[indeks]->GetPredmeti() != nullptr) {
			if (_uspjeh[indeks]->_predmeti->getTrenutno() == 6) 
				return false;
			if (PostojiPredmet(predmet, indeks))
				return false;
			if (datum - _uspjeh[indeks]->_predmeti->getElement2(_uspjeh[indeks]->_predmeti->getTrenutno() - 1) < 1)
				return false;
		}
		else
			_uspjeh[indeks]->_predmeti = new Kolekcija<Predmet*, DatumVrijeme>;

		_uspjeh[indeks]->_predmeti->AddElement(&predmet, datum);
		if (_uspjeh[indeks]->_predmeti->getTrenutno() == 6) {
			thread posaljiPoruku(&Kandidat::PosaljiPoruku, *this, indeks);
			posaljiPoruku.join();
		}

		return true;
	}

	Uspjeh* operator()(eRazred razred) {
		int indeks = NadjiRazred(razred);
		if (indeks == -1)
			return nullptr;
		return _uspjeh[indeks];
	}
};

void main() {
	/****************************************************************************
	1. SVE KLASE TREBAJU POSJEDOVATI ADEKVATAN DESTRUKTOR
	2. NAMJERNO IZOSTAVLJANJE KOMPLETNIH I/ILI POJEDINIH DIJELOVA DESTRUKTORA KOJI UZROKUJU RUNTIME ERROR CE BITI OZNACENO KAO "RE"
	3. SPAŠAVAJTE PROJEKAT KAKO BI SE SPRIJECILO GUBLJENJE URAÐENOG ZADATKA
	4. PROGRAMSKI CODE SE TAKOÐER NALAZI U FAJLU CODE.TXT
	5. NAZIVI FUNKCIJA, TE BROJ I TIP PARAMETARA MORAJU BITI IDENTICNI ONIMA KOJI SU KORIŠTENI U TESTNOM CODE-U, OSIM U SLUCAJU DA POSTOJI ADEKVATAN RAZLOG ZA NJIHOVU MODIFIKACIJU. OSTALE, POMOCNE FUNKCIJE MOŽETE IMENOVATI I DODAVATI PO ŽELJI.
	6. IZUZETAK BACITE U FUNKCIJAMA U KOJIMA JE TO NAZNACENO.
	****************************************************************************/

	DatumVrijeme temp,
		datum19062018_1015(19, 6, 2018, 10, 15),
		datum20062018_1115(20, 6, 2018, 11, 15),
		datum30062018_1215(30, 6, 2018, 12, 15),
		datum05072018_1231(5, 7, 2018, 12, 31),
		datum20062018_1115_Copy(datum20062018_1115),

		test1(5, 7, 2018, 12, 45), //dodano zbog testiranja
		test2(5, 7, 2018, 12, 50), //dodano zbog testiranja
		test3(5, 7, 2018, 13, 31), //dodano zbog testiranja
		test4(5, 7, 2018, 14, 31); //dodano zbog testiranja

	//funkcija ToCharArray vraca datum i vrijeme kao char *
	cout << datum19062018_1015.ToCharArray() << endl;//treba ispisati: 19.6.2018 10:15
	temp = datum05072018_1231;
	cout << temp.ToCharArray() << endl;//treba ispisati: 5.7.2018 12:31
	cout << datum20062018_1115_Copy.ToCharArray() << endl;//treba ispisati: 20.6.2018 11:15

	const int kolekcijaTestSize = 10;
	Kolekcija<int, int> kolekcija1;
	for (size_t i = 0; i < kolekcijaTestSize; i++)
		kolekcija1.AddElement(i + 1, i * i);
	cout << kolekcija1 << endl;

	Kolekcija<int, int> kolekcija2 = kolekcija1;
	cout << kolekcija2 << crt;
	Kolekcija<int, int> kolekcija3;
	kolekcija3 = kolekcija1;
	kolekcija3.SortOpadajuciByT2(); //od najvece prema najmanjoj vrijednosti
	cout << kolekcija3 << crt;

	//napomena se moze dodati i prilikom kreiranja objekta
	Predmet Matematika("Matematika", 5, "Ucesce na takmicenju"),
		Fizika("Fizika", 5),
		Hemija("Hemija", 3),
		Engleski("Engleski", 5),
		Historija("Historija", 5),//dodano zbog testiranja
		Informatika("Informatika", 5);//dodano zbog testiranja

	Fizika.DodajNapomenu("Pohvala za ostvareni uspjeh"); // dodaje novu napomenu zadrzavajuci ranije dodane
	cout << Matematika << endl;

	Kandidat jasmin("Jasmin Azemovic", "jasmin@gmail.com", "033 281 172");
	Kandidat adel("Adel Handzic", "adel@edu.fit.ba", "033 281 170");

	/*
	uspjeh (tokom srednjoskolskog obrazovanja) se dodaje za svaki predmet na nivou razreda.
	tom prilikom onemoguciti:
	- dodavanje vise od 6 predmeta za jedan razred
	- dodavanje istoimenih predmeta na nivou jednog razreda,
	- dodavanje vise predmeta u kratkom vremenskom periodu (na nivou jednog razreda, razmak izmedju dodavanja pojedinih
	  predmeta mora biti najmanje minut).
	razredi (predmeti ili uspjeh) ne moraju biti dodavani sortiranim redoslijedom (npr. prvo se moze dodati uspjeh za ii
	razred, pa onda za i razred i sl.). funkcija vraca true ili false u zavisnosti od (ne)uspjesnost izvrsenja
	*/
	if (jasmin.AddPredmet(DRUGI, Fizika, datum20062018_1115))
		cout << "Predmet uspjesno dodan!" << crt;
	if (jasmin.AddPredmet(DRUGI, Hemija, datum30062018_1215))
		cout << "Predmet uspjesno dodan!" << crt;
	if (jasmin.AddPredmet(PRVI, Engleski, datum19062018_1015))
		cout << "Predmet uspjesno dodan!" << crt;
	if (jasmin.AddPredmet(PRVI, Matematika, datum20062018_1115))
		cout << "Predmet uspjesno dodan!" << crt;
	//ne treba dodati Matematiku jer je vec dodana u prvom razredu
	if (jasmin.AddPredmet(PRVI, Matematika, datum05072018_1231))
		cout << "Predmet uspjesno dodan!" << crt;
	//ne treba dodati Fiziku jer nije prosao minut od dodavanja posljednjeg predmeta
	if (jasmin.AddPredmet(PRVI, Fizika, datum20062018_1115))
		cout << "Predmet uspjesno dodan!" << crt;

	if (jasmin.AddPredmet(PRVI, Fizika, test1)) //dodano zbog testiranja
		cout << "Predmet uspjesno dodan!" << crt;
	if (jasmin.AddPredmet(PRVI, Historija, test2)) //dodano zbog testiranja
		cout << "Predmet uspjesno dodan!" << crt;
	if (jasmin.AddPredmet(PRVI, Informatika, test3)) //dodano zbog testiranja
		cout << "Predmet uspjesno dodan!" << crt;
	if (jasmin.AddPredmet(PRVI, Hemija, test4)) //dodano zbog testiranja
		cout << "Predmet uspjesno dodan!" << crt;

	/*pretpostavka je da na nivou jednog razreda kandidati imaju 6 predmeta.
	upravo zbog toga, nakon evidentiranja 6 predmeta na nivou jednog razreda kandidatu se salje mail
	sa porukom: "evidentirali ste uspjeh za X razred". ukoliko je prosjek na nivou tog razreda veci od minimalan_prosjek
	kandidatu se salje SMS sa porukom: "svaka cast za uspjeh 4.X". slanje poruka i emailova implemenitrati koristeci
	zasebne thread-ove.
	*/

	Uspjeh * u = jasmin(PRVI);//vraca uspjeh kandidata ostvaren u prvom razredu
	if (u != nullptr)
		cout << *u << endl;


	system("pause>0");
}
