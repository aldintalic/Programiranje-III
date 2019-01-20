#include<iostream>
#include<vector>
#include<string>
#include<ctime>
#include<thread>
#include<regex>
#include<mutex>
#include<chrono>
using namespace std;

const char *crt = "\n-----------------------------------------------\n";

enum Status { ZAHTJEV_ZAPRIMLJEN, ODUSTAO, ODBIJENA, IZDATA, ISTEKLA, PONISTENA };
enum Sortiranje { RASTUCI, OPADAJUCI };
enum Drzava { SAD, AUSTRALIJA, ENGLESKA };

const char* charStatusi[] = { "ZAHTJEV_ZAPRIMLJEN", "ODUSTAO", "ODBIJENA", "IZDATA", "ISTEKLA", "PONISTENA" };
const char* charDrzave[] = { "SAD", "AUSTRALIJA", "ENGLESKA" };


char* AlocirajNizKaraktera(const char* sadrzaj) {
	if (sadrzaj == nullptr)
		return nullptr;
	int vel = strlen(sadrzaj) + 1;
	char* temp = new char[vel];
	strcpy_s(temp, vel, sadrzaj);
	return temp;
}

class Datum {
	int *_dan, *_mjesec, *_godina;
	void Kopiraj(const Datum& original) {
		_dan = new int(*original._dan);
		_mjesec = new int(*original._mjesec);
		_godina = new int(*original._godina);
	}
public:
	Datum(int dan = 1, int mjesec = 1, int godina = 2000) {
		_dan = new int(dan);
		_mjesec = new int(mjesec);
		_godina = new int(godina);
	}

	Datum(const Datum& original) { Kopiraj(original); }

	Datum& operator=(const Datum& original) {
		if (this != &original) {
			delete _dan; delete _mjesec; delete _godina;
			Kopiraj(original);
		}
		return *this;
	}

	~Datum() {
		delete _dan; _dan = nullptr;
		delete _mjesec; _mjesec = nullptr;
		delete _godina; _godina = nullptr;
	}

	friend ostream& operator<< (ostream &COUT, Datum &obj) {
		COUT << *obj._dan << "." << *obj._mjesec << "." << *obj._godina;
		return COUT;
	}

	Datum operator++(int) {
		Datum datum(*this);
		++(*this);
		return datum;
	}

	Datum& operator++() {
		(*_dan)++;
		if ((*_mjesec == 1 || *_mjesec == 3 || *_mjesec == 5 || *_mjesec == 7 || *_mjesec == 8 || *_mjesec == 10 || *_mjesec == 12)
			&& *_dan > 31) {
			*_dan = 1;
			(*_mjesec)++;
		}
		else if ((*_mjesec == 4 || *_mjesec == 6 || *_mjesec == 9 || *_mjesec == 11) && *_dan > 30) {
			*_dan = 1;
			(*_mjesec)++;
		}
		else if (*_mjesec == 2) {
			if ((*_godina % 4 == 0 && *_godina % 100 == 0) || *_godina % 400 == 0) {
				if (*_dan > 29) {
					*_dan = 1;
					(*_mjesec)++;
				}
			}
			else {
				if (*_dan > 28) {
					*_dan = 1;
					(*_mjesec)++;
				}
			}
		}

		if (*_mjesec > 12) {
			*_mjesec = 1;
			(*_godina)++;
		}
		return *this;
	}

	bool operator==(Datum& datum) {
		int lijevi = *_godina * 10000 + *_mjesec * 100 + *_dan;
		int desni = *datum._godina * 10000 + *datum._mjesec * 100 + *datum._dan;
		if (lijevi == desni)
			return true;
		return false;
	}

	bool operator>(Datum& datum) {
		int lijevi = *_godina * 10000 + *_mjesec * 100 + *_dan;
		int desni = *datum._godina * 10000 + *datum._mjesec * 100 + *datum._dan;
		if (lijevi > desni)
			return true;
		return false;
	}

	bool operator<(Datum& datum) {
		int lijevi = *_godina * 10000 + *_mjesec * 100 + *_dan;
		int desni = *datum._godina * 10000 + *datum._mjesec * 100 + *datum._dan;
		if (lijevi < desni)
			return true;
		return false;
	}

	int operator-(const Datum& datum) {
		int lijevi = *_godina * 10000 + *_mjesec * 100 + *_dan;
		int desni = *datum._godina * 10000 + *datum._mjesec * 100 + *datum._dan;
		return lijevi - desni;
	}
};

template<class T1, class T2, int max>
class FITKolekcija {
	T1 * _elementi1[max];
	T2 * _elementi2[max];
	int _trenutno;
	void Dealociraj() {
		for (size_t i = 0; i < max; i++) {
			delete _elementi1[i]; _elementi1[i] = nullptr;
			delete _elementi2[i]; _elementi2[i] = nullptr;
		}
	}
	void Kopiraj(const FITKolekcija& original) {
		_trenutno = original._trenutno;
		for (int i = 0; i < _trenutno; i++) {
			_elementi1[i] = new T1(*original._elementi1[i]);
			_elementi2[i] = new T2(*original._elementi2[i]);
		}
	}
public:
	FITKolekcija() {
		for (size_t i = 0; i < max; i++) {
			_elementi1[i] = nullptr;
			_elementi2[i] = nullptr;
		}
		_trenutno = 0;
	}

	FITKolekcija(const FITKolekcija& original) { Kopiraj(original); }

	FITKolekcija& operator=(const FITKolekcija& original) {
		if (this != &original) {
			Dealociraj();
			Kopiraj(original);
		}
		return *this;
	}

	~FITKolekcija() { Dealociraj(); }

	void Dodaj(const T1& obj1, const T2& obj2) {
		if (_trenutno == max)
			throw exception("Kolekcija je puna. Ne moze se dodati element.");
		_elementi1[_trenutno] = new T1(obj1);
		_elementi2[_trenutno] = new T2(obj2);
		_trenutno++;
	}

	T1 ** GetT1() { return _elementi1; }

	T2 ** GetT2() { return _elementi2; }

	int GetTrenutno() { return _trenutno; }

	friend ostream& operator<< (ostream &COUT, const FITKolekcija &obj) {
		for (size_t i = 0; i < obj._trenutno; i++)
			COUT << *obj._elementi1[i] << " " << *obj._elementi2[i] << endl;
		return COUT;
	}

	void Sortiraj(Sortiranje tip) {
		bool prolaz = true;
		int vel = _trenutno - 1;
		while (prolaz) {
			prolaz = false;
			for (int i = 0; i < vel; i++) {
				if ((tip == RASTUCI && *_elementi1[i] > *_elementi1[i + 1]) || (tip == OPADAJUCI && *_elementi1[i] < *_elementi1[i + 1])) {
					/*T1* temp1 = _elementi1[i];
					T2* temp2 = _elementi2[i];
					_elementi1[i] = _elementi1[i + 1];
					_elementi2[i] = _elementi2[i + 1];
					_elementi1[i + 1] = temp1;
					_elementi2[i + 1] = temp2;*/
					swap(_elementi1[i], _elementi1[i + 1]);
					swap(_elementi2[i], _elementi2[i + 1]);

					prolaz = true;
				}
			}
			vel--;
		}
	}

};

class Viza {
protected:
	Drzava _drzava;
	Datum _vazenjeOD;
	Datum _vazenjeDO;
	char * _napomena;
	// string se odnosi na razlog promjene statusa, a moguce je samo 10 izmjena statusa
	FITKolekcija<Status, string, 10> _statusi;
	void Kopiraj(const Viza& original) {
		_drzava = original._drzava;
		_vazenjeDO = original._vazenjeDO;
		_vazenjeOD = original._vazenjeOD;
		_napomena = AlocirajNizKaraktera(original._napomena);
		_statusi = original._statusi;
	}
public:
	Viza(Drzava drzava, Datum _OD, Datum _DO, char * napomena = nullptr) :_vazenjeDO(_DO), _vazenjeOD(_OD) {
		_napomena = AlocirajNizKaraktera(napomena);
		_drzava = drzava;
	}

	Viza(const Viza& original) { Kopiraj(original); }

	Viza& operator=(const Viza& original) {
		if (this != &original) {
			delete[] _napomena;
			Kopiraj(original);
		}
		return *this;
	}

	virtual ~Viza() { delete[] _napomena; _napomena = nullptr; }

	Datum& GetVazenjeOD() { return _vazenjeOD; }

	Datum& GetVazenjeDO() { return _vazenjeDO; }

	char * GetNapomena() { return _napomena; }

	FITKolekcija<Status, string, 10> & GetStatuse() { return _statusi; }

	virtual void Info() = 0;

	Drzava GetDrzava() const { return _drzava; }

	bool operator==(Viza& viza) {
		if (viza._drzava == _drzava && _vazenjeOD == viza._vazenjeOD && _vazenjeDO == viza._vazenjeDO)
			return true;
		return false;
	}

	void SetNapomena(string napomena) { delete[] _napomena; _napomena = AlocirajNizKaraktera(napomena.c_str()); }


};

class PoslovnaViza :public Viza {
	string _kompanija; //naziv kompanije koja potvrduje da posluje sa aplikantom
public:
	PoslovnaViza(Drzava drzava, Datum _OD, Datum _DO, string kompanija, char * napomena = nullptr) : Viza(drzava, _OD, _DO, napomena) {
		_kompanija = kompanija;
	}

	PoslovnaViza(const PoslovnaViza& original) :Viza(original) {
		_kompanija = original._kompanija;
	}

	PoslovnaViza& operator=(const PoslovnaViza& original) {
		if (this != &original) {
			(*(Viza*)(this)) = original;
			_kompanija = original._kompanija;
		}
		return *this;
	}

	friend ostream & operator << (ostream & COUT, PoslovnaViza & obj) {
		COUT << charDrzave[obj._drzava] << " " << obj._vazenjeOD << " " << obj._vazenjeDO << " " << obj._kompanija << endl;
		COUT << ((obj._napomena != nullptr) ? obj._napomena : "") << endl;
		COUT << "STATUSI -> " << endl;
		for (int i = 0; i < obj._statusi.GetTrenutno(); i++)
			cout << charStatusi[*obj._statusi.GetT1()[i]] << endl;
		return COUT;

	}

	void Info() { cout << *this << crt; }
};

class TuristickaViza : public Viza {
	string _adresa; //adresa hotele ili drugog smještaja na kojoj ce aplikant boraviti
public:
	TuristickaViza(Drzava drzava, Datum _OD, Datum _DO, string adresa, char * napomena = nullptr) : Viza(drzava, _OD, _DO, napomena) {
		_adresa = adresa;
	}

	TuristickaViza(const TuristickaViza& orignal) :Viza(orignal) {
		_adresa = orignal._adresa;
	}

	TuristickaViza& operator=(const TuristickaViza& original) {
		if (this != &original) {
			(*(Viza*)(this)) = original;
			_adresa = original._adresa;
		}
		return *this;
	}

	friend ostream & operator << (ostream & COUT, TuristickaViza & obj) {
		COUT << charDrzave[obj._drzava] << " " << obj._vazenjeOD << " " << obj._vazenjeDO << " " << obj._adresa << endl;
		COUT << ((obj._napomena != nullptr) ? obj._napomena : "") << endl;
		COUT << "STATUSI -> " << endl;
		for (int i = 0; i < obj._statusi.GetTrenutno(); i++)
			cout << charStatusi[*obj._statusi.GetT1()[i]] << endl;

		return COUT;

	}

	void Info() { cout << *this << crt; }
};

mutex cuvar;

class Aplikant {
	char * _imePrezime;
	Datum * _datumRodjenja;
	char * _brojPasosa;
	vector<Viza *> _vize;
	void Kopiraj(const Aplikant& original) {
		_imePrezime = AlocirajNizKaraktera(original._imePrezime);
		_datumRodjenja = new Datum(*original._datumRodjenja);
		_brojPasosa = AlocirajNizKaraktera(original._brojPasosa);
		for (size_t i = 0; i < original._vize.size(); i++) {
			PoslovnaViza* poslovna = dynamic_cast<PoslovnaViza*>(original._vize[i]);
			if (poslovna != nullptr)
				_vize.push_back(new PoslovnaViza(*poslovna));
			TuristickaViza* turisticka = dynamic_cast<TuristickaViza*>(original._vize[i]);
			if (turisticka != nullptr)
				_vize.push_back(new TuristickaViza(*turisticka));
		}
	}
public:
	Aplikant(const char * imePrezime, Datum datumRodjenja, const char * brPasosa) {
		_imePrezime = AlocirajNizKaraktera(imePrezime);
		_brojPasosa = AlocirajNizKaraktera(brPasosa);
		_datumRodjenja = new Datum(datumRodjenja);
	}

	Aplikant(const Aplikant& original) { Kopiraj(original); }

	Aplikant& operator=(const Aplikant& original) {
		if (this != &original) {
			delete[]_imePrezime; _imePrezime = nullptr;
			delete[]_brojPasosa; _brojPasosa = nullptr;
			delete _datumRodjenja; _datumRodjenja = nullptr;
			for (size_t i = 0; i < _vize.size(); i++) {
				delete _vize[i]; _vize[i] = nullptr;
			}
			Kopiraj(original);
		}
		return *this;
	}

	~Aplikant() {
		delete[]_imePrezime; _imePrezime = nullptr;
		delete[]_brojPasosa; _brojPasosa = nullptr;
		delete _datumRodjenja; _datumRodjenja = nullptr;
		for (size_t i = 0; i < _vize.size(); i++)
		{
			delete _vize[i]; _vize[i] = nullptr;
		}
	}

	friend ostream & operator << (ostream & COUT, Aplikant & obj) {
		COUT << obj._imePrezime << " " << *obj._datumRodjenja << " " << obj._brojPasosa << endl;
		COUT << "VIZE -> " << endl;
		for (size_t i = 0; i < obj._vize.size(); i++)
			obj._vize[i]->Info();
		return COUT;

	}

	vector<Viza * > GetVize() { return _vize; }

	bool ProvjeriGodine(Datum& datum) { return (datum - *_datumRodjenja) > 10; }

	bool PostojiPonistena() {
		for (int i = 0; i < _vize.size(); i++) {
			if (_vize[i]->GetDrzava() == SAD) {
				for (int j = 0; j < _vize[i]->GetStatuse().GetTrenutno(); j++) {
					if (*_vize[i]->GetStatuse().GetT1()[j] == PONISTENA)
						return true;
				}
			}
		}
		return false;
	}

	bool Provjera(Viza* viza) {
		if (!ProvjeriGodine(const_cast<Viza*>(viza)->GetVazenjeOD()))
			return false;
		if (viza->GetDrzava() == SAD && PostojiPonistena())
			return false;
		for (int i = 0; i < _vize.size(); i++) {
			if (_vize[i]->GetDrzava() == viza->GetDrzava() && *_vize[i]->GetStatuse().GetT1()[_vize[i]->GetStatuse().GetTrenutno() - 1] == IZDATA) {
				if ((viza->GetVazenjeOD() > _vize[i]->GetVazenjeOD() && viza->GetVazenjeOD() < _vize[i]->GetVazenjeDO()) ||
					viza->GetVazenjeDO() > _vize[i]->GetVazenjeOD() && viza->GetVazenjeDO() < viza->GetVazenjeDO())
					return false;
			}
		}
		return true;
	}

	bool DodajVizu(Viza* viza) {
		if (Provjera(viza) == false)
			return false;
		PoslovnaViza* poslovna = dynamic_cast<PoslovnaViza*>(viza);
		TuristickaViza* turisticka = dynamic_cast<TuristickaViza*>(viza);
		if (poslovna != nullptr)
			_vize.push_back(new PoslovnaViza(*poslovna));
		if (turisticka != nullptr)
			_vize.push_back(new TuristickaViza(*turisticka));
		return true;
	}

	bool ProvjeriStatuse(Status status, Viza* viza) {
		if (viza == nullptr)
			return false;
		if (viza->GetStatuse().GetTrenutno() == 0)
			return true;
		if (*viza->GetStatuse().GetT1()[viza->GetStatuse().GetTrenutno() - 1] < status)
			return true;
		return false;

	}

	int NadjiVizu(Viza* viza) {
		for (int i = 0; i < _vize.size(); i++)
			if (*viza == *_vize[i])
				return i;
		return -1;
	}

	bool DodajStatus(Viza* viza, Status status, string razlog = " ") {
		int indeks = NadjiVizu(viza);
		if (indeks == -1 || !ProvjeriStatuse(status, viza))
			return false;
		_vize[indeks]->GetStatuse().Dodaj(status, razlog);
		_vize[indeks]->SetNapomena(razlog);
		return true;
	}

	void Ispisi(Viza* viza) {
		int indeks = NadjiVizu(viza);
		if (indeks == -1)
			return;
		cuvar.lock();
		this_thread::sleep_for(chrono::seconds(2));
		_vize[indeks]->Info();
		cuvar.unlock();
	}

	void PrintajVizu(Viza* viza) {
		thread temp(&Aplikant::Ispisi,this,viza);
		temp.join();
	}

	vector<Viza*> GetVizeByDrzava(Drzava drzava, Status status) {
		vector<Viza*> temp;
		for (int i = 0; i < _vize.size(); i++) {
			if (_vize[i]->GetDrzava() == drzava) {
				for (int j = 0; j < _vize[i]->GetStatuse().GetTrenutno(); j++) {
					if (*_vize[i]->GetStatuse().GetT1()[i] == status)
						temp.push_back(_vize[i]);
				}
			}
		}
		return temp;
	}

	void ZamijeniRijec(string trazena, string zamjena) {
		regex reg(trazena);
		for (int i = 0; i < _vize.size(); i++) {
			_vize[i]->SetNapomena(regex_replace(_vize[i]->GetNapomena(), reg, zamjena));
		}

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



	Datum danas(15, 2, 2017);
	Datum sutra(++danas); // inkrementiranjem se uvecava dan za 1
	Datum prekosutra;
	Datum dat15022017(15, 2, 2017), dat18082017(18, 8, 2017), dat11052018(11, 5, 2018), dat11122017(11, 12, 2017);


	prekosutra = danas;
	if (prekosutra == danas)
		cout << "Datumi su jednaki" << endl;
	cout << danas << endl << sutra << endl << prekosutra << crt;

	int v6 = 6, v13 = 13, v32 = 32, v63 = 63, v98 = 98, v109 = 109, v196 = 196;

	FITKolekcija<int, int, 7> brojevi;
	brojevi.Dodaj(v196, v6);
	brojevi.Dodaj(v13, v32);
	brojevi.Dodaj(v98, v196);
	brojevi.Dodaj(v63, v13);
	brojevi.Dodaj(v98, v196);
	brojevi.Dodaj(v6, v63);
	brojevi.Dodaj(v196, v6);

	cout << brojevi << crt;
	//NA KOJI NACIN CE SE VRŠITI SORTIRANJE PRVENSTVENO ZAVISI OD VRIJEDNOSTI PARAMETRA, A VRŠI SE NA OSNOVU VRIJEDNOSTI TIPA T1
	brojevi.Sortiraj(RASTUCI);
	cout << brojevi << crt;
	brojevi.Sortiraj(OPADAJUCI);
	cout << brojevi << crt;

	try
	{
		//U SLUCAJU PREKORACENJA DEFINISANE VELICINE KOLEKCIJE, POTREBNO JE BACITI IZUZETAK
		brojevi.Dodaj(v13, v196);
	}
	catch (exception & err)
	{
		cout << "Error -> " << err.what() << endl;
	}

	Aplikant jasmin("Jasmin Azemovic", Datum(15, 5, 1979), "BS6985645");
	PoslovnaViza poslovna(SAD, dat15022017, dat18082017, "Microsoft");

	/*
	NOVA VIZA SE NE MOŽE DODATI UKOLIKO:
	-	JE APLIKANT MLAÐI OD 10 GODINA
	-	U TOM PERIODU (ODNOSI SE NA PERIOD VIZE OD - DO) NE POSTOJI VAŽECA (POJAM VAŽECA PODRAZUMIJEVA DA JE POSLJEDNJI
		STATUS VIZE BIO IZDATA TJ. DA VIZA NIJE PONIŠTENA ILI ISTEKLA) VIZA ZA TU DRŽAVU, BEZ OBZIRA NA NJENU VRSTU.
	-	JE NEKA OD RANIJIH VIZA PONIŠTENA (OVO SE ODNOSI SAMO NA VIZE ZA DRŽAVU: SAD)
	*/

	if (jasmin.DodajVizu(&poslovna))
		cout << "Podaci o vizi uspješno zaprimljeni" << endl;
	/*
	STATUSI SE MOGU DODAVATI SAMO U REDOSLIJEDU U KOJEM SE NALAZE U ENUMERACIJI NPR.
	-	NIJE MOGUCE ODBITI VIZU AKO PRETHODNO NIJE IMALA STATUS DA JE ZAHTJEV_ZAPRIMLJEN;
	-	NIJE MOGUCE PONIŠTITI VIZU KOJA PRETHODNO NIJE IZDATA
	*/

	if (jasmin.DodajStatus(&poslovna, ZAHTJEV_ZAPRIMLJEN, "provjeriti navode sa predstavnicima kompanije Microsoft"))
		cout << "Status vize uspjesno promijenjen" << endl;
	else
		cout << "Aplikant " << jasmin << " ne posjeduje evidentiranu trazenu vizu ili status vize nema odgovarajucu vrijednost" << endl;


	if (jasmin.DodajStatus(&poslovna, PONISTENA, "predstavnicima kompanije Microsoft nemaju informaciju o posjeti navedenog aplikanta"))
		cout << "Status vize uspjesno promijenjen" << endl;
	else
		cout << "Aplikant " << jasmin << " ne posjeduje evidentiranu trazenu vizu ili status vize nema odgovarajucu vrijednost" << endl;

	TuristickaViza turisticka(ENGLESKA, dat18082017, dat11122017, "Whitehall Pl, London SW1A 2BD, UK");

	if (jasmin.DodajVizu(&turisticka)) //dodano zbog testiranja
		cout << "Podaci o vizi uspješno zaprimljeni" << endl;


	if (jasmin.DodajStatus(&turisticka, ZAHTJEV_ZAPRIMLJEN))
		cout << "Status vize uspjesno promijenjen" << endl;
	else
		cout << "Aplikant " << jasmin << " ne posjeduje evidentiranu trazenu vizu ili status vize nema odgovarajucu vrijednost" << endl;

	if (jasmin.DodajStatus(&turisticka, IZDATA)) {
		cout << "Status vize uspjesno promijenjen" << endl;
		//U ZASEBNOJ NITI URADITI PRINTANJE VIZE...KORISTITI METODU SLEEP_FOR
		jasmin.PrintajVizu(&turisticka);
	}
	else
		cout << "Aplikant " << jasmin << " ne posjeduje evidentiranu trazenu vizu ili status vize nema odgovarajucu vrijednost" << endl;

	//VRACA PODATKE O SVIM VIZAMA KOJE SU IZDATE ZA ODREÐENU DRŽAVU, A KOJE SU IMALE ODREÐENI STATUS
	vector<Viza *> jasminVize = jasmin.GetVizeByDrzava(SAD, PONISTENA);

	//UNUTAR NAPOMENE MIJENJA SADRŽAJ "nemaju informaciju" SA "su upoznti sa" - KORISTITI REGEX

	jasmin.ZamijeniRijec("nemaju informaciju", "su upoznati sa");

	system("Pause>0");
}
