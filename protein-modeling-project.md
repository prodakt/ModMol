# Individual Protein Modeling Project

**From Protein Sequence to a Validated and Refined 3D Structure**  
*Kurs Podstaw Modelowania Molekularnego — instrukcja projektu indywidualnego*

> **Cel:** samodzielnie wybrać sekwencję białka (**TARGET**), znaleźć homologiczny szablon eksperymentalny (**TEMPLATE**), przeprowadzić charakterystykę sekwencji i modelowanie struktury 3D, ocenić jakość i wiarygodność modelu, zidentyfikować problematyczne regiony, wykonać refinement oraz ponownie zweryfikować wynik.

Projekt wykonuje **każdy student indywidualnie**. Cztery grupy laboratoryjne korzystają z różnych głównych serwisów modelowania, ale wykonują te same analizy i przekazują porównywalny zestaw wyników.

## 1. Organizacja pracy: cztery grupy

| Grupa | Główna metoda / serwis | Uwagi |
|---|---|---|
| **1** | [SWISS-MODEL](https://swissmodel.expasy.org/) | Modelowanie porównawcze na podstawie szablonu homologicznego |
| **2** | [I-TASSER](https://zhanggroup.org/I-TASSER/) | Threading, składanie fragmentów i udoskonalanie struktury; możliwe kolejki |
| **3** | [Phyre2](https://www.sbg.bio.ic.ac.uk/phyre2/html/page.cgi) | Rozpoznawanie fałdu i modelowanie z wykorzystaniem szablonów; dostępność należy sprawdzić przed zajęciami |
| **4** | [AlphaFold Server](https://alphafoldserver.com/) **+ SWISS-MODEL** | AlphaFold to predykcja AI, a nie klasyczne modelowanie homologiczne; obowiązkowy dodatkowy model SWISS-MODEL zapewnia realizację celu kursu |

**Uwaga organizacyjna:** przed przydziałem grup prowadzący sprawdza aktualne limity, warunki korzystania, rejestrację i dostępność serwisów. W razie niedostępności usługi dopuszczalny jest serwis zastępczy uzgodniony z prowadzącym. Obliczenia I-TASSER i niektórych innych serwisów należy uruchomić odpowiednio wcześniej. W trakcie oczekiwania można wykonywać etapy 3–5.

## 2. Wybór TARGET i TEMPLATE — warunek rozpoczęcia projektu

### 2.1. Wybór sekwencji TARGET

Wyszukaj białko w [UniProt](https://www.uniprot.org/) lub [NCBI Protein](https://www.ncbi.nlm.nih.gov/protein/) i pobierz pełną sekwencję FASTA.

| Parametr | Kryterium dydaktyczne |
|---|---|
| Długość TARGET | **150–400 aa** |
| Sekwencja | Pełna, bez nieokreślonych reszt `X`; sprawdź izoformę i dojrzałą postać białka |
| Adnotacja | Znana lub możliwa do opisania funkcja biologiczna; preferowany rekord UniProt reviewed |
| Architektura | Preferowane białko globularne, jedno- lub proste dwudomenowe |
| Białko błonowe | Dopuszczalne po akceptacji prowadzącego; może wymagać specjalnej interpretacji i refinementu |
| Struktura eksperymentalna TARGET | Do podstawowego zadania wybierz sekwencję bez dostępnej w PDB **pełnej eksperymentalnej struktury o identycznej sekwencji** |

### 2.2. Poszukiwanie homologicznych szablonów

1. Otwórz [NCBI Protein BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE=Proteins).
2. Uruchom **BLASTp** dla sekwencji TARGET, wybierając bazę sekwencji struktur **PDB** (jeśli dostępna w interfejsie; w przeciwnym razie użyj wyszukiwania sekwencyjnego [RCSB PDB](https://www.rcsb.org/)).
3. Zapisz parametry wyszukiwania, datę oraz wyniki. **Nie wybieraj szablonu tylko na podstawie najwyższego `% identity`.**
4. Zidentyfikuj **co najmniej dwa** potencjalne szablony i sprawdź ich rekordy PDB, odpowiednie łańcuchy oraz kompletność współrzędnych.
5. Wybierz jeden główny TEMPLATE i uzasadnij wybór.

| Parametr | Preferowane wymaganie |
|---|---|
| Sequence identity TARGET–TEMPLATE | **60–95%** (zakres przyjęty na potrzeby ćwiczenia) |
| Query coverage | **≥85%**, najlepiej **≥90%** |
| Pokrycie strukturalne | Większość dopasowanego fragmentu musi mieć **rzeczywiście wyznaczone współrzędne** w strukturze PDB |
| E-value | Preferowane **≤ 1 × 10⁻¹⁰** |
| Gaps | Nieliczne; odnotuj szczególnie insercje/delecje w rdzeniu białka |
| Struktura TEMPLATE | Eksperymentalna: X-ray, cryo-EM lub NMR; preferowana dobra jakość i kompletność |
| X-ray | Preferowana rozdzielczość **≤2,5 Å**; nie stosuj tego progu do NMR |
| Stan biologiczny | Sprawdź ligandy, kofaktory, podjednostki i stan oligomeryczny |

**Interpretacja:** `sequence identity` dotyczy przyrównanych pozycji; `query coverage` mówi, jaką część TARGET obejmuje dopasowanie; `template coverage` i kompletność współrzędnych są odrębnymi zagadnieniami. Zakresy procentowej identyczności z dawnej prezentacji traktujemy jako dydaktyczne przybliżenia, **nie** jako bezwzględne granice modelowalności. Nawet przy 100% identity modelowanie może być użytecznym ćwiczeniem, ale nie spełnia przyjętego tutaj kryterium wyboru TARGET.

**Tabela A. Wybór sekwencji i szablonu**

| Parametr | TARGET | TEMPLATE 1 | TEMPLATE 2 |
|---|---|---|---|
| Białko / organizm | | | |
| UniProt / NCBI accession | | | |
| PDB ID / chain | — | | |
| Długość sekwencji (aa) | | | |
| Sequence identity (%) | — | | |
| Query coverage (%) | — | | |
| Alignment length / gaps | — | | |
| E-value | — | | |
| Metoda eksperymentalna / rozdzielczość | — | | |
| Brakujące reszty w strukturze | — | | |
| Uzasadnienie wyboru | | | |

**Punkt kontrolny:** prześlij prowadzącemu identyfikator TARGET, FASTA, dwa kandydackie szablony, tabelę A i krótkie uzasadnienie. **Modelowanie rozpoczyna się po zatwierdzeniu wyboru.**

## 3. Teoretyczne właściwości fizykochemiczne TARGET i TEMPLATE

Oblicz dla **obu pełnych, jasno zdefiniowanych sekwencji** parametry za pomocą [ExPASy ProtParam](https://web.expasy.org/protparam/) i opcjonalnie [Compute pI/Mw](https://web.expasy.org/compute_pi/). Możesz zweryfikować wyniki w notebooku Python/Colab (Biopython `ProteinAnalysis`).

**Tabela B. Porównanie właściwości fizykochemicznych**

| Parametr | TARGET | TEMPLATE | Interpretacja różnicy |
|---|---|---|---|
| Długość sekwencji (aa) | | | |
| Masa cząsteczkowa (Da/kDa) | | | |
| Teoretyczny pI | | | |
| Reszty ujemnie naładowane (Asp + Glu) | | | |
| Reszty dodatnio naładowane (Arg + Lys) | | | |
| Współczynnik ekstynkcji | | | |
| Instability index | | | |
| Aliphatic index | | | |
| GRAVY | | | |
| Skład aminokwasowy (%) — wybrane reszty | | | |

**Wzorzec opracowania tabel:** w prezentacji prowadzącego wskazano **tabele 1, 2 i 3** publikacji [DOI: 10.1002/jnr.23177](https://doi.org/10.1002/jnr.23177) jako przykład porównawczego przedstawienia wyników. **Zapoznaj się z tymi tabelami** i wykorzystaj ich ideę organizacji danych, dostosowując własne zestawienie do TARGET i TEMPLATE. Nie kopiuj danych z publikacji. Niniejsza tabela B jest szablonem dydaktycznym, a nie reprodukcją tabel z artykułu.

**Pytanie:** które różnice mogą wynikać ze składu sekwencji, a które rzeczywiście mogą mieć znaczenie dla funkcji? `Instability index` nie jest eksperymentalnym pomiarem stabilności białka.

## 4. Profile właściwości wzdłuż sekwencji

Wykonaj analizy **TARGET i TEMPLATE** przy tych samych skalach i parametrach. Podawaj numerację aminokwasów, wielkość okna przesuwnego i źródło skali. Porównuj homologiczne regiony na podstawie przyrównania sekwencji.

**Przykładowe narzędzia:** [ExPASy ProtScale](https://web.expasy.org/protscale/), notebook Python/Colab (Biopython, NumPy, Matplotlib). W historycznej prezentacji wskazano również narzędzia statystyk sekwencji EMBL-EBI.

**Profile obowiązkowe:**

1. **Hydrofobowość** według skali **Kyte’a–Doolittle’a**, dla co najmniej dwóch wielkości okna.
2. **Hydrofilowość** (z podaniem użytej skali).
3. **Dwa dodatkowe profile** wybrane spośród: polarności, elastyczności, dostępności powierzchniowej, tendencji do tworzenia helis/struktur β, ładunku lub **antygenowości** (wyłącznie jako predykcji zależnej od przyjętej skali, a nie dowodu immunogenności).

**Rezultat:** minimum cztery profile dla obu białek, z podpisami i omówieniem zgodności/rozbieżności; w razie potrzeby oznacz regiony TM i nieuporządkowane na wykresach.

## 5. Regiony transmembranowe, nieuporządkowanie i struktura drugorzędowa

Nie ograniczaj się do jednego algorytmu. Wybierz **co najmniej dwa narzędzia do predykcji regionów transmembranowych** i **co najmniej dwa do struktury drugorzędowej**; dla nieuporządkowania wykonaj przynajmniej jedną niezależną predykcję. Dostępność historycznych serwisów należy zweryfikować przed użyciem; niedziałające serwisy można zastąpić odpowiednikami po uzgodnieniu.

### 5.1. Regiony transmembranowe / topologia

Narzędzia wskazane w prezentacji i uzupełniające:

- [TMHMM 2.0](https://services.healthtech.dtu.dk/services/TMHMM-2.0/) — klasyczna predykcja helis TM;
- [DeepTMHMM](https://dtu.biolib.com/DeepTMHMM) — nowsza predykcja topologii;
- [TOPCONS](https://topcons.cbr.su.se/) — predykcja konsensusowa;
- **SOSUI** — klasyfikacja białek błonowych i przewidywanie helis TM (sprawdź aktualną dostępność);
- **ProtScale** — profil hydrofobowości jako dodatkowa przesłanka, **nie** samodzielny dowód obecności helisy TM.

Zestaw pozycje przewidywanych regionów i porównaj je z adnotacjami UniProt oraz strukturą TEMPLATE. W przypadku białek rozpuszczalnych prawidłowym wynikiem może być brak regionów TM. Dla białek β-barrel pamiętaj o ograniczeniach metod skoncentrowanych na helisach TM.

### 5.2. Regiony nieuporządkowane

Przeprowadź analizę predykcji **intrinsically disordered regions (IDR)**, np. przy użyciu dostępnego serwisu z rodziny **IUPred**. W prezentacji przedstawiono także historyczne narzędzie **GeneSilico MetaDisorder**. Zaznacz pozycje potencjalnych IDR i skonfrontuj je z lokalną pewnością modelu 3D. Niska pewność modelu **nie oznacza automatycznie** biologicznego nieuporządkowania.

### 5.3. Struktura drugorzędowa

Przykładowe narzędzia wskazane w prezentacji:

- **GOR IV** — klasyczna metoda predykcji;
- [JPred](https://www.compbio.dundee.ac.uk/jpred/) — predykcja wykorzystująca informację ewolucyjną;
- **PSSpred** — predykcja struktury drugorzędowej;
- **SOPMA** — klasyczna predykcja struktury drugorzędowej;
- opcjonalnie **PSIPRED** jako dodatkowa metoda porównawcza.

Porównaj udział i położenie helis α, nici β i pozostałych regionów. Dla TEMPLATE porównaj przewidywania z przypisaniem struktury drugorzędowej na podstawie **struktury eksperymentalnej** (np. DSSP). Opisz zgodność i różnice.

**Tabela C. Zestawienie predykcji**

| Analiza | Narzędzie 1: wynik | Narzędzie 2: wynik | TARGET vs TEMPLATE / interpretacja |
|---|---|---|---|
| Regiony TM | | | |
| Regiony IDR | | — | |
| Helisy α / nici β / pozostałe | | | |

## 6. Budowa modelu 3D

Uruchom narzędzie przypisane grupie (sekcja 1), używając zatwierdzonej sekwencji TARGET. Jeśli narzędzie pozwala wybrać szablon, udokumentuj, czy wykorzystano zatwierdzony TEMPLATE, czy inny. Jeśli model obejmuje tylko część sekwencji, wyraźnie określ jej zakres.

**Zapisz:** nazwę narzędzia, datę, identyfikator zadania, parametry, użyte szablony, pokrycie, ocenę jakości raportowaną przez serwis oraz plik PDB/mmCIF. Zachowaj **model pierwotny** (`target_initial.pdb` lub `.cif`).

**Ważne:** I-TASSER i Phyre2 nie są identyczne metodologicznie z SWISS-MODEL; AlphaFold Server nie jest metodą modelowania homologicznego. W raporcie opisz rzeczywistą metodę, a nie nazywaj wszystkich czterech serwisów „homology modeling”.

## 7. Wizualizacja i porównanie strukturalne

**Narzędzia:** **RasMol / RasWin / RasTop** (prosta, historyczna wizualizacja; sprawdź zgodność z systemem) lub **[PyMOL](https://pymol.org/)** (zalecany do porównań i ilustracji). Można pomocniczo użyć przeglądarki Mol*.

Przygotuj ilustracje TEMPLATE i modelu TARGET w tych samych orientacjach. Pokaż minimum **trzy cechy strukturalne**, np. helisy/struktury β, regiony hydrofobowe, miejsca wiązania ligandów, regiony TM, miejsca o niskiej jakości modelu.

Wykonaj superpozycję **TARGET–TEMPLATE** z wykorzystaniem dopasowanych reszt. Podaj:

- RMSD, rodzaj porównywanych atomów (np. Cα), **liczbę dopasowanych reszt** i sposób superpozycji;
- opcjonalnie TM-score / [TM-align](https://zhanggroup.org/TM-align/);
- fragmenty o największych różnicach oraz ich możliwe przyczyny.

**Uwaga:** niski RMSD dla bardzo krótkiego dopasowanego fragmentu nie dowodzi podobieństwa całych struktur.

## 8. Walidacja i identyfikacja „hot spots”

Oceń model pierwotny przy użyciu **co najmniej dwóch niezależnych narzędzi oceny** i wyników dostarczonych przez serwis modelujący.

**Proponowane narzędzia:**

- [SWISS-MODEL Structure Assessment / QMEAN](https://swissmodel.expasy.org/assess) — ocena globalna i lokalna, QMEAN/QMEANDisCo zależnie od usługi;
- [ProSA-web](https://prosa.services.came.sbg.ac.at/prosa.php) — ocena statystyczna i profil energii;
- [MolProbity](https://molprobity.biochem.duke.edu/) — geometria, Ramachandran, konflikty steryczne;
- [PDBsum](https://www.ebi.ac.uk/thornton-srv/databases/pdbsum/) — organizacja strukturalna, diagramy i dodatkowe raporty **jeśli serwis jest dostępny**;
- [ModFOLD](https://www.reading.ac.uk/bioinf/ModFOLD/) — dodatkowa ocena jakości, jeśli usługa działa;
- [RCSB PDB: Structure Validation and Quality](https://www.rcsb.org/docs/additional-resources/structure-validation-and-quality) — interpretacja danych walidacyjnych struktur eksperymentalnych.

Zbadaj **Ramachandran outliers, clashes, jakość lokalną i globalną** oraz, gdy są dostępne, specyficzne wskaźniki modelowania (np. GMQE/QMEANDisCo, C-score, pLDDT). Nie porównuj tych miar tak, jakby były identyczne.

**Hot spots:** wskaż do trzech (lub więcej, jeśli uzasadnione) problematycznych regionów i opisz ich pozycje, rodzaj problemu, dowód z analizy oraz możliwą przyczynę. **Nie wymyślaj problemów:** jeśli model nie wykazuje trzech wyraźnych hot spots, uzasadnij mniejszą liczbę.

## 9. Refinement / minimalizacja i ponowna walidacja

**Refinement jest obowiązkowy.** Wybierz co najmniej jeden działający serwis online, np.:

- [GalaxyRefine / GalaxyRefine2](https://galaxy.seoklab.org/) — udoskonalanie lokalnej geometrii i struktury;
- [YASARA Energy Minimization Server](https://www.yasara.org/minimizationserver.htm) — minimalizacja energii;
- inne narzędzie po uzgodnieniu z prowadzącym, np. historycznie wskazywane w prezentacji **ModRefiner, MODELLER, ReFOLD**, jeśli jego obecna dostępność i sposób użycia zostały potwierdzone.

Zapisz parametry, nazwę metody i plik **`target_refined.pdb`** (lub `.cif`). Powtórz te same analizy walidacyjne co dla modelu pierwotnego.

**Tabela D. Ocena efektu refinementu**

| Parametr | Initial model | Refined model | Poprawa / pogorszenie? |
|---|---|---|---|
| Globalny wskaźnik jakości (ta sama metoda) | | | |
| Lokalna jakość regionów problematycznych | | | |
| Ramachandran outliers | | | |
| Clashscore | | | |
| ProSA Z-score (jeśli dostępny) | | | |
| RMSD względem TEMPLATE (ten sam zestaw reszt) | | | |
| RMSD initial–refined | — | | |
| Energia (tylko dla identycznego pola siłowego i protokołu) | | | |
| Ocena końcowa | | | |

**Zastrzeżenie naukowe:** poprawa geometrii, spadek energii w danym protokole czy korzystniejszy QMEAN **nie dowodzą stabilności termodynamicznej** białka ani poprawności biologicznej fałdu. Do badania stabilności potrzebne byłyby dalsze analizy (np. właściwie zaprojektowana dynamika molekularna i/lub doświadczenia). Jeśli refinement pogorszył model, **udokumentuj to** — nie zakładaj poprawy z góry.

## 10. Raport końcowy i zaliczenie

**Raport:** Introduction → Materials and Methods → Results → Discussion → Conclusions → References. Powinien zawierać tabele A–D, porównawcze wykresy profili, ilustracje strukturalne, raporty walidacyjne i opis refinementu.

Jeden plik od każdej jednej osoby. Projekt opcjonalnie można prezentować lub bronić, w tym wypadku należy dodatkowo wspomagać się raportami i wynikami poszczególnych narzędzi oraz analiz.

<!--
**Minimalny pakiet plików:**

```text
student_project/
├── target.fasta
├── template.fasta
├── template.pdb                 # lub .cif
├── target_initial.pdb           # lub .cif
├── target_refined.pdb           # lub .cif
├── tables/
├── profiles/
├── validation/
├── figures/
├── methods-and-parameters.md
└── final-report.pdf
```

W `methods-and-parameters.md` zapisz wersje i daty użycia narzędzi, identyfikatory analiz, parametry oraz źródła danych. Wyniki mogą pochodzić z narzędzi internetowych lub notebooków Python/Colab.

-->



### Przykładowe pytania w dyskusji

1. **Czy model jest wiarygodny?** Jakie dane potwierdzają tę ocenę, a jakie ją ograniczają?
2. **Czy geometria modelu jest fizycznie realistyczna?** Jakie nieprawidłowości wykryto?
3. **Co można, a czego nie można powiedzieć o stabilności?** Jakie dodatkowe analizy byłyby potrzebne?
4. **Gdzie znajdują się hot spots?** Jak wiążą się z sekwencją, przyrównaniem i profilem jakości?
5. **Czy refinement pomógł?** Pokaż porównanie metryk i struktur przed/po.
6. **Jakie co najmniej dwie dalsze analizy** proponujesz i dlaczego?

**Ocena:** poprawność metod (30%), interpretacja biologiczna (25%), porównanie TARGET–TEMPLATE (20%), walidacja i krytyczna ocena refinementu (15%), dokumentacja i odtwarzalność (10%). **Nie jest wymagane uzyskanie „idealnego” ani udowodnienie „stabilnego” modelu.**

---
<!--
## Materiały i źródła

- [Prezentacja prowadzącego: *Modelowanie molekularne — etapy*](#) — materiał przekazywany oddzielnie; schemat projektu i przykłady analiz.
- [Publikacja wskazana w prezentacji — DOI: 10.1002/jnr.23177](https://doi.org/10.1002/jnr.23177) — **tabele 1–3** jako przykład organizacji zestawień porównawczych.
- [UniProt](https://www.uniprot.org/) · [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) · [RCSB PDB](https://www.rcsb.org/)
- [ExPASy ProtParam](https://web.expasy.org/protparam/) · [ExPASy ProtScale](https://web.expasy.org/protscale/)
- [SWISS-MODEL](https://swissmodel.expasy.org/) · [I-TASSER](https://zhanggroup.org/I-TASSER/) · [AlphaFold Server](https://alphafoldserver.com/)
- [TM-align](https://zhanggroup.org/TM-align/) · [ProSA-web](https://prosa.services.came.sbg.ac.at/prosa.php)

> **Przed publikacją:** prowadzący powinien przetestować na jednej sekwencji dostępność serwisów (szczególnie starszych narzędzi), ograniczenia liczby zadań oraz przebieg refinementu. Link do prezentacji w pierwszym punkcie należy zastąpić rzeczywistą ścieżką do PDF w repozytorium albo usunąć.
-->
