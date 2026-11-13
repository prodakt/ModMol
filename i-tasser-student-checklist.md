# Projekt indywidualny: modelowanie białka w I-TASSER

**Kurs:** Podstawy Modelowania Molekularnego  
**Grupa:** I-TASSER (bez AlphaFold)  
**Temat:** Od sekwencji aminokwasowej do zweryfikowanego modelu 3D po refinement  
**Forma:** praca indywidualna; wyniki TARGET porównywane z TEMPLATE

> **Cel:** opracować model struktury 3D wybranego białka, ocenić wiarygodność predykcji I-TASSER i jakość geometrii, wskazać problematyczne regiony (*hot spots*), przeprowadzić **niezależny refinement** i ocenić, czy struktura rzeczywiście uległa poprawie. **Minimalizacja energii ani poprawny wykres Ramachandrana nie są dowodem stabilności termodynamicznej białka.**

## Przed rozpoczęciem

Musisz mieć zatwierdzone:

- [ ] **TARGET:** pełna sekwencja białka (150–400 aa), identyfikator UniProt/NCBI i plik `target.fasta`.
- [ ] **TEMPLATE:** homolog o **eksperymentalnie** wyznaczonej strukturze w PDB; preferowane **60–95% sequence identity** i **query coverage ≥85%** (najlepiej ≥90%) w BLASTp. Zapisz PDB ID, łańcuch i metodę eksperymentalną.
- [ ] **Przyrównanie TARGET–TEMPLATE:** % identity, query coverage, alignment length, E-value, luki, zakres dopasowania oraz informacja o fragmentach bez współrzędnych w PDB.
- [ ] Pliki `template.fasta` (sekwencja właściwego homologu) i `template.pdb` lub `template.cif` (struktura odpowiedniego łańcucha).
- [ ] Krótka odpowiedź: **dlaczego ten homolog jest użytecznym punktem odniesienia do modelowania?**

**Jak działa I-TASSER?** To nie jest SWISS-MODEL. I-TASSER wyszukuje **własne szablony strukturalne** metodami threading (LOMETS), składa z ich fragmentów modele i udoskonala je w ramach własnej procedury. Wybrany przez Ciebie TEMPLATE jest **obowiązkową strukturą referencyjną** do porównania. **Nie zakładaj**, że I-TASSER automatycznie użyje właśnie tego szablonu. Można podać szablon jako dodatkowe ograniczenie w **Option I**, ale w projekcie podstawowym stosujemy **tryb automatyczny, bez wymuszania szablonu**, aby móc niezależnie porównać jego wybór z wynikami BLAST. Wyjątki tylko po uzgodnieniu z prowadzącym.

**Organizacja czasu:** uruchom zadanie I-TASSER **jak najwcześniej po zatwierdzeniu sekwencji** (etap 7), a podczas oczekiwania wykonuj etapy 1–6. Wynik może wymagać długiego przetwarzania i kolejki. Serwis prosi, aby nie zgłaszać następnego zadania przed ukończeniem poprzedniego. Nie odkładaj pobrania wyników: przykładowe strony wynikowe informują o przechowywaniu danych przez **60 dni**.

W etapach 1–5 analizuj **TARGET i TEMPLATE osobno, z identycznymi ustawieniami**. Odróżniaj pełną sekwencję TEMPLATE od reszt rzeczywiście obecnych w jego strukturze PDB. Dla każdego narzędzia notuj datę wykonania, parametry i wynik.

---

## 1. Podstawowe właściwości fizykochemiczne

**Gdzie:** [ExPASy ProtParam](https://web.expasy.org/protparam/); opcjonalnie [Compute pI/Mw](https://web.expasy.org/compute_pi/).

- [ ] Otwórz ProtParam, wklej sekwencję **TARGET** i uruchom obliczenia.
- [ ] Powtórz analizę dla **TEMPLATE**.
- [ ] Zapisz: długość, masę cząsteczkową, teoretyczne pI, skład aminokwasowy, liczbę reszt naładowanych dodatnio/ujemnie, współczynnik ekstynkcji, **instability index**, **aliphatic index** i **GRAVY**.
- [ ] Uzupełnij **Tabelę 1: TARGET vs TEMPLATE**; wskaż trzy najważniejsze podobieństwa/różnice i ich możliwe znaczenie biologiczne.

**Wzór tabel:** sposób zestawiania wyników pokazują **tabele 1–3 publikacji [DOI: 10.1002/jnr.23177](https://doi.org/10.1002/jnr.23177)** wskazanej przez prowadzącego. Opracuj **własne** tabele; nie kopiuj publikacji.

**Oddaj:** `table_01_physicochemical.csv` (lub tabelę w raporcie) i **3–5 zdań interpretacji**. Indeks niestabilności jest wskaźnikiem opartym na sekwencji; **nie** stanowi pomiaru trwałości termodynamicznej białka.

## 2. Profile właściwości wzdłuż sekwencji

**Gdzie:** [ExPASy ProtScale](https://web.expasy.org/protscale/); opcjonalnie Python / Google Colab.

- [ ] Wprowadź sekwencję **TARGET** do ProtScale.
- [ ] Oblicz profil **hydrofobowości** (Kyte–Doolittle), zapisując wielkość okna; porównaj co najmniej **dwa okna** (np. 9 i 19 aa).
- [ ] Oblicz profil **hydrofilowości** (np. Hopp–Woods, jeśli opcja jest dostępna).
- [ ] Oblicz co najmniej **jeden profil tendencji strukturalnej** (np. skłonność do helisy α / nici β, skale typu Chou–Fasman).
- [ ] Powtórz profile dla **TEMPLATE** przy identycznych skalach i parametrach.
- [ ] Porównaj lokalizację maksimów i minimów **z uwzględnieniem przyrównania sekwencji**; wskaż regiony zachowane i odmienne.

**Oddaj:** co najmniej **3 rodzaje profili** dla obu białek (hydrofobowość, hydrofilowość, tendencja strukturalna), podpisy ze skalą/oknem i interpretację. Profil tendencji aminokwasowej **nie zastępuje** predykcji struktury drugorzędowej.

## 3. Regiony transmembranowe i topologia

**Gdzie:** [DeepTMHMM](https://dtu.biolib.com/DeepTMHMM), [TOPCONS](https://topcons.cbr.su.se/); dodatkowo [TMHMM](https://services.healthtech.dtu.dk/services/TMHMM-2.0/) lub SOSUI, jeśli dostępne.

- [ ] Przeanalizuj **TARGET** w **dwóch dostępnych narzędziach** (preferowane DeepTMHMM + TOPCONS).
- [ ] Powtórz analizę dla **TEMPLATE**.
- [ ] Zapisz liczbę, pozycje i typ regionów transmembranowych, orientację/topologię i przewidywane peptydy sygnałowe, jeśli są raportowane.
- [ ] Porównaj wyniki narzędzi między sobą oraz z profilami hydrofobowości i adnotacjami UniProt.
- [ ] Jeśli nie ma przewidywanych helis TM, odnotuj **wynik negatywny** — nie dopasowuj interpretacji do oczekiwań.

**Oddaj:** **Tabela 2: topologia TARGET vs TEMPLATE** i schemat/ilustracje predykcji.

## 4. Regiony nieuporządkowane

**Gdzie:** [IUPred3](https://iupred3.elte.hu/); opcjonalnie ANCHOR2.

- [ ] W IUPred3 uruchom **long disorder** dla **TARGET**.
- [ ] Powtórz dla **TEMPLATE**.
- [ ] Wskaż pozycje fragmentów z podwyższonym wynikiem predykcji nieuporządkowania (zapisz próg, jeśli jest stosowany).
- [ ] Zachowaj wykresy, aby później zestawić je z miejscami niskiej **lokalnej wiarygodności I-TASSER** i z oceną geometrii modelu.

**Oddaj:** dwa profile nieuporządkowania i krótki komentarz. **Predykcja nieuporządkowania** i **niedokładność modelu** to różne kwestie; jedna nie dowodzi automatycznie drugiej.

## 5. Predykcja struktury drugorzędowej

**Gdzie:** [JPred](https://www.compbio.dundee.ac.uk/jpred4/) oraz **jedno inne dostępne narzędzie**, np. GOR IV, SOPMA, [PSSpred](https://zhanggroup.org/PSSpred/) lub PSIPRED.

- [ ] Wykonaj predykcję **TARGET** w **JPred + drugiej metodzie**.
- [ ] Wykonaj predykcję **TEMPLATE** co najmniej jedną z tych metod.
- [ ] Zapisz przewidywane helisy α, nici β i pozostałe stany; porównaj ich udział procentowy i położenie.
- [ ] Dla **TEMPLATE** porównaj wyniki predykcji z elementami struktury drugorzędowej przypisanymi ze **struktury eksperymentalnej** (PDB / DSSP).
- [ ] Po otrzymaniu raportu I-TASSER (etap 7) porównaj także jego **własną predykcję struktury drugorzędowej** z wcześniejszymi wynikami. Nie zakładaj, że wszystkie algorytmy są niezależne.

**Oddaj:** **Tabela 3: struktura drugorzędowa**, mapa elementów wzdłuż sekwencji i dyskusja różnic między metodami.

## 6. Dodatkowa charakterystyka biologiczna

**Gdzie:** [UniProt](https://www.uniprot.org/), [RCSB PDB](https://www.rcsb.org/).

- [ ] Sprawdź, czy białko zawiera domeny, motywy, miejsca aktywne, kofaktory, jony, wiązania disiarczkowe lub modyfikacje potranslacyjne.
- [ ] Określ, czy białko funkcjonuje jako monomer, czy jest częścią kompleksu.
- [ ] Sprawdź, czy eksperymentalny TEMPLATE zawiera wszystkie reszty, ligand(y), łańcuchy i elementy ważne dla funkcji.
- [ ] Zapisz, **jak brak partnerów lub kofaktorów może wpłynąć na interpretację monomerycznego modelu I-TASSER**.

**Oddaj:** **5–8 zdań** charakterystyki biologicznej oraz źródła adnotacji.

## 7. Modelowanie struktury 3D — I-TASSER

**Gdzie:** **[I-TASSER — oficjalny serwer](https://zhanggroup.org/I-TASSER/)**; [FAQ](https://zhanggroup.org/I-TASSER/FAQ.html); [przykład raportu](https://zhanggroup.org/I-TASSER/example/).

1. [ ] Otwórz **I-TASSER On-line Server**. Jeśli nie masz konta, wybierz **Registration**, zarejestruj się i przygotuj przypisane dane logowania.
2. [ ] W polu sekwencji wklej **TARGET w formacie FASTA** albo użyj opcji przesłania pliku `target.fasta`. Sprawdź, czy w sekwencji są tylko oczekiwane reszty aminokwasowe. W projekcie analizujemy **jedno białko / pojedynczy łańcuch**.
3. [ ] Wpisz **Email** i **Password** przypisane do konta I-TASSER.
4. [ ] W opcjonalnym polu **ID** wpisz jednoznaczny identyfikator, np. `TARGET_accession_ITASSER`.
5. [ ] **Option I: Assign additional restraints & templates** — w podstawowym projekcie **pozostaw nieaktywne** (automatyczny wybór szablonów). Jeśli prowadzący zaleci modelowanie wymuszone przez wskazany TEMPLATE, możliwe jest **Specify template without alignment → Type a PDB ID** albo dostarczenie szablonu z przyrównaniem. Takie zadanie musi być osobno oznaczone w raporcie.
6. [ ] **Option II: Exclude some templates** — **nie ustawiaj limitu identity ani nie wykluczaj szablonów** bez zgody prowadzącego. W szczególności próg 60% w tej opcji oznaczałby **wykluczenie bliskich homologów**, a nie wymóg ich użycia.
7. [ ] **Option III: Specify secondary structure** — pozostaw ustawienia domyślne; nie narzucaj danych z własnych predykcji, chyba że prowadzący zleci taki eksperyment.
8. [ ] Sprawdź ustawienie **Keep my results public**. Jeśli wybierzesz wynik prywatny, **zapisz klucz dostępu**. Nie przesyłaj sekwencji poufnych. Nie ujawniaj hasła do konta w raporcie.
9. [ ] Uruchom modelowanie przyciskiem wysłania zadania I-TASSER; zapisz **ID zadania, datę i adres strony statusu**. Poczekaj na wiadomość e-mail / zakończenie obliczeń; w tym czasie wykonuj inne analizy. **Nie wysyłaj ponownie tego samego zadania bez potrzeby.**
10. [ ] Otwórz **stronę wyników**. Obejrzyj do pięciu modeli (`Model 1–5`), ich **C-score**, raportowane szablony i przewidywaną strukturę drugorzędową.
11. [ ] Pobierz **Model 1** i pozostałe dostępne modele (zazwyczaj pliki **PDB**). Zachowaj także pełne archiwum wyników, jeśli oferowany jest plik **`.tar.bz2`**, oraz kopię/zrzut strony raportu.
12. [ ] Nazwij kopię pierwszego modelu `target_initial.pdb` (**oryginał też zachowaj**), odnotuj ewentualne braki atomów/reszt oraz dokładną numerację. Przed refinement nie zmieniaj go ręcznie bez dokumentacji.

**Ważne:** określenie „refinement” występuje już w pełnej nazwie I-TASSER (*Iterative Threading ASSEmbly Refinement*). **Etap 11 projektu oznacza dodatkowy, odrębny refinement** otrzymanego modelu, którego efekty należy samodzielnie zwalidować.

**Oddaj:** ID i adres zadania, datę uruchomienia, pełny raport/archiwum, model 1, inne dostępne modele oraz opis ustawień i decyzji dotyczących **Option I–III**.

## 8. Interpretacja raportu jakości I-TASSER

**Gdzie:** strona wyników I-TASSER oraz jego oficjalna [instrukcja interpretacji](https://zhanggroup.org/I-TASSER/about.html) i [FAQ](https://zhanggroup.org/I-TASSER/FAQ.html).

- [ ] **C-score dla każdego dostępnego modelu:** zapisz wartości. Typowy zakres to około **−5 do +2**; im wyższy C-score, tym większa szacowana wiarygodność **w ramach tej metody**. **To nie jest procent poprawności.**
- [ ] **Estimated TM-score (Model 1):** zapisz wartość i podaną niepewność, jeśli widoczna. Jest to **oszacowanie** podobieństwa do nieznanej struktury natywnej, a **nie** wynik rzeczywistego porównania z wybranym TEMPLATE.
- [ ] **Estimated RMSD (Model 1):** zapisz wartość w Å i niepewność. To również **oszacowanie** względem hipotetycznej struktury natywnej, a **nie** RMSD policzone po superpozycji z TEMPLATE.
- [ ] **Ranking modeli:** wskaż, ile modeli wygenerowano, który ma rangę 1 i czym różni się od pozostałych. Modele są porządkowane głównie według wielkości klastrów; **C-score modeli 2–5 nie należy wykorzystywać do bezpośredniego szeregowania ich jakości w taki sam sposób jak modelu 1**.
- [ ] **Top 10 threading templates used by I-TASSER (LOMETS):** zanotuj co najmniej trzy pierwsze PDB ID i dostępne kolumny jakości dopasowania (np. coverage, Z-score). Sprawdź, **czy wśród nich jest Twój wcześniej wybrany TEMPLATE**.
- [ ] **Predicted secondary structure / predicted solvent accessibility:** zapisz lub pokaż odpowiednie profile i porównaj je z etapami 2, 4 i 5.
- [ ] **Predicted normalized B-factor / local structure accuracy profile:** obejrzyj dostępne profile, zaznacz fragmenty o potencjalnie słabszej przewidywalności/mobilności. **Nie traktuj przewidywanego znormalizowanego B-factora jak eksperymentalnego B-factora ani jak pLDDT.**
- [ ] **Proteins structurally close to the target in PDB:** zanotuj najbardziej podobne struktury wyznaczone przez TM-align; porównaj je z wcześniejszym BLASTp i wybranym TEMPLATE.

**I-TASSER nie podaje klasycznych metryk AlphaFold pLDDT, PAE i pTM ani GMQE znanego z SWISS-MODEL.** W jego własnym raporcie kluczowe są **C-score, Estimated TM-score/RMSD, szablony LOMETS, lokalna analiza struktury** oraz niezależna walidacja z kolejnego etapu.

**Tabela 4 — obowiązkowy skrót raportu I-TASSER:**

| Wskaźnik | Model 1 | Model 2 | Model 3 | Model 4 | Model 5 |
|---|---|---|---|---|---|
| Dostępny? (tak/nie) | | | | | |
| C-score | | | | | |
| Estimated TM-score (jeśli podany) | | n/d | n/d | n/d | n/d |
| Estimated RMSD, Å (jeśli podany) | | n/d | n/d | n/d | n/d |
| Uwagi o modelu | | | | | |

**Oddaj:** **Tabela 4**, listę szablonów LOMETS, fragmenty raportu dokumentujące oceny jakości oraz **3–5 zdań**: czy wybrany MODEL 1 jest uzasadnionym punktem wyjścia do walidacji/refinement?

## 9. Wizualizacja i porównanie TARGET z TEMPLATE

**Gdzie:** [PyMOL](https://pymol.org/); do podstaw wizualizacji opcjonalnie RasMol/RasTop; [TM-align](https://zhanggroup.org/TM-align/) do porównania struktur.

- [ ] Otwórz w PyMOL **eksperymentalną strukturę TEMPLATE** i `target_initial.pdb` (model I-TASSER).
- [ ] Usuń z widoku nieistotne łańcuchy TEMPLATE lub wybierz właściwy łańcuch; zachowaj kopię niezmienionej struktury.
- [ ] Wyświetl obie struktury jako **cartoon**, zastosuj różne kolory i wykonaj superpozycję odpowiadających sobie fragmentów.
- [ ] Zapisz **RMSD** po superpozycji, **liczbę uwzględnionych atomów/reszt** i zakres sekwencji użyty do porównania.
- [ ] Wykonaj porównanie przez TM-align i zapisz **rzeczywisty TM-score** (normalizację należy odnotować), a także długość dopasowania.
- [ ] Wyraźnie odróżnij **TM-score modelu względem TEMPLATE** od **Estimated TM-score** raportowanego przez I-TASSER.
- [ ] Wskaż co najmniej dwa regiony różniące się między modelem a TEMPLATE, jeśli występują; zwróć uwagę na insertions/deletions i pętle.
- [ ] Przygotuj zrzuty/ilustracje: TEMPLATE, model TARGET i ich superpozycja.

**Oddaj:** **Tabela 5: strukturalne porównanie TARGET–TEMPLATE**, ilustracje i opis regionów różniących się.

## 10. Niezależna walidacja jakości modelu

**Gdzie:** [MolProbity](https://molprobity.biochem.duke.edu/), [SWISS-MODEL Structure Assessment / QMEAN](https://swissmodel.expasy.org/assess), [ProSA-web](https://prosa.services.came.sbg.ac.at/prosa.php); **PDBsum** jako opcja, jeśli serwis jest dostępny.

- [ ] Wgraj **pierwotny model I-TASSER** (`target_initial.pdb`) do **MolProbity**.
- [ ] Oceń geometrię: **Ramachandran favored / allowed / outliers**, **clashscore**, **rotamer outliers**, ewentualnie błędne wiązania, kąty lub cis-peptydy.
- [ ] Wykonaj niezależną ocenę w **QMEAN/QMEANDisCo**; zapisz globalną ocenę oraz lokalny profil jakości, jeśli dostępny.
- [ ] Wykonaj **ProSA-web**; zachowaj **Z-score** i wykres oceny lokalnej, jeżeli serwis przyjął plik.
- [ ] Porównaj niskiej jakości fragmenty z predykcjami nieuporządkowania, strukturą drugorzędową i różnicami TARGET–TEMPLATE.
- [ ] Sporządź **tabelę hot spots**: numer(y) reszt, rodzaj problemu, źródło dowodu, możliwa przyczyna i propozycja poprawy. Wskaż **do trzech lub więcej** uzasadnionych regionów; **nie wymyślaj brakujących**.
- [ ] Jeśli struktura zawiera niekanoniczne reszty, przerwy w łańcuchu lub brakujące atomy, zapisz ograniczenia procedury walidacyjnej i przygotowania plików.

**Oddaj:** **Tabela 6: walidacja modelu pierwotnego**, wykres Ramachandrana, raporty i tabelę hot spots.

## 11. Refinement / minimalizacja energii

**Gdzie:** **[GalaxyRefine](https://galaxy.seoklab.org/cgi-bin/submit.cgi?type=REFINE)** (preferowany dla poprawy geometrii białka) **lub** [YASARA Energy Minimization Server](https://www.yasara.org/minimizationserver.htm) (minimalizacja energii w ustalonym protokole).

- [ ] Zachowaj niezmieniony oryginał modelu I-TASSER jako `target_initial.pdb`.
- [ ] Sprawdź zgodność sekwencji, liczby i numeracji reszt oraz braków we współrzędnych; **GalaxyRefine wymaga pojedynczego łańcucha i nie akceptuje luk w środku struktury**. Jeżeli model jest niekompletny lub zawiera wiele łańcuchów, poproś prowadzącego o wybór odpowiedniej procedury; **nie usuwaj luk bez dokumentacji**.
- [ ] **GalaxyRefine:** otwórz stronę, wpisz **Job name** (np. `TARGET_ITASSER_refine`) i opcjonalny e-mail; w sekcji **Model Structure to be refined → PDB File** załaduj `target_initial.pdb`, wybierz **Submit**. Zapisz stronę/identyfikator zadania. Po ukończeniu pobierz dostępne modele refined i raport. Wybór konkretnego modelu refined **uzasadnij porównaniem jakości**, nie wyłącznie kolejnością.
- [ ] **Albo YASARA:** otwórz serwis minimalizacji, wpisz e-mail, prześlij plik PDB i wybierz **Submit**. Zachowaj link do wyników, protokół oraz pliki wyjściowe. **Uwaga: wyniki są umieszczane w publicznym obszarze pobierania**; serwis może wymagać YASARA View do odczytu/konwersji formatu wynikowego.
- [ ] Zapisz: nazwę metody, adres serwisu, datę, wejściowy plik, ustawienia, liczbę wygenerowanych struktur i kryterium wyboru finalnego modelu.
- [ ] Zachowaj wybrany model jako `target_refined.pdb`, a oryginalne pliki i raport refinementu w osobnym katalogu.

**Ważne:** refinement może poprawić ułożenie łańcuchów bocznych, geometrię i lokalne oddziaływania; **nie gwarantuje poprawnego fałdu globalnego** ani stabilności termodynamicznej. Jeśli używasz **YASARA**, ewentualne zmiany energii interpretuj jedynie w kontekście **tego samego pola siłowego, układu i protokołu obliczeń**.

**Oddaj:** model **przed** i **po** refinement, komplet wyników serwisu oraz krótki opis wyboru modelu końcowego.

## 12. Ponowna walidacja: czy refinement poprawił model?

- [ ] Ponownie wykonaj **MolProbity, QMEAN/QMEANDisCo** i (jeśli analizowano model pierwotny) **ProSA-web** dla `target_refined.pdb`.
- [ ] Zestaw wartości **tych samych metryk** uzyskanych dla modelu pierwotnego i końcowego; zweryfikuj, czy liczba atomów i reszt oraz zastosowane procedury są porównywalne.
- [ ] Wykonaj superpozycję `target_initial.pdb` i `target_refined.pdb`, oblicz **RMSD** na porównywalnym zestawie atomów i zaznacz największe przemieszczenia.
- [ ] Ponownie porównaj model refined z eksperymentalnym TEMPLATE: RMSD i — najlepiej — TM-score na **tym samym zakresie** co wcześniej.
- [ ] Oceń, czy zniknęły wcześniej zidentyfikowane hot spots oraz czy nie powstały **nowe problemy**.
- [ ] Jeżeli serwis podaje energię, zapisuj ją wraz z metodą i jednostkami. **Nie porównuj bezpośrednio energii pochodzących z różnych programów ani różnie przygotowanych układów.**
- [ ] **Nie traktuj C-score z raportu I-TASSER jako ponownie obliczonej oceny modelu refined.** To ocena pierwotnej predykcji wykonanej przez I-TASSER.

**Tabela 7 — obowiązkowe porównanie:**

| Parametr | Model pierwotny | Model po refinement |
|---|---|---|
| Ramachandran favored (%) | | |
| Ramachandran outliers (%) | | |
| Clashscore | | |
| Rotamer outliers (%) | | |
| QMEAN / QMEANDisCo (ta sama metryka) | | |
| ProSA Z-score (jeśli wykonano) | | |
| RMSD względem TEMPLATE (ta sama selekcja) | | |
| TM-score względem TEMPLATE (jeśli wykonano) | | |
| RMSD względem modelu pierwotnego | — | |
| Liczba/charakter hot spots | | |
| Energia w tym samym protokole (jeśli dostępna) | | |

**Wniosek obowiązkowy:** czy poprawiła się **geometria**, czy zmieniło się **podobieństwo do TEMPLATE**, a co nadal pozostaje **niepewne**? Czy dostępne dane potwierdzają stabilność termodynamiczną? (Zazwyczaj **nie**; wymaga ona dodatkowych analiz.)

---

## Co należy oddać?

- [ ] **Raport PDF**: wstęp, metody, wyniki, dyskusja, wnioski i bibliografia.
- [ ] **Tabele 1–7** i ilustracje: właściwości, profile, topologia, nieuporządkowanie, struktura drugorzędowa, wyniki I-TASSER, Ramachandran, superpozycje 3D.
- [ ] **Dane wejściowe**: `target.fasta`, `template.fasta`, `template.pdb`/`.cif`, wynik BLASTp i uzasadnienie wyboru TEMPLATE.
- [ ] **Wyniki I-TASSER**: ID zadania, link, zapis strony, raport C-score / Estimated TM-score / Estimated RMSD, lista szablonów LOMETS, modele PDB i całe archiwum (jeśli udostępnione).
- [ ] **Modele 3D:** `target_initial.pdb`, `target_refined.pdb` oraz oryginalne pliki ze wszystkich etapów.
- [ ] **Raporty walidacji i refinementu**: MolProbity, QMEAN, ProSA, tabela hot spots, porównanie przed/po, daty, użyte parametry.
- [ ] **Jednoznaczna dokumentacja** metod, wersji (jeśli podane), źródeł danych i identyfikatorów analiz.

### Pytania, na które raport musi odpowiedzieć

1. Czy **TARGET** i **TEMPLATE** mają porównywalne właściwości fizykochemiczne, profile i strukturę przestrzenną? Co wynika z różnic?
2. Czy I-TASSER użył szablonów zgodnych z wcześniejszym wynikiem BLASTp? Czy **wybrany TEMPLATE** występuje w zestawie LOMETS?
3. Co oznaczają **C-score, Estimated TM-score i Estimated RMSD**, a czego **nie** mówią o rzeczywistej dokładności struktury?
4. Czy pierwotny model 3D jest wiarygodny? Które regiony są problematyczne i na jakiej podstawie je wskazano?
5. Czy dodatkowy **refinement** poprawił model? Które wskaźniki polepszyły się, a które pogorszyły?
6. Czy przedstawione dane uprawniają do wniosku, że białko jest **stabilne termodynamicznie**? Jakie dodatkowe badania (np. właściwie zaprojektowana symulacja dynamiki molekularnej, eksperymenty) byłyby potrzebne?

**Najważniejsza zasada:** **pewność predykcji (C-score)**, **podobieństwo strukturalne (TM-score/RMSD)**, **geometria (MolProbity / Ramachandran)** i **stabilność termodynamiczna** to **cztery różne kwestie**. Nie sprowadzaj ich do jednej „oceny jakości”.

## Oficjalne materiały uzupełniające

- [I-TASSER: formularz i rejestracja](https://zhanggroup.org/I-TASSER/)
- [I-TASSER: FAQ, interpretacja C-score, szablonów i modeli](https://zhanggroup.org/I-TASSER/FAQ.html)
- [I-TASSER: opis wyników i ograniczeń metody](https://zhanggroup.org/I-TASSER/about.html)
- [I-TASSER: przykładowy kompletny raport](https://zhanggroup.org/I-TASSER/example/)
- [TM-align: porównanie struktur](https://zhanggroup.org/TM-align/)
- [GalaxyRefine: formularz i wymagania dotyczące PDB](https://galaxy.seoklab.org/cgi-bin/submit.cgi?type=REFINE)
- [YASARA: Energy Minimization Server i protokół](https://www.yasara.org/minimizationserver.htm)

**Uwaga dotycząca usług internetowych:** serwisy naukowe mogą zmieniać interfejsy, limity, dostępność i czas wykonywania zadań. Jeśli narzędzie nie działa, **udokumentuj problem i skontaktuj się z prowadzącym** — nie zastępuj samodzielnie wyznaczonej metody inną metodą modelowania.
