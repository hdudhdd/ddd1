#include <iostream>
#include <vector>
using namespace std;

class Vehicul {
public:
    string numar;
    int oraIntrare, minIntrare;
    Vehicul(string nr, int ora, int min) : numar(nr), oraIntrare(ora), minIntrare(min) {}
};

class SpatiuParcare {
public:
    bool ocupat;
    Vehicul* masina;
    SpatiuParcare() : ocupat(false), masina(nullptr) {}
    void ocupa(Vehicul* v) { ocupat = true; masina = v; }
    void elibereaza() { ocupat = false; delete masina; masina = nullptr; }
};

class ParcareSimpla {
    vector<SpatiuParcare> locuri;
    double tarifPeOra;
public:
    ParcareSimpla(int nrLocuri, double tarif) : locuri(nrLocuri), tarifPeOra(tarif) {}
    bool adaugaVehicul(string nr, int ora, int min) {
        for (auto &loc : locuri) {
            if (!loc.ocupat) {
                loc.ocupa(new Vehicul(nr, ora, min));
                cout << "Vehicul " << nr << " a parcat la " << ora << ":" << min << "\n";
                return true;
            }
        }
        cout << "Parcarea este plina! " << nr << " nu poate intra.\n";
        return false;
    }
    void scoateVehicul(string nr, int oraOut, int minOut) {
        for (auto &loc : locuri) {
            if (loc.ocupat && loc.masina->numar == nr) {
                int durata = (oraOut * 60 + minOut) - (loc.masina->oraIntrare * 60 + loc.masina->minIntrare);
                double cost = (durata / 60.0) * tarifPeOra;
                cout << "Vehicul " << nr << " a iesit din parcare. Cost: " << cost << " RON\n";
                loc.elibereaza();
                return;
            }
        }
        cout << "Vehicul " << nr << " nu se afla in parcare.\n";
    }
};

int main() {
    ParcareSimpla parcare(2, 10.0);
    parcare.adaugaVehicul("ABC123", 10, 15);
    parcare.adaugaVehicul("XYZ789", 10, 30);
    parcare.adaugaVehicul("LMN456", 11, 00);
    parcare.scoateVehicul("ABC123", 12, 15);
    parcare.adaugaVehicul("LMN456", 11, 10);
    return 0;
}
