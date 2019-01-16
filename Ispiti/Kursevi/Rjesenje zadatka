#include<iostream>
#include<string>
#include<vector>
#include<regex>
#include<exception>
#include<ctime>
using namespace std;

char* AlocirajNizKaraktera(const char* sadrzaj) {
	if (sadrzaj == nullptr)
		return nullptr;
	int vel = strlen(sadrzaj) + 1;
	char* temp = new char[vel];
	strcpy_s(temp, vel, sadrzaj);
	return temp;
}


class Datum{
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

	friend ostream& operator<< (ostream &COUT,const Datum &obj) {
		COUT << *obj._dan << " " << *obj._mjesec << " " << *obj._godina;
		return COUT;
	}

	friend bool operator==(const Datum&, const Datum&);

	bool DaLiZadovoljava(const Datum& pocetak, const Datum& kraj) {
		int trenutni = *_godina * 10000 + *_mjesec * 100 + *_dan;
		int poc= *pocetak._godina * 10000 + *pocetak._mjesec * 100 + *pocetak._dan;
		int kr= *kraj._godina * 10000 + *kraj._mjesec * 100 + *kraj._dan;
		if (trenutni >= poc && trenutni <=kr)
			return false;
		return true;
	}
};
bool operator==(const Datum& d1, const Datum& d2) {
	if ((*d1._dan != *d2._dan) || (*d1._mjesec != *d2._mjesec) || (*d1._godina != *d2._godina))
		return false;
	return true;
}



class Izuzetak :public exception {
	Datum _datum;//KORISTITE DANASNJI DATUM
	string _funkcija;//FUNKCIJA U KOJOJ JE NASTAO IZUZETAK
	int _linija;//LINIJA CODE-A U KOJOJ JE NASTAO IZUZETAK
public:
	Izuzetak(string funkcija, int linija,const char* poruka):exception(poruka){
		time_t trenutno = time(0);
		tm lokaltrenutno;
		localtime_s(&lokaltrenutno, &trenutno);
		Datum date(lokaltrenutno.tm_mday, lokaltrenutno.tm_mon, lokaltrenutno.tm_year);
		_datum = date;
		_funkcija = funkcija;
		_linija = linija;
	}

	friend ostream& operator<<(ostream& ispis,  const Izuzetak& izuzetak) {
		ispis << "DATUM: " << izuzetak._datum << endl;
		ispis << "FUNKCIJA: " << izuzetak._funkcija<< endl;
		ispis << "LINIJA: " << izuzetak._linija << endl;
		ispis << "PORUKA: " << izuzetak.what()<< endl;
		return ispis;
	}
};

template<class T1, class T2, int max>
class Kolekcija {
	T1 _elementi1[max];
	T2 _elementi2[max];
	int * _trenutno;
	void Kopiraj(const Kolekcija<T1, T2, max>& original) {
		_trenutno = new int(*original._trenutno);
		for(int i = 0; i < *_trenutno; i++){
			_elementi1[i] = original._elementi1[i];
			_elementi2[i] = original._elementi2[i];
		}
	}
public:
	Kolekcija() { _trenutno = new int(0); }

	Kolekcija(const Kolekcija<T1, T2, max>& original) {	Kopiraj(original);}

	Kolekcija<T1, T2, max>& operator=(const Kolekcija<T1, T2, max>& original) {
		if (this != &original) {
			delete _trenutno;
			Kopiraj(original);
		}
		return *this;
	}

	~Kolekcija() { delete _trenutno; _trenutno = nullptr;}

	void AddElement(const T1& obj1, const T2& obj2) {
		if (*_trenutno == max)
			throw Izuzetak(__FUNCTION__, __LINE__,"Greska ! Kolekcija je puna.");
		_elementi1[*_trenutno] = obj1;
		_elementi2[*_trenutno] = obj2;
		(*_trenutno)++;
	}

	T1& GetElement1(int i){
		if (i < 0 || i >= *_trenutno)
			throw Izuzetak(__FUNCTION__, __LINE__,"Nespotojeca lokacija");
		return _elementi1[i];
	}

	T2& GetElement2(int i) {
		if (i < 0 || i >= *_trenutno)
			throw Izuzetak( __FUNCTION__, __LINE__,"Nepostojeca lokacija");
		return _elementi2[i];
	}

	int GetMax() { return max; }

	int GetTrenutno() { return *_trenutno; }

	friend ostream& operator<<(ostream& ispis, const Kolekcija& kolekcija) {
		for (int i = 0; i < *kolekcija._trenutno; i++)
			ispis << kolekcija._elementi1[i] << "  " << kolekcija._elementi2[i] << endl;
		return ispis;
	}

	int size() { return GetTrenutno(); }

	T1& operator[](int i) { return GetElement1(i); }

	friend bool operator==(const Kolekcija& k1, const Kolekcija& k2) {
		if (*k1._trenutno != *k2._trenutno)
			return false;
		int brojac = 0;
		for (int i = 0; i < *k1._trenutno; i++){
			for (int j = 0; j < *k1._trenutno; j++){
				if (k1._elementi1[i] == k2._elementi1[j]) {
					brojac++;
					break;
				}
			}
		}
		if (brojac == *k1._trenutno)
			return true;
		return false;
	}
};


enum enumKursevi { HtmlCSSJavaScript, SoftwareEngeneeringFundamentals, MasteringSQL, WindowsSecurity };
const char *charKursevi[]= { "HtmlCSSJavaScript", "SoftwareEngeneeringFundamentals", "MasteringSQL", "WindowsSecurity "};

class Kurs {
	enumKursevi _kurs;
	Datum _pocetak;
	Datum _kraj;
	char * _imePredavaca;
	void Kopiraj(const Kurs& original) {
		_kurs = original._kurs;
		_pocetak = original._pocetak;
		_kraj = original._kraj;
		_imePredavaca = AlocirajNizKaraktera(original._imePredavaca);
	}
public:
	Kurs(enumKursevi kurs,const char* imePredavaca,const Datum& pocetak, const Datum& kraj){
		_kurs = kurs;
		_pocetak = pocetak;
		_kraj = kraj;
		_imePredavaca = AlocirajNizKaraktera(imePredavaca);
	}

	Kurs():_kurs(enumKursevi(0)),_imePredavaca(nullptr){}

	Kurs(const Kurs& original) { Kopiraj(original); }
	
	Kurs& operator=(const Kurs& original) {
		if (this != &original) {
			delete[] _imePredavaca;
			Kopiraj(original);
		}
		return *this;

	}

	~Kurs() { delete[] _imePredavaca; _imePredavaca = nullptr; }

	friend ostream& operator<<(ostream& ispis, const Kurs& kurs) {
		ispis << "Kurs: " << charKursevi[kurs._kurs] << endl;
		ispis << "Datum pocetka: " << kurs._pocetak<<endl;
		ispis << "Datum zavrsetka: " << kurs._kraj<<endl;
		ispis << "Ime predavaca: " << kurs._imePredavaca << endl;
		return ispis;
	}

	friend bool operator==(const Kurs&, const Kurs&);

	bool DaLiZadovoljava(const Datum& pocetak, const Datum& kraj) {
		if (_pocetak.DaLiZadovoljava(pocetak, kraj))
			return true;
		return false;
	}

	Datum GetPocetak()const { return _pocetak; }
	
	Datum GetKraj()const { return _kraj; }

	enumKursevi GetKurs() { return _kurs; }

	char* GetImePredavaca() { return _imePredavaca; }

};
bool operator==(const Kurs& k1, const Kurs& k2) {
	if ((k1._kurs == k2._kurs) && (k1._pocetak == k2._pocetak) && (k1._kraj == k2._kraj) && (strcmp(k1._imePredavaca, k2._imePredavaca) == 0))
		return true;
	return false;
}

bool ProvjeriFormatTelefona(string telefon){
	string pravilo = "\\(06\\d\\)\\d{3}\\-\\d{3}";
	if (regex_match(telefon, regex(pravilo)))
		return true;
	return false;
}
class Polaznik {
	static int ID;
	const int _polaznikID;
	//SVAKOM NOVOM POLAZNIKU AUTOMATSKI DODIJELITI NOVI ID (AUTOINCREMENT) POCEVSI OD BROJA 1
	char * _imePrezime;
	string _kontaktTelefon; //BROJ TELEFONA TREBA BITI U FORMATU (06X)XXX-XXX 
	Kolekcija<Kurs *, int, 10> _uspjesnoOkoncaniKursevi;
	//INT PREDSTAVLJA OSTVARENI PROCENAT NA ISPITU, A JEDAN POLAZNIK MOZE POLOZITI NAJVISE 10 KURSEVA
public:
	Polaznik(const char * imePrezime, string telefon) :_polaznikID(ID++) {
		_imePrezime = AlocirajNizKaraktera(imePrezime);
		_kontaktTelefon = telefon;
	}
	
	Polaznik(const Polaznik& original):_polaznikID(original._polaznikID){
		_imePrezime = AlocirajNizKaraktera(original._imePrezime);
		_kontaktTelefon = original._kontaktTelefon;
		for (int i = 0; i < const_cast<Polaznik*>(&original)->_uspjesnoOkoncaniKursevi.GetTrenutno(); i++)
			_uspjesnoOkoncaniKursevi.AddElement(new Kurs(*const_cast<Polaznik*>(&original)->_uspjesnoOkoncaniKursevi.GetElement1(i)),
			_uspjesnoOkoncaniKursevi.GetElement2(i));
	}

	Polaznik& operator=(Polaznik& original) {
		if (this != &original) {
delete[] _imePrezime;
for (int i = 0; i < _uspjesnoOkoncaniKursevi.GetTrenutno(); i++)
	delete _uspjesnoOkoncaniKursevi.GetElement1(i);
_imePrezime = AlocirajNizKaraktera(original._imePrezime);
_kontaktTelefon = _kontaktTelefon;
for (int i = 0; i < original._uspjesnoOkoncaniKursevi.GetTrenutno(); i++)
	_uspjesnoOkoncaniKursevi.AddElement(new Kurs(*original._uspjesnoOkoncaniKursevi.GetElement1(i)),
		_uspjesnoOkoncaniKursevi.GetElement2(i));
		}
		return *this;
	}

	~Polaznik() {
		delete[] _imePrezime; _imePrezime = nullptr;
		for (size_t i = 0; i < _uspjesnoOkoncaniKursevi.size(); i++)
		{
			delete _uspjesnoOkoncaniKursevi[i];
			_uspjesnoOkoncaniKursevi[i] = nullptr;
		}
	}

	int GetID() const { return _polaznikID; }

	friend ostream& operator<<(ostream& ispis, const Polaznik& polaznik) {
		ispis << endl << "Polaznik ID: " << polaznik._polaznikID << endl;
		ispis << "Ime i prezime: " << polaznik._imePrezime << endl;
		ispis << "Kontakt telefon: " << polaznik._kontaktTelefon << endl;
		ispis << polaznik._uspjesnoOkoncaniKursevi << endl;
		return ispis;
	}

	friend bool operator==(const Polaznik& p1, const Polaznik& p2) {
		if (strcmp(p1._imePrezime, p2._imePrezime) != 0)
			return false;
		if (p1._uspjesnoOkoncaniKursevi == p2._uspjesnoOkoncaniKursevi)
			return true;
		return false;
	}

	Kolekcija<Kurs*, int, 10> GetUspjesnoOkoncaneKurseve() { return _uspjesnoOkoncaniKursevi; }

	int GetProcenat(enumKursevi kurs) {
		for (int i = 0; i < _uspjesnoOkoncaniKursevi.GetTrenutno(); i++)
			if (_uspjesnoOkoncaniKursevi.GetElement1(i)->GetKurs() == kurs)
				return _uspjesnoOkoncaniKursevi.GetElement2(i);
		return 0;
	}
};
int Polaznik::ID = 1;

class SkillsCentar {
	string _nazivCentra;
	vector<Kurs> _kursevi;//KURSEVI KOJE NUDI ODREDJENI CENTAR
	Kolekcija<Kurs, Polaznik *, 150> _aplikanti;
public:
	SkillsCentar(string naziv) { _nazivCentra = naziv; }

	SkillsCentar(const SkillsCentar & obj) :_kursevi(obj._kursevi), _aplikanti(obj._aplikanti) {
		_nazivCentra = obj._nazivCentra;
	}

	int ImaLiIsti(const Kurs& kurs) {
		for (int i = 0; i < _kursevi.size(); i++)
			if (_kursevi[i] == kurs)
				return i;
		return -1;
	}

	bool ProvjeriKoliziju(const Kurs& kurs) {
		for (int i = 0; i < _kursevi.size(); i++)
			if (!_kursevi[i].DaLiZadovoljava(kurs.GetPocetak(), kurs.GetKraj()))
				return false;
		if (ImaLiIsti(kurs)!=-1)
			throw Izuzetak(__FUNCTION__, __LINE__, "GRESKA. Vec postoji taj kurs.");
		return true;
	}

	void AddKurs(const Kurs& kurs) {
		if (!ProvjeriKoliziju(kurs))
			return;
		_kursevi.push_back(kurs);
	}

	int ImaLiPolaznik(const Kurs& kurs, int id) {
		for (int i = 0; i < _aplikanti.GetTrenutno(); i++)
			if (_aplikanti.GetElement1(i) == kurs && _aplikanti.GetElement2(i)->GetID() == id)
				return i;
		return -1;
	}

	void AddAplikaciju(const Kurs& kurs,  Polaznik& polaznik) {
		if (ImaLiIsti(kurs)==-1)
			throw Izuzetak(__FUNCTION__, __LINE__, "GRESKA. Taj kurs ne postoji u nasem skills centru.");
		if (ImaLiPolaznik(kurs, polaznik.GetID()) != -1)
			throw Izuzetak(__FUNCTION__, __LINE__, "GRESKA. Polaznik vec pohadja taj kurs.");
		_aplikanti.AddElement(kurs, &polaznik);
	}

	void DodajUspjesnoOkoncanKurs(int id, const Kurs& kurs, int procenat) {
		int indeks = ImaLiPolaznik(kurs, id);
		if (indeks != -1 && procenat > 55)
			_aplikanti.GetElement2(indeks)->GetUspjesnoOkoncaneKurseve().AddElement(new Kurs(kurs), procenat);
	}

	vector<Polaznik> GetPolazniciByPredavac(const char* imePredavaca, enumKursevi kurs) {
		vector<Polaznik> polaznici;
		for (int i = 0; i < _aplikanti.size(); i++) {
			if (_aplikanti.GetElement1(i).GetKurs() == kurs && strcmp(_aplikanti.GetElement1(i).GetImePredavaca(), imePredavaca) == 0 &&
				_aplikanti.GetElement2(i)->GetProcenat(kurs) > 55)
				polaznici.push_back(*_aplikanti.GetElement2(i));
		}
		return polaznici;
	}

	bool IsKursPrazan(const Kurs& kurs) {
		for (int i = 0; i < _aplikanti.size(); i++){
			if (_aplikanti.GetElement1(i) == kurs && _aplikanti.GetTrenutno() != 0)
				return false;
		}
		return true;
	}

	bool RemoveKurs(const Kurs& kurs) {
		int indeks = ImaLiIsti(kurs);
		
		if (indeks!=-1 && !IsKursPrazan(kurs))
			return false;
		_kursevi.erase(_kursevi.begin()+indeks);
		return true;
	}

	friend ostream& operator<<(ostream& ispis,  SkillsCentar& centar) {
		ispis << endl << "Naziv centra: " << centar._nazivCentra << endl<<endl<<"KURSEVI: "<<endl<<endl;
		for (int i = 0; i < centar._kursevi.size(); i++)
			ispis << centar._kursevi[i] << endl;
		for (int i = 0; i < centar._aplikanti.size(); i++){
			ispis << charKursevi[centar._aplikanti.GetElement1(i).GetKurs()] << " -> " << endl << endl;
			ispis << *centar._aplikanti.GetElement2(i) << endl << endl;
		}
		return ispis;
	}
};
const char *crt = "\n---------------------------------------\n";
int main()
{
	/****************************************************************************
	1. SVE KLASE TREBAJU POSJEDOVATI ADEKVATAN DESTRUKTOR
	2. NAMJERNO IZOSTAVLJANJE KOMPLETNIH I/ILI POJEDINIH DIJELOVA DESTRUKTORA KOJI UZROKUJU RUNTIME ERROR CE BITI OZNACENO KAO "RE"
	3. SPAŠAVAJTE PROJEKAT KAKO BI SE SPRIJECILO GUBLJENJE URAÐENOG ZADATKA
	4. PROGRAMSKI CODE SE TAKOÐER NALAZI U FAJLU CODE.TXT
	5. NAZIVI FUNKCIJA, TE BROJ I TIP PARAMETARA MORAJU BITI IDENTICNI ONIMA KOJI SU KORIŠTENI U TESTNOM CODE-U, OSIM U SLUCAJU DA POSTOJI ADEKVATAN RAZLOG ZA NJIHOVU MODIFIKACIJU. OSTALE, POMOCNE FUNKCIJE MOŽETE IMENOVATI I DODAVATI PO ŽELJI.
	6. IZUZETAK BACITE U FUNKCIJAMA U KOJIMA JE TO NAZNACENO.
	****************************************************************************/

#pragma region TestiranjeDatuma
	Datum danas(26, 11, 2015);
	Datum sutra(danas);
	Datum prekosutra;
	prekosutra = danas;
	cout << danas << endl << sutra << endl << prekosutra << crt;
#pragma endregion
#pragma region TestiranjeKolekcije
	Kolekcija<int, int, 10> kolekcija1;
	try
	{
		for (size_t i = 0; i < kolekcija1.GetMax(); i++)
			kolekcija1.AddElement(i, i + 2);
		//FUNKCIJA TREBA BACITI IZUZETAK U SLUCAJ DA NEMA VISE PROSTORA ZA DODAVANJE NOVIH ELEMENATA
		cout << kolekcija1.GetElement1(0) << " " << kolekcija1.GetElement2(0) << endl;
		cout << kolekcija1.GetElement1(20) << " " << kolekcija1.GetElement2(20) << endl;
		//FUNKCIJA TREBA DA BACI IZUZETAK UKOLIKO ELEMENT NA ZAHTIJEVANOJ LOKACIJI NE POSTOJI
	}
	catch (Izuzetak & err)
	{
		cout << "Greska -> " << err << endl;
	}

	cout << kolekcija1 << endl;
	Kolekcija<int, int, 10> kolekcija2;
	kolekcija2 = kolekcija1;
	cout << kolekcija2 << crt;
	if (kolekcija1.GetTrenutno() == kolekcija2.GetTrenutno())
		cout << "ISTI BROJ ELEMENATA" << endl;
	Kolekcija<int, int, 10> kolekcija3(kolekcija2);
	cout << kolekcija3 << crt;
#pragma endregion
#pragma region TestiranjeKursa
	Kurs sef(SoftwareEngeneeringFundamentals, "Jasmin Azemovic", Datum(28, 1, 2016), Datum(15, 2, 2016));
	cout << sef << endl;
	Kurs msql(MasteringSQL, "Adel Handzic", Datum(28, 2, 2016), Datum(15, 3, 2016));
	//sef = msql;
	if (sef == msql)//KURSEVI SU ISTI AKO POSJEDUJU IDENTICNE VRIJEDNOSTI SVIH ATRIBUTA
		cout << "ISTI SU KURSEVI" << endl;
	Kurs msql2(msql);
	cout << msql2 << endl;
#pragma endregion
#pragma region TestiranjePolaznika
	string telefon1 = "(061)111-111";
	string telefon2 = "(061)111222";
	string telefon3 = "(061)111-333";

	if (!ProvjeriFormatTelefona(telefon1))
		cout << "Broj " << telefon1 << " nije validan" << endl;
	if (!ProvjeriFormatTelefona(telefon2))
		cout << "Broj " << telefon2 << " nije validan" << endl;
	if (!ProvjeriFormatTelefona(telefon3))
		cout << "Broj " << telefon3 << " nije validan" << endl;


	Polaznik denis("Denis Music", telefon1);
	Polaznik denis2("Denis2 Music2", telefon3);

#pragma endregion

#pragma region TestiranjeCentra
	try {
		SkillsCentar mostar("Skills Center Mostar");
		if (mostar.ProvjeriKoliziju(sef))
			//JEDAN KURS NE SMIJE POCETI DOK DRUGI TRAJE TJ. VRIJEME ODRZAVANJA KURSA SE NE SMIJE POKLAPATI
			mostar.AddKurs(sef);
		if (mostar.ProvjeriKoliziju(msql))
			mostar.AddKurs(msql);
		if (mostar.ProvjeriKoliziju(msql2))
			mostar.AddKurs(msql2);
		//ONEMOGUCITI DODAVANJE IDENTICNIH KURSEVA. FUNKCIJA TREBA DA BACI IZUZETAK UKOLIKO SE POKUSA DODATI IDENTICAN KURS
		mostar.AddAplikaciju(sef, denis);
		//ONEMOGUCITI APLICIRANJE ZA KURSEVE KOJI NISU REGISTROVANI U CENTRU
		mostar.AddAplikaciju(msql, denis);
		//mostar.AddAplikaciju(msql, denis);
		//ONEMOGUCITI APLICIRANJE ZA ISTI KURS. FUNKCIJA BAZA IZUZETAK
		cout << denis2 << endl;
		denis2 = denis;
		if (denis == denis2)
			//POLAZNICI SU ISTI AKO POSJEDUJU ISTO IME I AKO SU POHADJALI ISTE KURSEVE
			cout << "ISTI SU" << endl;

		mostar.DodajUspjesnoOkoncanKurs(1, sef, 60);
		//BROJ 1 OZNACAVA ID POLAZNIKA. FUNKCIJA JE ZADUZENA DA POLAZNIKU DODA INFORMACIJU O USPJESNO POLOZENOM KURSU 
		//KOJI JE POSLAN KAO PARAMETAR. PREDUSLOV ZA DODAVANJE JE DA JE POLAZNIK PRETHODNO APLICIRAO ZA TAJ KURS, TE DA 
		//JE NA ISPITU OSTVARIO VISE OD 55%
		mostar.DodajUspjesnoOkoncanKurs(1, msql, 83);
		vector<Polaznik> listaPolaznika = mostar.GetPolazniciByPredavac("Jasmin Azemovic", SoftwareEngeneeringFundamentals);
		//VRACA INFORMACIJE O POLAZNICIMA KOJI SU KOD ODREDJENOG PREDAVACA USPJESNO OKONCALI ODREDJENI KURS

		if (mostar.RemoveKurs(sef))//ONEMOGUCITI UKLANJANJE KURSA KOJI SU VEC PRIJAVILI NEKI OD POLAZNIKA
			cout << "Kurs " << sef << " uspjesno uklonjen iz centra " << mostar << endl;

		SkillsCentar sarajevo = mostar;
		cout << sarajevo << endl;
	}
	catch (Izuzetak & err) {
		cout << err << endl;
	}
#pragma endregion

	system("pause>0");
	return 0;
}
