#include<iostream>
#include<string>
#include<vector>
#include<regex>
#include<sstream>
using namespace std;

char* AlocirajNizKaraktera(const char* sadrzaj) {
	if (sadrzaj == nullptr)
		return nullptr;
	int vel = strlen(sadrzaj) + 1;
	char* temp = new char[vel];
	strcpy_s(temp, vel, sadrzaj);
	return temp;
}

const char *crt = "\n-------------------------------------------\n";
enum stringFormat { DDMMGG, DDMMGGGG, DDMMGGSSMM, DDMMGGGGSSMM };
enum prikazPoruka { SVE_PORUKE, SAKRIJ_POBRISANE, PRIKAZI_SAMO_POBRISANE };
vector<string> nedozvoljeneRijeci;
/*
DDMMGG - 29.8.17
DDMMGGGG - 29.8.2017
DDMMGGGGSSMM - 29.8.2017 10:15
*/

template<class T1, class T2>
class Kolekcija {
	T1 * _elementi1;
	T2 * _elementi2;
	int * _trenutno;
public:
	Kolekcija(){
		_elementi1 = nullptr;
		_elementi2 = nullptr;
		_trenutno = new int(0);
	}

	Kolekcija(const Kolekcija& orig){
		_elementi1 = new T1[*orig._trenutno];
		_elementi2 = new T2[*orig._trenutno];
		*_trenutno = *orig._trenutno;

		for (int i = 0; i < *_trenutno; i++){
			_elementi1[i] = orig._elementi1[i];
			_elementi2[i] = orig._elementi2[i];
		}
	}

	Kolekcija & operator=(Kolekcija<T1, T2> & orig)
	{
		if (this != &orig)
		{
			delete[]_elementi1;
			delete[]_elementi2;

			_elementi1 = new T1[*orig._trenutno];
			_elementi2 = new T2[*orig._trenutno];
			*_trenutno = *orig._trenutno;

			for (int i = 0; i < *_trenutno; i++)
			{
				_elementi1[i] = orig._elementi1[i];
				_elementi2[i] = orig._elementi2[i];
			}
		}
		return *this;
	}

	bool AddElement(T1 elem1, T2 elem2)
	{
		T1 * temp1 = new T1[*_trenutno + 1];
		T2 * temp2 = new T2[*_trenutno + 1];

		for (int i = 0; i < *_trenutno; i++) {
			temp1[i] = _elementi1[i];
			temp2[i] = _elementi2[i];
		}

		delete[] _elementi1;
		delete[] _elementi2;

		_elementi1 = temp1;
		_elementi2 = temp2;

		_elementi1[*_trenutno] = elem1;
		_elementi2[*_trenutno] = elem2;
		(*_trenutno)++;
		return true;
	}
	
	~Kolekcija(){
		delete[]_elementi1; _elementi1 = nullptr;
		delete[]_elementi2; _elementi2 = nullptr;
		delete _trenutno; _trenutno = nullptr;
	}

	T1 * getElementi1Pok() { return _elementi1; }

	T2 * getElementi2Pok() { return _elementi2; }

	T1 & getElement1(int lokacija) { return _elementi1[lokacija]; }

	T2 & getElement2(int lokacija) { return _elementi2[lokacija]; }

	int getTrenutno() { return *_trenutno; }

	int size() { return getTrenutno(); }

	T1& operator[](int i) {
		if (i < 0 || i >= *_trenutno)
			throw exception("Nepostojeca lokacija.");
		return getElement1(i);
	}

	friend ostream & operator<<(ostream & COUT, Kolekcija<T1, T2> & obj){
		for (int i = 0; i < *obj._trenutno; i++){
			COUT << obj.getElement1(i) << " " << obj.getElement2(i) << endl;
		}
		return COUT;
	}
};
class DatumVrijeme {
	int *_dan, *_mjesec, *_godina, *_sati, *_minuti;
public:
	DatumVrijeme(int dan = 1, int mjesec = 1, int godina = 2000, int sati = 0, int minuti = 0) {
		_dan = new int(dan);
		_mjesec = new int(mjesec);
		_godina = new int(godina);
		_sati = new int(sati);
		_minuti = new int(minuti);
	}

	DatumVrijeme(const DatumVrijeme& original){
		_dan = new int(*original._dan);
		_mjesec = new int(*original._mjesec);
		_godina = new int(*original._godina);
		_sati = new int(*original._sati);
		_minuti = new int(*original._minuti);
	}

	DatumVrijeme& operator=(const DatumVrijeme& original) {
		if (this != &original) {
			delete _dan; delete _mjesec; delete _godina; delete _sati; delete _minuti;
			_dan = new int(*original._dan);
			_mjesec = new int(*original._mjesec);
			_godina = new int(*original._godina);
			_sati = new int(*original._sati);
			_minuti = new int(*original._minuti);
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
	
	friend ostream & operator<< (ostream & COUT, DatumVrijeme & obj) {
		COUT << *obj._dan << "." << *obj._mjesec << "." << *obj._godina << " " << *obj._sati << ":" << *obj._minuti << endl;
		return COUT;
	}

	string ToString(stringFormat format) {
		stringstream ss;
		ss << *_dan << "." << *_mjesec << ".";
		if (format == 0 || format == 2)
			ss << *_godina % 100<<" ";
		else
			ss << *_godina<<" ";
		if (format == 2 || format == 3)
			ss << *_sati << ":" << *_minuti;
		
		return ss.str();
	}

	bool operator>(const DatumVrijeme& vrijeme) {
		int lijevi = *_godina * 100000000 + *_mjesec * 1000000 + *_dan * 10000 + *_sati * 100 + *_minuti;
		int desni= *vrijeme._godina * 100000000 + *vrijeme._mjesec * 1000000 + *vrijeme._dan * 10000 + *vrijeme._sati * 100 + *vrijeme._minuti;
		if (lijevi > desni)
			return true;
		return false;
	}

	bool operator==(const DatumVrijeme& vrijeme) {
		int lijevi = *_godina * 100000000 + *_mjesec * 1000000 + *_dan * 10000 + *_sati * 100 + *_minuti;
		int desni = *vrijeme._godina * 100000000 + *vrijeme._mjesec * 1000000 + *vrijeme._dan * 10000 + *vrijeme._sati * 100 + *vrijeme._minuti;
		if (lijevi == desni)
			return true;
		return false;
	}

	int operator-(const DatumVrijeme& vrijeme) {
		int lijevi = *_godina * 100000000 + *_mjesec * 1000000 + *_dan * 10000 + *_sati * 100 + *_minuti;
		int desni = *vrijeme._godina * 100000000 + *vrijeme._mjesec * 1000000 + *vrijeme._dan * 10000 + *vrijeme._sati * 100 + *vrijeme._minuti;
		return lijevi - desni;
	}

};
class Poruka {
	string _posiljalac;
	string _primalac;
	string _sadrzaj;
	DatumVrijeme _vrijeme;
public:
	Poruka(string posiljalac, string primalac, string sadrzaj, DatumVrijeme vrijeme) :_vrijeme(vrijeme){
		_posiljalac = posiljalac;
		_primalac = primalac;
		_sadrzaj = sadrzaj;
	}

	Poruka():_posiljalac("NOT SET"), _primalac("NOT SET") , _sadrzaj("NOT SET"){}

	friend ostream & operator<< (ostream & COUT, Poruka & obj) {
		COUT << "(" << obj._vrijeme << ") " << obj._posiljalac << " -> ";
		COUT << obj._sadrzaj;
		return COUT;
	}
	
	string getPrimalac() { return _primalac; }
	
	string getPosiljalac() { return _posiljalac; }
	
	string getSadrzaj() { return _sadrzaj; }

	DatumVrijeme& GetVrijeme() { return _vrijeme; }
};
class Komunikacija {
	string _naslov;
	int _ocjena; //ocjena komunikacije 1 - 5 se automatski izracunava 
	Kolekcija<Poruka, bool> _poruke;//bool se odnosi na podatak da li je poruka pobrisana tj. nece biti vidljiva u prikazu ukoliko se drugacije ne navede 
public:
	Komunikacija(string naslov) {
		_naslov = naslov;
		_ocjena = -1;
	}

	Komunikacija():_naslov("NOT SET"),_ocjena(0){}

	friend ostream & operator<< (ostream & COUT, Komunikacija & obj) {
		COUT << "(" << obj._ocjena << ") " << obj._naslov << " -> ";
		for (int i = 0; i < obj._poruke.size(); i++){
			COUT << obj._poruke[i] << endl;
		}
		return COUT;
	}

	Kolekcija<Poruka, bool> * getPoruke() { return &_poruke; }
	
	int getOcjena() { return _ocjena; }
	
	void SetOcjena(int ocjena) { _ocjena = ocjena; }

	string getNaslov() { return _naslov; }

	void Sort() {
		bool prolaz = true;
		int vel = _poruke.getTrenutno() - 1;
		while (prolaz){
			prolaz = false;
			for (int i = 0; i < vel; i++){
				if (_poruke.getElement1(i).GetVrijeme() > _poruke.getElement1(i + 1).GetVrijeme()) {
					Poruka temp1 = _poruke.getElement1(i);
					bool temp2 = _poruke.getElement2(i);
					_poruke.getElement1(i)=_poruke.getElement1(i+1);
					_poruke.getElement2(i) = _poruke.getElement2(i + 1);
					temp1 = _poruke.getElement1(i + 1);
					temp2 = _poruke.getElement2(i + 1);
					prolaz = true;
				}
			}
			vel--;
		}
	}

	
};



class Korisnik {
	string _korisnickoIme;
	char * _imePrezime;
	//char * - se odnosi na naziv predmeta ili tema gdje ce se cuvati kompletna komunikacija sa nastavnim osobljem ili korisnikom na odredenom predmetu/temi
	Kolekcija<char *, Komunikacija> _komunikacija;
public:
	Korisnik(const char * korisnickoIme, const char * imePrezime){
		_imePrezime = AlocirajNizKaraktera(imePrezime);
		_korisnickoIme = korisnickoIme;
	}
	
	Korisnik(Korisnik & original) :_komunikacija(original._komunikacija){
		_imePrezime = AlocirajNizKaraktera(original._imePrezime);
		_korisnickoIme = original._korisnickoIme;
	}

	~Korisnik(){
		delete[] _imePrezime; _imePrezime = nullptr;
	}

	friend ostream & operator<< (ostream &COUT, Korisnik &obj){
		COUT << obj._imePrezime << endl << obj._komunikacija << endl;
		return COUT;
	}
	
	Kolekcija<char*, Komunikacija> * GetKomunikacija() { return &_komunikacija; }
	
	string getKorisnickoIme() { return _korisnickoIme; }

	int GetIndeksKom(string naslov) {
		for (int j = 0; j < _komunikacija.size(); j++)
			if (_komunikacija.getElement1(j) == naslov)
				return j;
		return -1;
	}

	void PrikaziKomunikacijuPoNaslovu(string naslov, prikazPoruka prikaz) {
		int indeksKom = GetIndeksKom( naslov);
		if (indeksKom == -1)
			return;
		_komunikacija.getElement2(indeksKom).Sort();
		DatumVrijeme ispisan(_komunikacija.getElement2(indeksKom).getPoruke()->getElement1(0).GetVrijeme());
		for (int i = 0; i < _komunikacija.getElement2(indeksKom).getPoruke()->size(); i++) {
			if (!(_komunikacija.getElement2(indeksKom).getPoruke()->getElement1(i).GetVrijeme() == ispisan)) {
				cout << _komunikacija.getElement2(indeksKom).getPoruke()->getElement1(i).GetVrijeme() << endl;
				ispisan = _komunikacija.getElement2(indeksKom).getPoruke()->getElement1(i).GetVrijeme();
			}
			if (!_komunikacija.getElement2(indeksKom).getPoruke()->getElement2(i))
				cout << _komunikacija.getElement2(indeksKom).getPoruke()->getElement1(i).getSadrzaj() << crt;
			else
				cout << "PORUKA IZBRISANA" << crt;
		}
	}

	int PrikaziKomunikacijuSaKorisnikom(string korisnickoIme) {
		int poslano = 0;
		for (int i = 0; i < _komunikacija.size(); i++){
			for (int j = 0; j < _komunikacija.getElement2(i).getPoruke()->size(); j++) {
				if (_komunikacija.getElement2(i).getPoruke()->getElement1(j).getPrimalac() == korisnickoIme) {
					poslano++;
					cout <<endl<< _komunikacija.getElement2(i).getPoruke()->getElement1(j).getSadrzaj() <<endl<< endl;
				}
			}
		}
		return poslano;	
	}

	int Ocijeni(int ukupno) {
		if (ukupno > 20)
			return 1;
		if (ukupno > 15 && ukupno< 20)
			return 2;
		if (ukupno > 10 && ukupno< 15)
			return 3;
		if (ukupno > 5 && ukupno< 10)
			return 4;
		if (ukupno <=5)
			return 5;
		return 0;
	}

	void OcijeniKomunikaciju(string naslov) {
		int indeksKom = GetIndeksKom(naslov);
		int ukupno=0;
		for (int i = 0; i < _komunikacija.getElement2(indeksKom).getPoruke()->size()-1; i++){
			ukupno += _komunikacija.getElement2(indeksKom).getPoruke()->getElement1(i+1).GetVrijeme() - _komunikacija.getElement2(indeksKom).getPoruke()->getElement1(i).GetVrijeme();
		}
		ukupno /= _komunikacija.getElement2(indeksKom).getPoruke()->size();

		_komunikacija.getElement2(indeksKom).SetOcjena(Ocijeni(ukupno));
	}
};
int GetIndeksKomunikacije( Korisnik* korisnik, string nazivKomunikacije) {
	for (int j = 0; j < korisnik->GetKomunikacija()->size(); j++)
		if (korisnik->GetKomunikacija()->getElement1(j) == nazivKomunikacije)
			return j;
	return -1;
}

int GetIndeksKorisnika(Korisnik** korisnici, int max, string korisnickoIme) {
	for (int i = 0; i < max; i++)
		if (korisnici[i] != nullptr && korisnici[i]->getKorisnickoIme() == korisnickoIme)
			return i;
	return -1;
}

bool SadrziNedozvoljenuRijec(string poruka) {
	string pravilo = "(";
	for (size_t i = 0; i < nedozvoljeneRijeci.size(); i++){
		pravilo += nedozvoljeneRijeci[i];
		if(i!=nedozvoljeneRijeci.size()-1)
			pravilo += "|";
	}
	pravilo += ")";
	if (regex_search(poruka, regex(pravilo)))
		return true;
	return false;
}

void PosaljiPoruku(Korisnik** korisnici, int max, const char* nazivKomunikacije, string posiljalac, string primalac, string poruka, const DatumVrijeme& datum) {
	int indeksPos = GetIndeksKorisnika(korisnici, max, posiljalac);
	int indeksPri = GetIndeksKorisnika(korisnici, max, primalac);
	if (indeksPri == -1 || indeksPos == -1)
		throw exception("Greska, ne postoje primalac ili posiljalac.");
	int komPos = GetIndeksKomunikacije(korisnici[indeksPos], nazivKomunikacije);
	int komPri = GetIndeksKomunikacije(korisnici[indeksPri], nazivKomunikacije);
	bool provjera = false;
	if (SadrziNedozvoljenuRijec(poruka))
		provjera = true;
	if (komPos == -1 || komPri == -1) {
		korisnici[indeksPos]->GetKomunikacija()->AddElement(const_cast<char*>(nazivKomunikacije), Komunikacija(nazivKomunikacije));
		korisnici[indeksPri]->GetKomunikacija()->AddElement(const_cast<char*>(nazivKomunikacije), Komunikacija(nazivKomunikacije));
		komPos= korisnici[indeksPos]->GetKomunikacija()->size() - 1;
		komPri= korisnici[indeksPri]->GetKomunikacija()->size() - 1;
	}
	korisnici[indeksPos]->GetKomunikacija()->getElement2(komPos).getPoruke()->AddElement(Poruka(posiljalac, primalac, poruka, datum),provjera);
	korisnici[indeksPri]->GetKomunikacija()->getElement2(komPri).getPoruke()->AddElement(Poruka(posiljalac, primalac, poruka, datum), provjera);
}


int main(){

	/****************************************************************************
	1. SVE KLASE TREBAJU POSJEDOVATI ADEKVATAN DESTRUKTOR
	2. NAMJERNO IZOSTAVLJANJE KOMPLETNIH I/ILI POJEDINIH DIJELOVA DESTRUKTORA KOJI UZROKUJU RUNTIME ERROR CE BITI OZNACENO KAO "RE"
	3. SPAŠAVAJTE PROJEKAT KAKO BI SE SPRIJECILO GUBLJENJE URAÐENOG ZADATKA
	4. PROGRAMSKI CODE SE TAKOÐER NALAZI U FAJLU CODE.TXT
	5. NAZIVI FUNKCIJA, TE BROJ I TIP PARAMETARA MORAJU BITI IDENTICNI ONIMA KOJI SU KORIŠTENI U TESTNOM CODE-U, OSIM U SLUCAJU DA POSTOJI ADEKVATAN RAZLOG ZA NJIHOVU MODIFIKACIJU. OSTALE, POMOCNE FUNKCIJE MOŽETE IMENOVATI I DODAVATI PO ŽELJI.
	6. IZUZETAK BACITE U FUNKCIJAMA U KOJIMA JE TO NAZNACENO.
	****************************************************************************/


	/*rijeci koje su zabranjene u komunikaciji*/
	nedozvoljeneRijeci.push_back("ubiti");
	nedozvoljeneRijeci.push_back("mrzim");
	nedozvoljeneRijeci.push_back("rat");

	DatumVrijeme temp,
		datum19062017_1015(19, 6, 2017, 10, 15),
		datum19062017_1020(19, 6, 2017, 10, 20),
		datum19062017_1021(19, 6, 2017, 10, 21),
		datum19062017_1030(19, 6, 2017, 10, 30);

	cout << datum19062017_1015.ToString(DDMMGGGG) << endl;//vraca string: 19.6.2017
	cout << datum19062017_1015.ToString(DDMMGGGGSSMM) << endl;//vraca string: 19.6.2017 10:15


	const int max = 3;
	Korisnik * korisnici[max];
	korisnici[0] = new Korisnik("jasmin", "Jasmin Azemovic");
	korisnici[1] = new Korisnik("adel", "Adel Handzic");
	korisnici[2] = new Korisnik("denis", "Denis Music");

	
	/*sve poruke se cuvaju u komunikaciji posiljaoca i primaoca tako da u svakom momentu oba ucesnika u komunikaciji posjeduju kompletnu historiju poruka. 
    poruke se grupisu na osnovu naslova, pa ce poruke pod naslovom "sastanak" izmedju dva korisnika sadrzavati sve poruke koje su razmijenjene na tu temu
	u slucaju da posiljalac ili primalac ne postoje funkcija treba baciti izuzetak sa odgovarajucom porukom. takodjer, koristeci regex sve poruke koje sadrze 
    neku od zabranjenih rijeci trebaju biti oznacene kao*/
	
	PosaljiPoruku(korisnici, max, "Sastanak",
		korisnici[0]->getKorisnickoIme(), korisnici[1]->getKorisnickoIme(),
		"Danas imamo sastanak u 10h!", datum19062017_1015);

	PosaljiPoruku(korisnici, max, "Sastanak",
		korisnici[1]->getKorisnickoIme(), korisnici[0]->getKorisnickoIme(),
		"Naravno, vidimo se u sali za sastanke!", datum19062017_1020);

	PosaljiPoruku(korisnici, max, "Sastanak",
		korisnici[0]->getKorisnickoIme(), korisnici[1]->getKorisnickoIme(),
		"Sa sobom ponesi radni materijal!", datum19062017_1021);
	

	/*ispisuje sve poruke koje je korisnik razmijenio pod naslovom definisanim parametrom. ispis komunikacije
	treba biti organizovan po datumu na nacin da se prvo prikaze datum, a nakon toga sve poruke koje su razmijenjene tog datuma.
	Na mjestima pobrisanih poruka prikazati tekst "PORUKA IZBRISANA"
	*/
	korisnici[0]->PrikaziKomunikacijuPoNaslovu("Sastanak", SVE_PORUKE);

	/*funkcija prikazuje sve poruke sa korisnikom cije korisnicko ime je definisano parametrom, te vraca ukupan broj (razmijenjenih poruka sa tim korisnikom)*/
	int ukupnoPoruka = korisnici[0]->PrikaziKomunikacijuSaKorisnikom("adel");

	/*formira ocjenu na nivou komunikacije na nacin da izracunava prosjecno vrijeme odgovora drugog ucesnika izrazeno u minutama i to prema sljedecoj skali
	prosjek > 20 minuta = ocjena 1
	prosjek > 15 minuta = ocjena 2
	prosjek > 10 minuta = ocjena 3
	prosjek > 5 minuta = ocjena 4
	prosjek <=5 minuta = ocjena 5
	*/
	
	korisnici[0]->OcijeniKomunikaciju("Sastanak");

	for (size_t i = 0; i < max; i++)
	{
		delete korisnici[i];
		korisnici[i] = nullptr;
	}

	system("PAUSE");
	return 0;
}
