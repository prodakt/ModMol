# Projekt indywidualny: modelowanie białka w SWISS-MODEL

**Kurs:** Podstawy Modelowania Molekularnego  
**Grupa:** SWISS-MODEL (modelowanie homologiczne, bez modelowania AlphaFold)  
**Temat:** Od sekwencji aminokwasowej do zweryfikowanego i zoptymalizowanego modelu 3D  
**Forma:** praca indywidualna; porównanie TARGET z eksperymentalnym TEMPLATE  
**Narzędzie główne:** [SWISS-MODEL](https://swissmodel.expasy.org/)  
**Instrukcja szczegółowa:** [SWISS-MODEL — Help](https://swissmodel.expasy.org/docs/help) · [Oficjalne przykłady krok po kroku](https://swissmodel.expasy.org/docs/examples) · [Filmy instruktażowe](https://swissmodel.expasy.org/docs/tutorial)

> **Cel:** wybrać białko **TARGET** i strukturę referencyjną **TEMPLATE**, przeprowadzić komplet analiz sekwencji, **zbudować model homologiczny przy świadomym wyborze szablonu w SWISS-MODEL**, ocenić jakość globalną i lokalną modelu (**GMQE, QMEANDisCo**), wykryć regiony problematyczne (*hot spots*), przeprowadzić **niezależny refinement**, a następnie sprawdzić, czy jakość modelu poprawiła się. **Wyniki modelowania i minimalizacji energii nie są eksperymentalnym dowodem termodynamicznej stabilności białka.**

## Przed rozpoczęciem — obowiązkowa kontrola danych

- [ ] **TARGET:** zatwierdzona, pełna sekwencja białka o długości **150–400 aminokwasów**, identyfikator UniProt/NCBI; zapisz `target.fasta`.
- [ ] **TEMPLATE:** co najmniej jeden homolog ze **strukturą eksperymentalną** w PDB, wybrany na podstawie BLASTp. Preferowane **60–95% sequence identity**, **query coverage ≥85%** (najlepiej ≥90%) oraz istotny statystycznie wynik przyrównania (preferowane **E-value ≤1 × 10⁻¹⁰**). To *kryteria projektu dydaktycznego*, a nie ogólna granica możliwości modelowania.
- [ ] **Dwa kandydaty na TEMPLATE:** zachowaj identyfikatory PDB, łańcuchy, metody wyznaczenia struktur, pokrycie i rozdzielczość (gdy dotyczy); zaznacz, który kandydat jest preferowany.
- [ ] **Dane:** `template.fasta` (sekwencja homologu) i `template.pdb` lub `template.cif` (eksperymentalne współrzędne odpowiedniego łańcucha / zespołu biologicznego).
- [ ] **Wyniki przyrównania:** identity, query coverage, alignment length, E-value, luki oraz zakresy dopasowania; zachowaj plik lub zrzut wyników BLASTp.
- [ ] **Kontrola zgodności:** sprawdź, czy w PDB nie brakuje części sekwencji, czy jest to właściwy łańcuch i czy struktura nie przedstawia jedynie krótkiej domeny.
- [ ] **Uzasadnienie:** dlaczego ten TEMPLATE nadaje się do modelowania TARGET, mimo różnic sekwencyjnych?

**Punkty wyjścia:** [UniProt](https://www.uniprot.org/), [NCBI Protein](https://www.ncbi.nlm.nih.gov/protein/), [BLASTp](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE=Proteins), [RCSB PDB](https://www.rcsb.org/).

**Ważne dla tej grupy:** SWISS-MODEL potrafi także znaleźć szablon z **AlphaFold DB**. W tym ćwiczeniu wybieramy **wyłącznie szablon oparty na strukturze eksperymentalnej PDB**; w tabeli wyników sprawdzamy pole **Method** i **Template Search Method**. Nie używamy predykcji AlphaFold jako TEMPLATE. Nie należy też mylić **TEMPLATE wybranego w BLASTp** ze **szablonem rzeczywiście użytym do budowy modelu** — oba muszą być udokumentowane.

**Organizacja pracy:** w etapach 1–6 porównuj **TARGET i TEMPLATE osobno, przy tych samych parametrach**; w etapach 7–12 pracuj z modelem TARGET. Jeśli zajęcia rozłożone są na kilka spotkań, uruchom modelowanie po zatwierdzeniu danych i zapisz wynik lokalnie. Projekty SWISS-MODEL nie są przechowywane bezterminowo — **pobierz cały projekt i raport HTML**.

---

## 1. Podstawowe właściwości fizykochemiczne

**Gdzie:** [ExPASy ProtParam](https://web.expasy.org/protparam/) · [Compute pI/Mw](https://web.expasy.org/compute_pi/) (narzędzie pomocnicze).

- [ ] Wklej **TARGET** do ProtParam i uruchom obliczenia.
- [ ] Powtórz dla **TEMPLATE** (całej sekwencji wybranego homologu, nie tylko reszt widocznych w modelu PDB; ewentualne różnice opisz).
- [ ] Zapisz: długość sekwencji, masę cząsteczkową, teoretyczne pI, liczbę Asp+Glu i Arg+Lys, skład aminokwasowy, współczynnik ekstynkcji, **instability index**, **aliphatic index**, **GRAVY** oraz — jeśli przydatne — przewidywany czas półtrwania.
- [ ] Wyjaśnij co najmniej **trzy podobieństwa albo różnice** oraz ich potencjalny związek ze strukturą/funkcją białka.
- [ ] Zapisz narzędzie, datę, wersję (jeżeli podana) i sposób przygotowania sekwencji.

**Tabela 1 — właściwości TARGET vs TEMPLATE**

| Parametr | TARGET | TEMPLATE | Interpretacja różnicy |
|---|---|---|---|
| UniProt/NCBI accession | | | |
| Długość (aa) | | | |
| Masa cząsteczkowa (Da) | | | |
| Teoretyczne pI | | | |
| Asp + Glu | | | |
| Arg + Lys | | | |
| Instability index | | | |
| Aliphatic index | | | |
| GRAVY | | | |
| Współczynnik ekstynkcji | | | |

**Przykład prezentacji wyników:** zobacz **tabele 1–3** w publikacji wskazanej przez prowadzącego: **[DOI: 10.1002/jnr.23177](https://doi.org/10.1002/jnr.23177)**. Opracuj **własne** tabele — nie kopiuj tabel z publikacji.

**Oddaj:** Tabela 1, źródło sekwencji i 3–5 zdań interpretacji. **Instability index** to teoretyczny indeks oparty na sekwencji, **nie pomiar stabilności termodynamicznej**.

## 2. Profile właściwości wzdłuż sekwencji

**Gdzie:** [ExPASy ProtScale](https://web.expasy.org/protscale/) · opcjonalnie Python/Google Colab (np. Biopython + Matplotlib).

- [ ] Dla **TARGET** wykonaj **profil hydrofobowości Kyte’a–Doolittle’a**; porównaj **dwie długości okna** (np. 9 i 19 aa).
- [ ] Wykonaj **profil hydrofilowości** (np. Hopp–Woods — jeśli skala jest dostępna).
- [ ] Wykonaj przynajmniej **jeden profil tendencji aminokwasowych** — np. skłonność do tworzenia helisy α, nici β, skrętu, elastyczność lub dostępność powierzchniowa (zapisz dokładnie nazwę skali).
- [ ] Dla **TEMPLATE** powtórz **te same skale, długości okna i ustawienia**.
- [ ] Na wykresach podaj nazwę białka, skalę, numerację aminokwasów, długość okna i jednostkę/skoring, jeśli występuje.
- [ ] Zaznacz fragmenty o wysokiej hydrofobowości, lokalnych ekstremach i różnicach TARGET–TEMPLATE, **uwzględniając przyrównanie sekwencji**.

**Oddaj:** co najmniej **trzy rodzaje profili dla obu sekwencji**, a w przypadku hydrofobowości porównanie dwóch długości okna; krótka interpretacja. **Profil tendencji do helisy α nie jest predykcją całej struktury drugorzędowej.**

## 3. Regiony transmembranowe i topologia

**Gdzie:** [DeepTMHMM / BioLib](https://dtu.biolib.com/DeepTMHMM) · [TOPCONS](https://topcons.net/) · dodatkowo [TMHMM 2.0](https://services.healthtech.dtu.dk/services/TMHMM-2.0/), [SOSUI](https://harrier.nagahama-i-bio.ac.jp/sosui/) — **jeśli aktualnie dostępne**.

- [ ] Dla **TARGET** przeprowadź predykcję w **dwóch dostępnych narzędziach** (najlepiej DeepTMHMM + TOPCONS).
- [ ] Powtórz analizy dla **TEMPLATE**.
- [ ] Zapisz: przewidywaną liczbę i pozycje segmentów transmembranowych, orientację względem błony i ewentualne peptydy sygnałowe (jeżeli narzędzie je raportuje).
- [ ] Porównaj wyniki metod z profilem hydrofobowości oraz adnotacjami UniProt.
- [ ] Jeżeli oba białka są rozpuszczalne i nie przewiduje się segmentów TM, **zapisz wynik negatywny** — nie dopasowuj analizy do z góry założonej hipotezy.

**Tabela 2 — topologia**

| Metoda / cecha | TARGET | TEMPLATE | Zgodność / uwagi |
|---|---|---|---|
| DeepTMHMM: liczba TM | | | |
| DeepTMHMM: pozycje | | | |
| TOPCONS: liczba TM | | | |
| TOPCONS: pozycje | | | |
| Peptyd sygnałowy | | | |
| Adnotacja UniProt | | | |

**Oddaj:** Tabela 2, zrzuty/schematy topologii i interpretację zgodności między narzędziami.

## 4. Przewidywanie regionów nieuporządkowanych

**Gdzie:** [IUPred3](https://iupred3.elte.hu/) · opcjonalnie ANCHOR2 (jeżeli dostępny w tym środowisku).

- [ ] Wprowadź TARGET do IUPred3; wybierz **long disorder** (zapisz ustawienia).
- [ ] Powtórz analizę dla TEMPLATE.
- [ ] Zaznacz fragmenty o podwyższonej predykcji nieuporządkowania; zapisz granice reszt i przyjęty próg.
- [ ] Zachowaj wykresy. Po modelowaniu porównaj te fragmenty z regionami słabego pokrycia szablonem i **niskimi wartościami lokalnego QMEANDisCo**.
- [ ] Sprawdź, czy nieuporządkowane fragmenty mogą być końcówkami łańcucha, elastycznymi linkerami albo regionami nieobecnymi w strukturze PDB.

**Oddaj:** dwa profile i komentarz. Predykcja nieuporządkowania **nie jest** automatycznie dowodem błędu modelowania.

## 5. Predykcja struktury drugorzędowej

**Gdzie:** [JPred](https://www.compbio.dundee.ac.uk/jpred4/) · druga metoda do porównania: GOR IV, SOPMA, [PSSpred](https://zhanggroup.org/PSSpred/) albo PSIPRED (jeśli dostępne).

- [ ] Dla TARGET wykonaj predykcję **JPred + przynajmniej jedną inną metodą**. Sprawdź, czy JPred proponuje gotową strukturę z PDB zamiast predykcji; zanotuj tryb analizy.
- [ ] Dla TEMPLATE wykonaj predykcję przynajmniej jedną metodą z powyższych.
- [ ] Wyznacz procentowy udział helis α, nici β i pozostałych fragmentów; zaznacz rozbieżności między metodami.
- [ ] Dla TEMPLATE sprawdź **przypisanie struktury drugorzędowej z eksperymentalnego PDB** (np. DSSP / adnotacje PDB), a następnie porównaj je z predykcją.
- [ ] Po zbudowaniu modelu TARGET zestaw predykcje sekwencyjne z rzeczywistym przypisaniem H/E/C **wynikającym z modelu**; wyjaśnij, dlaczego nie muszą się pokrywać.

**Tabela 3 — struktura drugorzędowa**

| Cecha / metoda | TARGET | TEMPLATE | Interpretacja |
|---|---|---|---|
| JPred: α-helix (%) | | | |
| JPred: β-strand (%) | | | |
| Druga metoda: α-helix (%) | | | |
| Druga metoda: β-strand (%) | | | |
| PDB/DSSP: α-helix (%) | nie dotyczy przed modelowaniem | | |
| PDB/DSSP: β-strand (%) | nie dotyczy przed modelowaniem | | |

**Oddaj:** Tabela 3, profile lub mapy struktury drugorzędowej i komentarz dotyczący zgodności predykcji z eksperymentem.

## 6. Dodatkowa charakterystyka biologiczna

**Gdzie:** [UniProt](https://www.uniprot.org/) · [RCSB PDB](https://www.rcsb.org/) · opcjonalnie [InterPro](https://www.ebi.ac.uk/interpro/).

- [ ] Zapisz funkcję i lokalizację komórkową białka, domeny, ważne motywy i miejsca aktywne, jeżeli są znane.
- [ ] Sprawdź obecność kofaktorów, jonów, ligandów, mostków disiarczkowych, modyfikacji potranslacyjnych i peptydu sygnałowego.
- [ ] Ustal, czy białko jest monomerem, czy częścią oligomeru/kompleksu.
- [ ] Sprawdź, czy wybrana struktura PDB przedstawia **właściwy stan oligomeryczny, konformacyjny i ligandowy**, odpowiedni dla planowanej analizy.
- [ ] Zapisz, czy TARGET jest białkiem błonowym i czy model pojedynczego łańcucha bez błony/ligandów ma ograniczenia biologiczne.

**Oddaj:** 5–8 zdań z odnośnikami do adnotacji biologicznych.

---

## 7. Modelowanie homologiczne 3D — SWISS-MODEL (krok po kroku)

**Gdzie:** **[SWISS-MODEL — Start Modelling / Workspace](https://swissmodel.expasy.org/interactive)** · [oficjalny przykład: SOD](https://swissmodel.expasy.org/docs/examples) · [pełna instrukcja modelowania](https://swissmodel.expasy.org/docs/help).

**Wymaganie dydaktyczne:** zbuduj **co najmniej jeden model z ręcznie wskazanego szablonu eksperymentalnego**, a jeśli dostępne są odpowiednie alternatywy — **drugi model porównawczy** z innego szablonu albo z trybu automatycznego. Nie jest konieczne uruchamianie dodatkowego modelowania w AlphaFold.

### A. Wyszukiwanie i wybór TEMPLATE (tryb interaktywny — obowiązkowy)

1. [ ] Wejdź na stronę **Start Modelling**, zaloguj się, jeśli jest to wymagane lub potrzebne do zachowania projektów.
2. [ ] W polu **Target Sequence(s)** wklej **TARGET** w formacie FASTA lub podaj identyfikator UniProt. Upewnij się, że analizujesz **prawidłową sekwencję i jeden łańcuch**, chyba że prowadzący zatwierdzi kompleks.
3. [ ] Wpisz **Project Title**, np. `TARGET_accession_SWISSMODEL`; w razie potrzeby podaj e-mail.
4. [ ] Wybierz **Search for Templates**, a nie od razu **Build Model**, ponieważ chcemy **samodzielnie przeanalizować i wybrać szablon**.
5. [ ] Otwórz **Templates / Template Results**. Zidentyfikuj kandydatów wskazanych przez **BLAST i HHblits** w bazie SWISS-MODEL Template Library (**SMTL**).
6. [ ] Wybierz **co najmniej dwa szablony eksperymentalne**. Dla każdego zapisz **SMTL ID / PDB ID / chain**, identity, coverage, **GMQE przed modelowaniem**, metodę i rozdzielczość eksperymentalną, stan oligomeryczny, obecność ligandów oraz występujące luki.
7. [ ] Sprawdź, czy na liście są szablony z **AlphaFold DB** (`AlphaFold v2` / `AFDB search`). **Nie używaj ich w tym projekcie jako TEMPLATE.**
8. [ ] Zlokalizuj **TEMPLATE wybrany wcześniej przez BLASTp** i sprawdź, czy jest dostępny w SMTL. Jeżeli nie, wybierz inny homolog eksperymentalny spełniający wymagania lub uzgodnij tryb **User Template** z prowadzącym.
9. [ ] Zaznacz wybrany szablon w tabeli, obejrzyj jego strukturę w wizualizatorze i **przyrównanie TARGET–TEMPLATE**. Sprawdź obszary niepokryte, insercje/delecje, zakończenia sekwencji, stan oligomeryczny i ligandy.
10. [ ] Uzasadnij wybór szablonu na podstawie **identity, coverage, jakości eksperymentalnej, GMQE i biologicznej adekwatności**. **Najwyższe identity lub najwyższe GMQE nie są jedynymi kryteriami**.

### B. Budowa właściwego modelu

11. [ ] Dla zatwierdzonego, ręcznie zaznaczonego TEMPLATE kliknij **Build Model** lub odpowiednią akcję rozpoczęcia modelowania z wybranych szablonów.
12. [ ] Po zakończeniu przejdź do **Models / Model Results**. Sprawdź, **który TEMPLATE rzeczywiście został użyty** i jakie regiony TARGET udało się zmodelować.
13. [ ] Pobierz współrzędne w formacie **PDB** i zapisz niezmieniony plik jako `target_initial.pdb`; zachowaj również oryginalną nazwę pliku w archiwum źródłowym.
14. [ ] Pobierz **cały projekt / archiwum ZIP**, obejmujące model, przyrównanie i plik **`report.html`**. Zapisz link/identyfikator projektu, datę i ustawienia.
15. [ ] Jeżeli SWISS-MODEL wygeneruje kilka alternatywnych modeli, nie wybieraj automatycznie pierwszego — porównaj ich **szablony, coverage, GMQE i QMEANDisCo**.

### C. Model porównawczy (zalecany / obowiązkowy, jeśli dostępny odpowiedni TEMPLATE)

16. [ ] Zbuduj drugi model, wybierając **inny eksperymentalny TEMPLATE** z odpowiednim pokryciem **lub** uruchamiając tryb automatyczny (**Build Model** na ekranie początkowym).
17. [ ] Jeśli tryb automatyczny wybierze szablon z AlphaFold DB, **nie zaliczaj go jako modelu porównawczego w tej grupie**; wybierz porównawczy szablon eksperymentalny ręcznie.
18. [ ] Zapisz drugi model jako `target_alternative.pdb`, dokumentując jego rzeczywisty TEMPLATE. Porównaj pokrycie, geometrię i regiony różniące się od pierwszego modelu.

### D. Dodatkowy wariant dla chętnych

- [ ] **User Template:** jeśli trzeba użyć eksperymentalnego PDB spoza SMTL, kliknij **User Template → Add Template File...**, dostarcz **PDB**, a następnie **Build Model**. Oficjalna dokumentacja wskazuje, że w tym trybie plik **mmCIF nie jest obsługiwany** — nie należy tego utożsamiać z ogólną nieobsługą mmCIF w innych serwisach.
- [ ] **Target–Template Alignment mode:** można przekazać własne dopasowanie FASTA/Clustal, jeśli uzasadniona jest korekta luk; jest to rozszerzenie po uzgodnieniu z prowadzącym.

**Oddaj:** plik `target_initial.pdb`, pełne archiwum projektu (z `report.html`), dokumentację wyboru TEMPLATE i opcjonalnie `target_alternative.pdb`.

### Tabela 4 — wybór szablonów i wynik modelowania

| Parametr | TEMPLATE A / MODEL A | TEMPLATE B / MODEL B |
|---|---|---|
| PDB ID + chain / SMTL ID | | |
| Metoda eksperymentalna i rozdzielczość | | |
| Sequence identity (%) | | |
| Target coverage (%) | | |
| Alignment gaps / brakujące reszty | | |
| GMQE **przed** modelowaniem (0–1) | | |
| GMQE **po** modelowaniu (0–1) | | |
| QMEANDisCo global (0–1; jeśli dostępny) | | |
| QSQE (jeśli dotyczy oligomeru) | | |
| Liczba rzeczywiście zmodelowanych reszt | | |
| Obecność ligandów / stan oligomeryczny | | |
| Dlaczego model wybrano do refinement? | | |

## 8. Ocena modelu na podstawie raportu SWISS-MODEL

**Gdzie:** wyniki modelowania w **SWISS-MODEL Workspace** i pobrany **`report.html`**. Dokumentacja: [SWISS-MODEL — Model Evaluation](https://swissmodel.expasy.org/docs/help) · [QMEAN / QMEANDisCo](https://swissmodel.expasy.org/qmean/help).

- [ ] Otwórz **Model Results** i raport wybranego modelu; zapisz nazwę szablonu, pokrycie, stan oligomeryczny, ewentualne ligandy i ostrzeżenia.
- [ ] Zanotuj **GMQE (Global Model Quality Estimate)** — wartość **0–1**, uwzględniająca m.in. dopasowanie TARGET–TEMPLATE i pokrycie; wysoka wartość oznacza lepszą **oczekiwaną jakość**, a nie dowód eksperymentalny.
- [ ] Zanotuj **QMEANDisCo global** (0–1), jeżeli jest raportowany; to szacowanie jakości już zbudowanej struktury. Zapisz również ewentualne oszacowanie błędu.
- [ ] Otwórz wykres **Local Quality** (QMEANDisCo per residue) i wskaż regiony o obniżonej jakości, szczególnie fragmenty z wynikiem **poniżej około 0,6**. To wskazówka interpretacyjna, **nie absolutna granica poprawności**.
- [ ] Zmień kolorowanie modelu na **Confidence / Quality** i zobacz, gdzie w strukturze 3D znajdują się problematyczne regiony.
- [ ] Sprawdź, czy raport zawiera **ostrzeżenia**, braki pokrycia, nietypową geometrię lub uwagi o insercjach/delecjach.
- [ ] Jeśli model dotyczy biologicznego oligomeru, zapisz **QSQE** i opisz znaczenie wyboru zespołu biologicznego. Dla prostego monomeru QSQE może być **nieadekwatny / niedostępny**.
- [ ] **Nie wymagaj historycznego QMEAN Z-score, jeśli serwis go nie obliczył.** Według aktualnej dokumentacji SWISS-MODEL ta metryka jest **przestarzała (deprecated)**; preferowane są GMQE i QMEANDisCo.
- [ ] Zapisz, czy *GMQE przed modelowaniem* i *GMQE po modelowaniu* są identyczne; wyjaśnij, dlaczego wartości mogą się różnić.

**Uwaga:** dla modelu z **eksperymentalnego TEMPLATE** korzystamy ze standardowej oceny SWISS-MODEL. W przypadku szablonów z AlphaFold DB serwis stosuje odmienną procedurę i nie oblicza standardowego QMEANDisCo global — **takich szablonów nie dopuszczamy w tej grupie**.

**Oddaj:** **Tabela 4** z pełnymi wynikami, wykres jakości lokalnej, ilustrację struktury pokolorowanej według jakości, krótki komentarz: **co w modelu jest wiarygodne, a czego nie da się ocenić z samego raportu?**

## 9. Wizualizacja i porównanie 3D: TARGET vs TEMPLATE

**Gdzie:** [PyMOL](https://pymol.org/) (preferowany) · opcjonalnie RasMol/RasTop do podstaw wizualizacji · [SWISS-MODEL Structure Comparison](https://swissmodel.expasy.org/comparison/) · [TM-align](https://zhanggroup.org/TM-align/).

- [ ] Otwórz w PyMOL **eksperymentalny TEMPLATE**, `target_initial.pdb` i — jeśli zbudowano — `target_alternative.pdb`.
- [ ] Wybierz właściwy łańcuch TEMPLATE; zidentyfikuj reszty niezwiązane z badaną sekwencją oraz ligandy. **Nie modyfikuj oryginalnego pliku źródłowego.**
- [ ] Wyświetl **cartoon**, pokoloruj białka odmiennie, wyróżnij helisy, nici β, pętle i regiony niepokryte przez TEMPLATE.
- [ ] Wykonaj superpozycję TARGET–TEMPLATE (np. [PyMOL `align`](https://pymolwiki.org/index.php/Align)), a następnie zapisz **RMSD, liczbę dopasowanych atomów/reszt i ich zakresy sekwencyjne**.
- [ ] Wykonaj również porównanie w TM-align lub SWISS-MODEL Structure Comparison (jeżeli dostępne); zapisz **TM-score** wraz z informacją, względem której struktury jest znormalizowany, oraz pokrycie dopasowania.
- [ ] Zaznacz pętle, regiony insercji/delecji i fragmenty o największych różnicach strukturalnych.
- [ ] Jeśli powstały dwa modele SWISS-MODEL, porównaj **MODEL A vs MODEL B** w tym samym zakresie reszt; opisz wpływ wyboru szablonu.

**Tabela 5 — podobieństwo strukturalne**

| Porównanie | Zakres reszt / liczba atomów | RMSD (Å) | TM-score (jeśli wykonano) | Uwagi |
|---|---|---|---|---|
| MODEL A vs TEMPLATE A | | | | |
| MODEL A vs TEMPLATE B | | | | |
| MODEL A vs MODEL B (jeśli dostępny) | | | | |

**Oddaj:** Tabela 5, obrazy TEMPLATE i MODEL A oraz ich superpozycji (plus model alternatywny, jeśli wykonano). **RMSD bez liczby dopasowanych atomów i zakresu porównania jest niepełną informacją.**

## 10. Niezależna walidacja modelu i identyfikacja *hot spots*

**Gdzie:** [MolProbity — walidacja stereochemii](https://molprobity.biochem.duke.edu/) · [SWISS-MODEL Structure Assessment](https://swissmodel.expasy.org/assess) · [QMEAN](https://swissmodel.expasy.org/qmean/) · [ProSA-web](https://prosa.services.came.sbg.ac.at/prosa.php) · [RCSB: Structure Validation and Quality](https://www.rcsb.org/docs/additional-resources/structure-validation-and-quality). **[PDBsum](https://www.ebi.ac.uk/pdbsum/)** traktuj jako źródło uzupełniające, jeśli będzie dostępne.

- [ ] Prześlij **oryginalny `target_initial.pdb`** do **MolProbity**; wykonaj pełną kontrolę geometrii.
- [ ] Zapisz **Ramachandran favored / allowed / outliers**, **clashscore**, **rotamer outliers**, a jeśli raportowane — nietypowe wiązania, kąty, cis-peptydy i inne problemy stereochemiczne.
- [ ] Wykonaj **QMEAN/QMEANDisCo** jako ocenę dodatkową lub zapisz identycznie zdefiniowane wartości z SWISS-MODEL; rozróżnij **ocenę w raporcie modelowania** od **niezależnego sprawdzenia geometrii w MolProbity**.
- [ ] Prześlij model do **ProSA-web**; zapisz **Z-score** oraz lokalny profil jakości, jeśli analiza się powiodła.
- [ ] Nałóż fragmenty o niskim lokalnym QMEANDisCo na wykresy z etapów 2–5: hydrofobowość, nieuporządkowanie, struktura drugorzędowa i różnice TARGET–TEMPLATE.
- [ ] Sporządź **listę *hot spots***: podaj numery reszt, rodzaj problemu, źródło dowodu, prawdopodobną przyczynę oraz możliwą metodę poprawy.
- [ ] Wskaż do **trzech lub więcej rzeczywiście problematycznych** regionów, jeśli występują. Jeśli model wygląda dobrze, **nie wymyślaj** trzech problemów na siłę — uzasadnij brak wyraźnych defektów.
- [ ] Sprawdź, czy złe wyniki nie wynikają z błędnej preparatyki pliku, brakujących atomów, nieobecnych ligandów, nadmiarowych łańcuchów albo niejednoznacznej numeracji.

**Tabela 6 — walidacja pierwotnego modelu**

| Parametr | Wartość | Narzędzie / uwagi |
|---|---|---|
| QMEANDisCo global | | |
| Minimum / problematyczny zakres lokalnego QMEANDisCo | | |
| Ramachandran favored (%) | | |
| Ramachandran outliers (%) | | |
| Clashscore | | |
| Rotamer outliers (%) | | |
| ProSA Z-score | | |
| Liczba i pozycje *hot spots* | | |

**Oddaj:** Tabela 6, wykres Ramachandrana, co najmniej jeden raport niezależnej walidacji, ilustrację lokalizacji *hot spots* i krótką dyskusję błędów. **PDBsum nie powinien być jedynym lub obowiązkowym źródłem walidacji: jego publiczny serwis może być niedostępny.**

## 11. Refinement / minimalizacja energii (obowiązkowa)

**Gdzie:** [GalaxyRefine](https://galaxy.seoklab.org/cgi-bin/submit.cgi?type=REFINE) **albo** [YASARA Energy Minimization Server](https://www.yasara.org/minimizationserver.htm).

- [ ] Zachowaj **niezmieniony** plik `target_initial.pdb` i oddzielnie przygotuj kopię wejściową do refinement.
- [ ] Upewnij się, że model ma prawidłową sekwencję i numerację. **GalaxyRefine nie akceptuje luk w środku struktury**; jeśli model ma niezamodelowane odcinki, nie „naprawiaj” ich przez ciche usuwanie reszt — skonsultuj zakres refinement z prowadzącym.
- [ ] **GalaxyRefine:** otwórz formularz, wpisz *Job name*, opcjonalnie e-mail, wgraj model w polu **Model Structure to be refined → PDB File**, a następnie wybierz **Submit**. Zachowaj identyfikator zadania i pobierz udostępnione modele/raporty.
- [ ] **Lub YASARA:** otwórz serwer, wpisz e-mail, prześlij PDB i wybierz **Submit**. Pobierz wynik wraz z informacjami o zastosowanej minimalizacji. **Uwaga: YASARA umieszcza wyniki w publicznym obszarze pobierania — nie przekazuj danych poufnych.**
- [ ] Jeśli serwer zwróci kilka modeli, **wybierz model końcowy dopiero po porównaniu jakości**, a nie automatycznie pierwszy plik na liście.
- [ ] Zapisz datę, nazwę i wersję metody (jeżeli podana), parametry, plik wejściowy, identyfikator zadania, kryterium wyboru i wynikowy plik `target_refined.pdb`.
- [ ] Jeżeli model zawiera istotne biologicznie kofaktory, metale lub ligandy, sprawdź, czy były uwzględnione podczas refinement — brak odpowiedniej parametryzacji ogranicza interpretację wyniku.

**Ważne:** SWISS-MODEL wykorzystuje w swoim silniku ProMod3 **wewnętrzną lokalną minimalizację energii**, ale nie zwalnia to z wymaganego w projekcie **dodatkowego, niezależnego refinementu**. Poprawa geometryczna nie jest równoznaczna z dowodem poprawnego fałdu lub stabilności termodynamicznej.

**Oddaj:** `target_initial.pdb`, `target_refined.pdb`, pełny raport refinement i krótkie uzasadnienie wyboru finalnego modelu.

## 12. Ponowna walidacja — czy refinement rzeczywiście poprawił model?

- [ ] Ponownie wykonaj **MolProbity**, **QMEAN/QMEANDisCo** oraz **ProSA-web** (o ile użyto go dla modelu pierwotnego) dla `target_refined.pdb`.
- [ ] Porównaj **te same metryki**, liczone w możliwie tych samych wersjach narzędzi, z identycznie przygotowanymi strukturami.
- [ ] W PyMOL wykonaj superpozycję **`target_initial.pdb` vs `target_refined.pdb`**; oblicz RMSD z zapisaną liczbą atomów i zakresem reszt.
- [ ] Ponownie porównaj model refined z **tym samym eksperymentalnym TEMPLATE** w identycznym zakresie reszt (RMSD, opcjonalnie TM-score).
- [ ] Sprawdź, czy wcześniej zidentyfikowane *hot spots* uległy poprawie oraz czy **nie pojawiły się nowe problemy**.
- [ ] Jeśli program podaje energię układu, zapisuj **jednostki, pole siłowe, protonację, środowisko i protokół**. Porównuj energie **wyłącznie przy zgodnym protokole i układzie** — nie łącz wartości energii z różnych programów.
- [ ] **Nie przepisuj GMQE z pierwotnego raportu SWISS-MODEL jako nowej oceny modelu po refinement.** GMQE dotyczy modelowania homologicznego z konkretnym szablonem i dopasowaniem, nie jest uniwersalnym wskaźnikiem geometrycznym do porównywania dowolnych plików PDB.
- [ ] Oceń oddzielnie: **geometrię**, **podobieństwo do TEMPLATE**, **lokalną jakość modelu** i **czego nie wiadomo o stabilności**.

**Tabela 7 — obowiązkowe porównanie modelu przed refinement i po nim**

| Parametr | Model pierwotny | Model po refinement | Zmiana / komentarz |
|---|---|---|---|
| Ramachandran favored (%) | | | |
| Ramachandran outliers (%) | | | |
| Clashscore | | | |
| Rotamer outliers (%) | | | |
| QMEANDisCo global (ta sama metoda) | | | |
| Problematic local QMEANDisCo regions | | | |
| ProSA Z-score (jeśli wykonano) | | | |
| RMSD względem TEMPLATE (ten sam zakres) | | | |
| TM-score względem TEMPLATE (jeśli wykonano) | | | |
| RMSD initial vs refined | — | | |
| Liczba / charakter *hot spots* | | | |
| Energia w tym samym protokole (jeśli dostępna) | | | |

**Oddaj:** Tabela 7, ilustrację **przed / po** oraz krótką dyskusję. Nie zakładaj, że refinement musi poprawić wszystkie wskaźniki — możliwe są kompromisy między miarami.

---

## Co należy oddać? — lista końcowa

- [ ] **Raport PDF** w formacie krótkiej pracy naukowej: wprowadzenie, materiały i metody, wyniki, dyskusja, wnioski, literatura.
- [ ] **Tabele 1–7** (szczególnie TEMPLATE A/B, GMQE, QMEANDisCo, porównanie pierwotny/refined).
- [ ] **Ilustracje:** profile, topologia, nieuporządkowanie, struktura drugorzędowa, lokalne jakości SWISS-MODEL, Ramachandran, TEMPLATE, model pierwotny, superpozycja, model refined.
- [ ] **Dane:** `target.fasta`, `template.fasta`, `template.pdb`/`.cif`, wynik BLASTp i pliki dokumentujące wybór szablonu.
- [ ] **Komplet projektu SWISS-MODEL**: ZIP z oryginalnym `report.html`, przyrównaniem TARGET–TEMPLATE, informacją o szablonach i modelami PDB.
- [ ] **Modele 3D:** `target_initial.pdb`, `target_refined.pdb`, opcjonalnie `target_alternative.pdb`; oryginalne pliki z serwisów zachowaj w osobnym katalogu.
- [ ] **Walidacja i refinement:** raporty MolProbity, QMEAN/QMEANDisCo i ProSA (jeżeli wykonane), lista *hot spots*, dokumentacja zadania GalaxyRefine/YASARA.
- [ ] **Odtwarzalność:** nazwy i wersje programów (jeśli podane), daty, parametry, źródła i identyfikatory sekwencji / struktur / zadań.

### Pytania, na które raport musi odpowiedzieć

1. Dlaczego **TEMPLATE** był dobrym szablonem do modelowania TARGET? Czy wyniki **BLASTp** i wyszukiwania **SWISS-MODEL** prowadzą do podobnego wyboru?
2. Czy **GMQE i QMEANDisCo** wskazują na wiarygodny model? Jakie fragmenty są najmniej wiarygodne i dlaczego?
3. Jak zgodne są właściwości fizykochemiczne, profile, topologia, struktura drugorzędowa i struktura 3D TARGET oraz TEMPLATE?
4. Czy zmiana szablonu (jeśli zastosowano dwa) wpływa na strukturę i jakość modelu? Czy istotne są luki przyrównania lub różny stan oligomeryczny?
5. Które regiony mają nieprawidłową geometrię lub niską lokalną ocenę jakości? Jakie dane pozwalają je uznać za *hot spots*?
6. Czy niezależny **refinement** poprawił model? **Które wskaźniki wzrosły, a które się pogorszyły?** Czy zachowana została właściwa architektura białka?
7. Czy na podstawie wykonanych analiz można **udowodnić stabilność termodynamiczną**? Jeśli nie, jakie dalsze badania (np. dobrze zaprojektowana dynamika molekularna lub eksperyment) mogłyby to umożliwić?

**Zasada interpretacji:** **GMQE** (oczekiwana jakość modelu wynikająca m.in. z szablonu i pokrycia), **QMEANDisCo** (ocena jakości modelu), **MolProbity/Ramachandran** (jakość geometrii), **RMSD/TM-score** (podobieństwo strukturalne) i **stabilność termodynamiczna** to różne kwestie. **Żadna pojedyncza metryka nie dowodzi, że model jest „prawdziwy”.**

## Oficjalne materiały i przydatne odnośniki

### SWISS-MODEL — najważniejsze

1. [SWISS-MODEL — strona główna](https://swissmodel.expasy.org/)
2. [Start Modelling / Interactive Workspace](https://swissmodel.expasy.org/interactive)
3. [Oficjalna dokumentacja Help — tryby modelowania, wybór szablonu, GMQE, QMEANDisCo](https://swissmodel.expasy.org/docs/help)
4. [Oficjalne przykłady krok po kroku — SOD, oligomery, DeepView, hemoglobina](https://swissmodel.expasy.org/docs/examples)
5. [Oficjalne filmy szkoleniowe — sześć części](https://swissmodel.expasy.org/docs/tutorial) *(nagrania przygotowano w 2018 r.; wygląd interfejsu może się różnić)*
6. [SWISS-MODEL Repository — pomoc i interpretacja pokrycia / modeli](https://swissmodel.expasy.org/docs/repository_help)
7. [SWISS-MODEL Structure Comparison — porównanie modeli i struktur eksperymentalnych](https://swissmodel.expasy.org/comparison/)
8. [Structure Comparison — Help](https://swissmodel.expasy.org/comparison/help)
9. [SWISS-MODEL Structure Assessment](https://swissmodel.expasy.org/assess)
10. [QMEAN — serwer oceny jakości modeli](https://swissmodel.expasy.org/qmean/)
11. [QMEAN — opis metryk i interpretacja](https://swissmodel.expasy.org/qmean/help)
12. [SWISS-MODEL — publikacje do cytowania](https://swissmodel.expasy.org/docs/references)

### Analizy sekwencyjne, geometria i refinement

- [ExPASy ProtParam](https://web.expasy.org/protparam/) · [ProtScale](https://web.expasy.org/protscale/) · [Compute pI/Mw](https://web.expasy.org/compute_pi/)
- [NCBI BLASTp](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE=Proteins) · [UniProt](https://www.uniprot.org/) · [RCSB PDB](https://www.rcsb.org/) · [InterPro](https://www.ebi.ac.uk/interpro/)
- [DeepTMHMM](https://dtu.biolib.com/DeepTMHMM) · [TOPCONS](https://topcons.net/) · [TMHMM 2.0](https://services.healthtech.dtu.dk/services/TMHMM-2.0/)
- [IUPred3](https://iupred3.elte.hu/) · [JPred](https://www.compbio.dundee.ac.uk/jpred4/) · [PSSpred](https://zhanggroup.org/PSSpred/)
- [MolProbity](https://molprobity.biochem.duke.edu/) · [RCSB — Structure Validation and Quality](https://www.rcsb.org/docs/additional-resources/structure-validation-and-quality) · [ProSA-web](https://prosa.services.came.sbg.ac.at/prosa.php)
- [PyMOL](https://pymol.org/) · [PyMOL — align](https://pymolwiki.org/index.php/Align) · [TM-align](https://zhanggroup.org/TM-align/)
- [GalaxyRefine](https://galaxy.seoklab.org/cgi-bin/submit.cgi?type=REFINE) · [YASARA Energy Minimization Server](https://www.yasara.org/minimizationserver.htm)

### Przydatne publikacje i wzory raportowania

- **Przykład tabel porównawczych wskazany przez prowadzącego:** [DOI: 10.1002/jnr.23177](https://doi.org/10.1002/jnr.23177), tabele 1–3.
- **SWISS-MODEL:** Waterhouse i wsp., *SWISS-MODEL: homology modelling of protein structures and complexes*, *Nucleic Acids Research* (2018), [pełny tekst](https://pmc.ncbi.nlm.nih.gov/articles/PMC6030848/).
- **ProMod3 i QMEANDisCo:** zestaw referencji i DOI w [oficjalnej bibliografii SWISS-MODEL](https://swissmodel.expasy.org/docs/references).

**W przypadku niedostępności serwisu:** zapisz datę i problem; zgłoś go prowadzącemu. Nie podmieniaj samodzielnie wymaganej metody modelowania innym programem. Jeżeli brakuje wybranej metryki w aktualnym raporcie, zapisz **„niedostępna”**, zamiast wymyślać wartość.
