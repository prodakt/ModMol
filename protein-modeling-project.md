# INSTRUKCJA
## Individual Protein Modeling Project

**From Protein Sequence to a Validated and Refined 3D Structure**  
*Kurs Podstaw Modelowania Molekularnego — wspólna instrukcja przygotowania projektu i raportu naukowego*

> **Cel:** samodzielnie wybrać sekwencję białka (**TARGET**), znaleźć homologiczny szablon eksperymentalny (**TEMPLATE**), przeprowadzić charakterystykę sekwencji i modelowanie struktury 3D, ocenić jakość i wiarygodność modelu, zidentyfikować problematyczne regiony, wykonać refinement oraz ponownie zweryfikować wynik. **Najważniejszym zadaniem jest zbudowanie uzasadnionej, krytycznej argumentacji na podstawie wyników.**

Projekt wykonuje **każdy student indywidualnie**. Cztery grupy laboratoryjne korzystają z różnych głównych serwisów modelowania.

**Ten dokument jest instrukcją jak zaplanować i napisać przekonujący raport**, co powinien zawierać i na jakie pytania odpowiadać. Nie jest to instrukcja klikania w konkretnym serwisie. Szczegółowe działania techniczne znajdują się w osobnych checklistach dla SWISS-MODEL, I-TASSER i AlphaFold Server (oraz w materiałach przypisanych pozostałym grupom). Z tej strony korzystasz podczas **całego** projektu i podczas pisania raportu.

**Główne pytanie projektu:** *W jakim stopniu mój model 3D jest wiarygodną reprezentacją analizowanego białka, w których fragmentach jest dobrze lub słabo uzasadniony i czy przeprowadzony refinement rzeczywiście go poprawił?*

**Ważne:** metodami obliczeniowymi nie można jednoznacznie **udowodnić**, że nieznana struktura jest poprawna. Możesz jednak zgromadzić **wiele niezależnych, ilościowych przesłanek** potwierdzających wiarygodność modelu i jasno opisać ograniczenia. Dobry raport nie ukrywa wyników niezgodnych z oczekiwaniami.

## 0. Co właściwie masz wykazać w raporcie?

Nie chodzi o zebranie zrzutów ekranu z kolejnych stron internetowych. Tworzysz **krótki raport badawczy** odpowiadający na serię pytań. Wszystkie analizy mają dostarczyć danych do jednego wniosku o jakości struktury.

| Pytanie badawcze | Jakie dowody przedstawiasz? | Czego te wyniki same nie dowodzą? |
|---|---|---|
| Czy TARGET można modelować w oparciu o znane struktury? | BLAST, identity, pokrycie, przyrównanie, metoda i kompletność TEMPLATE | Że całe białko ma identyczny fałd i konformację |
| Czy sekwencja i przewidywana struktura są biologicznie spójne? | Parametry fizykochemiczne, profile, topologia TM, IDR, struktura drugorzędowa | Że przewidziana topologia lub funkcja jest potwierdzona eksperymentalnie |
| Jak wiarygodna jest predykcja 3D? | Raport modelowania, jakość lokalna, pokrycie i porównanie z TEMPLATE | Że model odtwarza dokładną strukturę TARGET |
| Czy geometria modelu jest prawidłowa? | Ramachandran, rotamery, clashes, QMEAN/ProSA/MolProbity | Że model ma poprawny fałd lub jest stabilny w komórce |
| Gdzie model może być błędny? | *Hot spots* z numeracją reszt, przyrównaniem i lokalnymi miarami jakości | Że każda elastyczna pętla lub niski score oznacza błąd |
| Czy refinement pomógł? | Te same miary **przed i po** oraz zmiany strukturalne i lokalne | Że nastąpił wzrost stabilności termodynamicznej |

**Zasada argumentacji naukowej:** każdą ważną konkluzję zapisuj według schematu **wynik liczbowy / obserwacja → interpretacja biologiczna → ograniczenia → wniosek**. Przykłady: „Model obejmuje tylko fragment sekwencji, więc jego dobra jakość nie dotyczy pozostałych reszt”; „poprawił się clashscore, ale pogorszyła lokalna jakość pętli — refinement ma wynik mieszany”.

### Jak odróżnić rodzaje dowodów?

- **Dane wejściowe:** rzeczywista sekwencja, adnotacje, współrzędne eksperymentalnego TEMPLATE i identyfikatory baz danych.
- **Obliczenia i predykcje:** pI, GRAVY, profile, regiony TM i IDR, struktura drugorzędowa, modele 3D.
- **Wskaźniki oceny:** GMQE, QMEANDisCo, C-score, pLDDT/PAE (zależnie od metody), RMSD, Ramachandran, clashes. Każdy opisuje **inny aspekt jakości**.
- **Wniosek biologiczny:** Twoja interpretacja zgodności tych źródeł danych, nie automatyczna odpowiedź programu.

W raporcie wyraźnie odróżniaj **wynik otrzymany z serwisu**, **własne porównanie/obliczenie** i **hipotezę lub interpretację**. Nie nazywaj przewidywań „pomiarami eksperymentalnymi”.

## 1. Organizacja pracy: cztery grupy

Różne metody rozwiązują struktury 3D inną drogą i proponują odmienne metryki jakości. Dlatego każdy student wykonuje **własny projekt badawczy**, a w części *Materials and Methods* i *Results* opisuje narzędzie/narzędzia przypisane swojej grupie. Nie powinno się mechanicznie kopiować interpretacji GMQE do I-TASSER ani pLDDT do SWISS-MODEL, ponieważ każde narzędzie lub ściezka analityczna ma inną specyfikę i inne parametry.

| Grupa | Główna metoda / serwis | Uwagi |
|---|---|---|
| **1** | [SWISS-MODEL](https://swissmodel.expasy.org/) | Modelowanie porównawcze na podstawie szablonu homologicznego |
| **2** | [I-TASSER](https://zhanggroup.org/I-TASSER/) | Threading, składanie fragmentów i udoskonalanie struktury; możliwe kolejki |
| **3** | [Phyre2](https://www.sbg.bio.ic.ac.uk/phyre2/html/page.cgi) | Rozpoznawanie fałdu i modelowanie z wykorzystaniem szablonów; dostępność należy sprawdzić przed zajęciami |
| **4** | [AlphaFold Server](https://alphafoldserver.com/) **+ SWISS-MODEL** | AlphaFold to predykcja AI, a nie klasyczne modelowanie homologiczne; obowiązkowy dodatkowy model SWISS-MODEL zapewnia realizację celu kursu |

**Instrukcje techniczne dla grup:** [SWISS-MODEL](./swiss-model-student-checklist.md) · [I-TASSER](./i-tasser-student-checklist.md) · [AlphaFold Server](./alphafold-server-student-checklist.md). To w nich znajdziesz dokładne operacje w poszczególnych serwisach.

**Uwaga organizacyjna:** przed przydziałem grup prowadzący sprawdza aktualne limity, warunki korzystania, rejestrację i dostępność serwisów. W razie niedostępności usługi dopuszczalny jest serwis zastępczy uzgodniony z prowadzącym. Obliczenia I-TASSER i niektórych innych serwisów należy uruchomić odpowiednio wcześniej. W trakcie oczekiwania można wykonywać etapy 3–5.

## 2. Wybór TARGET i TEMPLATE — warunek rozpoczęcia projektu

**Pytanie:** *Czy w ogóle istnieją podstawy, by przewidywać strukturę mojego białka z wykorzystaniem homologii?*

**Dlaczego to robisz?** Modelowanie porównawcze wykorzystuje zależność między podobieństwem ewolucyjnym sekwencji a podobieństwem struktur. Jeśli szablon obejmuje jedynie niewielką część TARGET, dobra zgodność tego fragmentu nie usprawiedliwia wnioskowania o całym białku. Ten etap ustala, **co może być modelowane wiarygodnie, a czego jeszcze nie wiemy**.

**Dowody w raporcie:** tabela A, przyrównanie sekwencji, identyfikatory PDB z numerami łańcuchów, opis brakujących fragmentów i krótkie uzasadnienie wyboru. Jeśli TARGET zawiera domenę TM, peptyd sygnałowy lub część nieuporządkowaną, oceń osobno pokrycie tych regionów.

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

**W raporcie odpowiedz jednym akapitem:** dlaczego wybrany TEMPLATE jest lepszy od drugiego kandydata? Czy poza liczbą `% identity` uwzględniłeś **pokrycie strukturalne, luki, rozdzielczość/jakość, biologiczną postać białka i ważne domeny**? Pamiętaj, że duża zgodność modelu z szablonem, na którym go zbudowano, nie jest niezależną walidacją modelu.

## 3. Teoretyczne właściwości fizykochemiczne TARGET i TEMPLATE

**Pytanie:** *Czy właściwości chemiczne TARGET są zgodne z jego znaną funkcją i czy różnice względem TEMPLATE można wytłumaczyć różnicami sekwencji?*

**Dlaczego to robisz?** Struktura i oddziaływania białka zależą od składu aminokwasowego, ładunku, hydrofobowości i organizacji domen. Takie parametry **opisują sekwencję**, ale same nie wyznaczają struktury 3D. W raporcie pokaż, które różnice są biologicznie sensowne; nie traktuj każdego odmiennego wyniku jako „błędu modelu”.

**Dowody w raporcie:** tabela B i interpretacja 3–5 najważniejszych właściwości (z jednostkami oraz warunkami obliczeń, jeśli dotyczy). Zaznacz, czy porównujesz białka pełnej długości, dojrzałe sekwencje po odcięciu peptydu sygnałowego, czy fragment modelowany — to **nie są te same obiekty**.

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

**Wzorzec opracowania tabel:** w prezentacji prowadzącego wskazano **tabele 1, 2 i 3** publikacji J. Sędzik, J. P. Jastrzębski, K. Ikenaka (2013), *Sequence motifs of myelin membrane proteins: Towards the molecular basis of diseases*, *Journal of Neuroscience Research* 91:479–493, [DOI: 10.1002/jnr.23177](https://doi.org/10.1002/jnr.23177), jako przykład porównawczego przedstawienia wyników. **Zapoznaj się z tymi tabelami** i wykorzystaj ich ideę organizacji danych, dostosowując własne zestawienie do TARGET i TEMPLATE. Nie kopiuj danych z publikacji. Niniejsza tabela B jest szablonem dydaktycznym, a nie reprodukcją tabel z artykułu.

**Pytanie:** które różnice mogą wynikać ze składu sekwencji, a które rzeczywiście mogą mieć znaczenie dla funkcji? `Instability index` nie jest eksperymentalnym pomiarem stabilności białka.

## 4. Profile właściwości wzdłuż sekwencji

**Pytanie:** *W których miejscach sekwencji występują właściwości mogące wpływać na lokalną strukturę i czy odpowiadające im regiony są podobne w TARGET i TEMPLATE?*

**Dlaczego to robisz?** Parametr średni, np. GRAVY, opisuje całe białko. Tymczasem białko może zawierać silnie hydrofobową helisę i jednocześnie duży hydrofilowy fragment. **Profil lokalizuje** takie regiony wzdłuż sekwencji. Nie jest jednak bezpośrednią mapą fałdu 3D.

**Dowody w raporcie:** czytelne, podpisane wykresy dla obu sekwencji, skala, okno, opis osi oraz zaznaczone regiony odpowiadające sobie w przyrównaniu. Wyjaśnij, co zmieniła wielkość okna i które lokalne różnice warto później sprawdzić w modelu 3D.

Wykonaj analizy **TARGET i TEMPLATE** przy tych samych skalach i parametrach. Podawaj numerację aminokwasów, wielkość okna przesuwnego i źródło skali. Porównuj homologiczne regiony na podstawie przyrównania sekwencji.

**Przykładowe narzędzia:** [ExPASy ProtScale](https://web.expasy.org/protscale/), notebook Python/Colab (Biopython, NumPy, Matplotlib). W historycznej prezentacji wskazano również narzędzia statystyk sekwencji EMBL-EBI.

**Profile obowiązkowe:**

1. **Hydrofobowość** według skali **Kyte’a–Doolittle’a**, dla co najmniej dwóch wielkości okna.
2. **Hydrofilowość** (z podaniem użytej skali).
3. **Dwa dodatkowe profile** wybrane spośród: polarności, elastyczności, dostępności powierzchniowej, tendencji do tworzenia helis/struktur β, ładunku lub **antygenowości** (wyłącznie jako predykcji zależnej od przyjętej skali, a nie dowodu immunogenności).

**Rezultat:** minimum cztery profile dla obu białek, z podpisami i omówieniem zgodności/rozbieżności; w razie potrzeby oznacz regiony TM i nieuporządkowane na wykresach.

## 5. Regiony transmembranowe, nieuporządkowanie i struktura drugorzędowa

**Pytanie:** *Czy przewidywana organizacja lokalna białka zgadza się z jego funkcją, lokalizacją i eksperymentalną strukturą TEMPLATE?*

**Dlaczego to robisz?** Są to trzy **różne** informacje: region transmembranowy sugeruje położenie w błonie; nieuporządkowanie wskazuje możliwość elastyczności lub braku jednej stabilnej struktury lokalnej; struktura drugorzędowa opisuje helisy, nici β i pozostałe fragmenty. Zestawienie tych przewidywań pozwala wykryć sprzeczności, które później trzeba wyjaśnić podczas analizy 3D.

**Dowody w raporcie:** tabela C, oznaczone zakresy reszt, wykresy i krótkie porównanie co najmniej dwóch algorytmów dla TM i struktury drugorzędowej. Pokaż także wynik **negatywny** (np. brak przewidywanych TM), jeśli jest biologicznie uzasadniony. Analizę wykonaj również dla TEMPLATE, a jego przewidywaną strukturę drugorzędową porównaj ze strukturą eksperymentalną.

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

**Pytanie:** *Jaki konkretnie model otrzymałem i jaką część badanego białka on przedstawia?*

**Dlaczego to robisz?** Plik współrzędnych nie jest sam w sobie odpowiedzią na pytanie o strukturę. Musisz wiedzieć, **jak powstał**, jaką sekwencję obejmuje, na jakich szablonach lub informacjach oparto predykcję oraz co program uznał za najbardziej prawdopodobny wynik. Opisz również, czy serwis przewidział tylko monomer, domenę, czy właściwą biologicznie jednostkę.

**Dowody w raporcie:** reprezentatywna ilustracja modelu, zakres modelowanych reszt i brakujących pozycji, narzędzie i parametry, informacja o szablonach oraz tabelka z wynikami oceny dostarczonej przez serwis. Przy kilku modelach uzasadnij wybór modelu głównego, nie ograniczając się do stwierdzenia „to pierwszy na liście”.

Uruchom narzędzie przypisane grupie (sekcja 1), używając zatwierdzonej sekwencji TARGET. Jeśli narzędzie pozwala wybrać szablon, udokumentuj, czy wykorzystano zatwierdzony TEMPLATE, czy inny. Jeśli model obejmuje tylko część sekwencji, wyraźnie określ jej zakres.

**Zapisz:** nazwę narzędzia, datę, identyfikator zadania, parametry, użyte szablony, pokrycie, ocenę jakości raportowaną przez serwis oraz plik PDB/mmCIF. Zachowaj **model pierwotny** (`target_initial.pdb` lub `.cif`).

**Ważne:** I-TASSER i Phyre2 nie są identyczne metodologicznie z SWISS-MODEL; AlphaFold Server nie jest metodą modelowania homologicznego. W raporcie opisz rzeczywistą metodę, a nie nazywaj wszystkich czterech serwisów „homology modeling”.

## 7. Wizualizacja i porównanie strukturalne

**Pytanie:** *Czy model TARGET ma architekturę zgodną z eksperymentalnym TEMPLATE? Gdzie występują różnice i czy są one biologicznie uzasadnione?*

**Dlaczego to robisz?** Tabela z oceną globalną może ukrywać duże lokalne różnice. Superpozycja wskazuje, **które regiony** zachowały podobny fałd, a które uległy przemieszczeniu. Wartość RMSD zależy od tego, **co dokładnie porównujesz** — uwzględnij to w interpretacji.

**Dowody w raporcie:** ryciny w jednakowej orientacji, superpozycja z podpisanymi różnicami, RMSD z liczbą dopasowanych reszt, opcjonalnie TM-score, a także wskazanie fragmentów niewłączonych do porównania.

**Narzędzia:** **RasMol / RasWin / RasTop** (prosta, historyczna wizualizacja; sprawdź zgodność z systemem) lub **[PyMOL](https://pymol.org/)** (zalecany do porównań i ilustracji). Można pomocniczo użyć przeglądarki Mol*.

Przygotuj ilustracje TEMPLATE i modelu TARGET w tych samych orientacjach. Pokaż minimum **trzy cechy strukturalne**, np. helisy/struktury β, regiony hydrofobowe, miejsca wiązania ligandów, regiony TM, miejsca o niskiej jakości modelu.

Wykonaj superpozycję **TARGET–TEMPLATE** z wykorzystaniem dopasowanych reszt. Podaj:

- RMSD, rodzaj porównywanych atomów (np. Cα), **liczbę dopasowanych reszt** i sposób superpozycji;
- opcjonalnie TM-score / [TM-align](https://zhanggroup.org/TM-align/);
- fragmenty o największych różnicach oraz ich możliwe przyczyny.

**Uwaga:** niski RMSD dla bardzo krótkiego dopasowanego fragmentu nie dowodzi podobieństwa całych struktur.

## 8. Walidacja i identyfikacja „hot spots”

**Pytanie:** *Czy model jest dobrze zbudowany pod względem geometrycznym i czy mamy powody ufać jego lokalnemu i globalnemu ułożeniu?*

**Dlaczego to robisz?** Program może uzyskać strukturę o pozornie poprawnym wyglądzie, lecz z niekorzystnymi kątami wiązań, konfliktami sterycznymi lub regionami o niskiej pewności. **Weryfikacja musi obejmować różne, możliwie niezależne aspekty jakości.** Sprawdzenie tego samego wyniku QMEAN w dwóch ekranach nie daje dwóch niezależnych dowodów.

**Dowody w raporcie:** miary właściwe dla wykorzystanego serwisu, **wykres Ramachandrana**, raport clashes/rotamerów, wynik QMEAN lub ProSA (jeśli dostępny), profile lokalnej jakości i tabela regionów problematycznych. Wnioski formułuj oddzielnie dla obszarów dobrze i słabo popartych danymi.

Oceń model pierwotny przy użyciu **co najmniej dwóch niezależnych narzędzi oceny** i wyników dostarczonych przez serwis modelujący.

**Proponowane narzędzia:**

- [SWISS-MODEL Structure Assessment / QMEAN](https://swissmodel.expasy.org/assess) — ocena globalna i lokalna, QMEAN/QMEANDisCo zależnie od usługi;
- [ProSA-web](https://prosa.services.came.sbg.ac.at/prosa.php) — ocena statystyczna i profil energii;
- [MolProbity](https://molprobity.biochem.duke.edu/) — geometria, Ramachandran, konflikty steryczne;
- [PDBsum](https://www.ebi.ac.uk/thornton-srv/databases/pdbsum/) — organizacja strukturalna, diagramy i dodatkowe raporty **jeśli serwis jest dostępny**;
- [ModFOLD](https://www.reading.ac.uk/bioinf/ModFOLD/) — dodatkowa ocena jakości, jeśli usługa działa;
- [RCSB PDB: Structure Validation and Quality](https://www.rcsb.org/docs/additional-resources/structure-validation-and-quality) — interpretacja danych walidacyjnych struktur eksperymentalnych.

Zbadaj **Ramachandran outliers, clashes, jakość lokalną i globalną** oraz, gdy są dostępne, specyficzne wskaźniki modelowania (np. GMQE/QMEANDisCo, C-score, pLDDT/PAE). Nie porównuj tych miar tak, jakby były identyczne. **GMQE** ocenia oczekiwaną jakość modelowania w SWISS-MODEL, **QMEANDisCo** ocenia jakość modelu, **C-score** szacuje wiarygodność predykcji I-TASSER, **pLDDT** opisuje lokalną pewność w AlphaFold, a **PAE** — przewidywaną niepewność położenia jednej części względem drugiej. **RMSD** jest miarą podobieństwa dwóch porównywanych struktur. Żadna z tych wielkości nie jest uniwersalnym „procentem poprawności”.

**Hot spots:** wskaż do trzech (lub więcej, jeśli uzasadnione) problematycznych regionów i opisz ich pozycje, rodzaj problemu, dowód z analizy oraz możliwą przyczynę. **Nie wymyślaj problemów:** jeśli model nie wykazuje trzech wyraźnych hot spots, uzasadnij mniejszą liczbę.

**Proponowana tabela uzupełniająca — lista *hot spots*:**

| Region (reszty TARGET) | Obserwowany problem | Dowód (narzędzie/metryka) | Możliwa przyczyna | Czy refinement może pomóc? |
|---|---|---|---|---|
| | | | | |

Dla co najmniej jednego problematycznego fragmentu przygotuj powiększenie ryciny z zaznaczonymi resztami. **Nie utożsamiaj nieuporządkowania biologicznego z błędem modelowania**: region elastyczny może naturalnie mieć niejednoznaczną strukturę.

## 9. Refinement / minimalizacja i ponowna walidacja

**Pytanie:** *Czy konkretna operacja poprawiła model w miejscach, które wcześniej uznałem za problematyczne, i czy nie pogorszyła innych jego części?*

**Dlaczego to robisz?** Refinement nie jest magicznym „naprawieniem” białka. Może poprawić kontakty atomowe, rotamery i lokalną geometrię, ale też zmienić położenie pętli lub zniszczyć korzystne oddziaływania. Dlatego konieczne jest porównanie **tego samego zestawu metryk** przed i po, a nie samo pokazanie nowego pliku PDB.

**Dowody w raporcie:** tabela D, parametry zastosowanego protokołu, model pierwotny i refined, nałożenie obu struktur, zmiany *hot spots* oraz jasno sformułowana ocena: *poprawił się / poprawił częściowo / nie poprawił się*. Jeśli masz wynik energii, podaj jednostkę, pole siłowe i warunki obliczeń — energii z dwóch różnych protokołów nie porównuj bezpośrednio.

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

## 10. Jak napisać raport końcowy?

**Raport nie ma być dziennikiem wykonanych kliknięć.** Ma przypominać krótką publikację naukową: jasno określać problem, przedstawiać dane, a następnie bronić wniosków na podstawie wyników. Osobne checklisty opisują **jak wykonać analizy**; tutaj opisujemy **jak nadać wynikom sens i pokazać ich znaczenie**.

### Zalecana struktura raportu

**1. Title, Abstract i podstawowe informacje o białku**  
Podaj nazwę TARGET, organizm, numer akcesyjny, główną metodę modelowania i główny wynik projektu. W krótkim streszczeniu (ok. 150–250 słów) napisz: jaki problem rozwiązujesz, co zrobiłeś, które wyniki najmocniej przemawiają za wiarygodnością modelu i co pozostaje niepewne. **Streszczenie napisz na końcu.**

**2. Introduction — wprowadzenie i pytanie badawcze**  
Wyjaśnij funkcję biologiczną białka, jego lokalizację, znaczenie poznania struktury i powód wyboru TARGET. Napisz, dlaczego odpowiednia eksperymentalna struktura TARGET nie jest dostępna oraz dlaczego modelowanie jest uzasadnione. Sformułuj cel i 2–3 pytania badawcze, np.:

- Czy sekwencja ma odpowiednio dobry szablon, aby odtworzyć rdzeń strukturalny?
- Czy model jest zgodny z przewidywaną topologią, strukturą drugorzędową i eksperymentalną strukturą homologu?
- Które fragmenty pozostają niepewne i czy ich jakość można poprawić?

**3. Materials and Methods — materiały i metody**  
Opisz **tylko to, co rzeczywiście zrobiłeś**, ale wystarczająco dokładnie, aby inna osoba mogła powtórzyć pracę. Uwzględnij źródła FASTA i PDB, akcesje, łańcuchy, BLAST i kryteria wyboru TEMPLATE, wszystkie użyte serwisy i wersje (jeżeli dostępne), skalę i okno profili, parametry modelowania, programy oceny, protokół refinementu i sposób superpozycji. Informacje techniczne przechowuj też w `methods-and-parameters.md`. **Nie wpisuj metody, której nie wykonywałeś.**

**4. Results — wyniki**  
Przedstaw wyniki w porządku logicznym, z zachowaniem tabel A–D i rycin:

- **4.1. TARGET i TEMPLATE:** tabela A, przyrównanie sekwencji i uzasadnienie wyboru szablonu.
- **4.2. Właściwości fizykochemiczne:** tabela B z interpretowalnymi różnicami.
- **4.3. Profile i predykcje lokalne:** wykresy hydrofobowości, hydrofilowości i innych właściwości, regiony TM, IDR, przewidywana struktura drugorzędowa oraz tabela C.
- **4.4. Modelowanie 3D:** ilustracja pierwotnego modelu, jego pokrycie, identyfikatory zadań i metryki odpowiednie dla użytego narzędzia.
- **4.5. Porównanie strukturalne:** superpozycja TARGET–TEMPLATE, RMSD (z liczbą dopasowanych reszt), opcjonalnie TM-score, opis różnic lokalnych.
- **4.6. Ocena jakości:** Ramachandran, konflikty steryczne, lokalne i globalne wskaźniki jakości, wykaz *hot spots*.
- **4.7. Refinement:** tabela D, nałożenie modeli initial/refined, wykres lub ryciny zmian w regionach problematycznych.

W części *Results* podawaj przede wszystkim **wyniki i zwięzłe obserwacje**, a dłuższe wyjaśnienia przenieś do *Discussion*. Każda tabela i rycina powinna mieć numer, tytuł/podpis, jednostki, rozwinięcie skrótów oraz odwołanie w tekście. Zrzuty ekranu mogą stanowić dokumentację, ale nie zastępują opisanych liczb i wykresów.

**5. Discussion — dyskusja i ocena wiarygodności**  
To **najważniejsza część raportu**. Nie powtarzaj tabel; połącz wyniki w logiczną argumentację. Omów, co potwierdzają dane z kilku metod, co jest sprzeczne, co wynika z ograniczeń szablonu i gdzie pozostaje niepewność. Oddziel:

- **jakość sekwencyjnego uzasadnienia** (identity, coverage, kompletność TEMPLATE),
- **zgodność biologiczną** (lokalizacja, topologia, profile, struktura drugorzędowa),
- **pewność predykcji** (miary właściwe dla narzędzia),
- **poprawność stereochemiczną** (Ramachandran, clashes itd.),
- **podobieństwo strukturalne** do TEMPLATE (RMSD/TM-score z opisem zakresu),
- **efekt refinementu** i ryzyko wprowadzenia nowych problemów,
- **granice wnioskowania o stabilności**.

**6. Conclusions — wnioski**  
Sformułuj 4–6 krótkich, precyzyjnych wniosków powiązanych z wynikami. Odpowiedz wprost: **którym regionom modelu można ufać najbardziej, którym najmniej, czy refinement był korzystny i czy model nadaje się do dalszych analiz oraz do jakich**. Jeżeli jakieś pytanie pozostaje nierozstrzygnięte, nazwij je i podaj powód.

**7. References i Supplementary materials**  
Zacytuj bazy danych, narzędzia oraz publikacje opisujące metody i biologiczny kontekst analizowanego białka. Dołącz wyniki i pliki pomocnicze pozwalające odtworzyć analizy.

### Obowiązkowe pytania, na które raport ma odpowiedzieć

1. **Czy model jest wiarygodny?** Jakie **konkretne dane liczbowe, porównania i wyniki niezależnych metod** wspierają tę ocenę, a które ją ograniczają?
2. **Czy geometria modelu jest fizycznie realistyczna?** Jakie nieprawidłowości wykryto i w których resztach?
3. **Co można, a czego nie można powiedzieć o stabilności?** Jakie dalsze analizy lub doświadczenia byłyby konieczne do jej oceny?
4. **Gdzie znajdują się hot spots?** Jak wiążą się z przyrównaniem sekwencji, profilami i lokalną oceną jakości?
5. **Czy refinement pomógł?** Przedstaw porównanie tych samych metryk i struktur przed optymalizacją i po niej.
6. **Jakie co najmniej dwie dodatkowe analizy** proponujesz i dlaczego? Co miałyby zweryfikować?
7. **Jakie zastosowanie jest uzasadnione?** Czy model można wykorzystać np. do analizy konserwowanych domen, czy raczej nie należy interpretować niepewnej pętli jako miejsca wiązania?

### Jak pisać interpretację: przykład

**Słabo:** „Model jest dobry, bo program podał wysoki wynik i nie było błędów”.

**Lepiej:** „Szablon obejmuje [X]% sekwencji TARGET przy [Y]% identity, dlatego modelowanie rdzenia [zakres reszt] ma dobre podstawy sekwencyjne. Niezależna walidacja wykazała [wynik Ramachandran/clashscore], natomiast w regionie [zakres] lokalna jakość była obniżona. Region ten zawiera [np. insercję lub przewidywaną pętlę], zatem należy zachować ostrożność przy interpretacji jego ułożenia. Refinement [poprawił / nie poprawił] [konkretną metrykę] bez [lub z] istotnymi zmianami pozostałych fragmentów. **Można zatem uznać model za [wiarygodny w określonym zakresie / częściowo wiarygodny], ale nie potwierdza to jego stabilności termodynamicznej.**”

W nawiasach kwadratowych wpisz **rzeczywiste** dane uzyskane dla swojego białka; nie kopiuj przykładowego akapitu bez ich sprawdzenia.

### Minimalny pakiet plików

```text
student_project/
├── target.fasta
├── template.fasta
├── template.pdb                 # lub .cif
├── target_initial.pdb           # lub .cif
├── target_refined.pdb           # lub .cif
├── tables/                      # tabele A–D i hot spots
├── profiles/                    # wykresy z opisami skal i parametrów
├── validation/                  # raporty walidacji przed/po
├── figures/                     # wizualizacje i superpozycje
├── methods-and-parameters.md    # wersje, daty, ustawienia, identyfikatory zadań
└── final-report.pdf
```

Wyniki mogą pochodzić z serwisów internetowych lub notebooków Python/Colab. **Zachowaj pliki pierwotne**; nie nadpisuj modelu initial modelem refined. Jeśli program nie dostarcza danej metryki lub analiza się nie powiodła, zaznacz to jawnie zamiast wpisywać fikcyjną wartość.

### Zasady zaliczenia

**Ocena:** poprawność metod (30%), interpretacja biologiczna (25%), porównanie TARGET–TEMPLATE (20%), walidacja i krytyczna ocena refinementu (15%), dokumentacja i odtwarzalność (10%). **Nie jest wymagane uzyskanie „idealnego” modelu ani udowodnienie jego termodynamicznej stabilności.**

**Najwyżej oceniona zostanie praca, w której student potrafi uczciwie wskazać zarówno mocne, jak i słabe strony modelu, a każdy ważny wniosek jest poparty identyfikowalnym wynikiem lub porównaniem.**

## 11. Kontrola jakości własnego raportu przed oddaniem

Zanim oddasz dokument, sprawdź:

- [ ] Czy czytelnik dowie się, **jakie białko badam i dlaczego** jego struktura jest istotna?
- [ ] Czy jasne jest, **skąd pochodzą TARGET i TEMPLATE**, co obejmuje przyrównanie i które fragmenty nie mają odpowiednika w szablonie?
- [ ] Czy wyniki fizykochemiczne i profile zostały **zinterpretowane**, a nie tylko wklejone?
- [ ] Czy każda istotna predykcja ma podaną **metodę, parametry i ograniczenia**?
- [ ] Czy z raportu wynika, **jak powstał model 3D**, ile reszt obejmuje i jakie oceny przedstawił serwis?
- [ ] Czy RMSD jest opisane wraz z **zakresem i liczbą dopasowanych reszt**, a podobieństwa z TEMPLATE nie przedstawiam jako niezależnego dowodu poprawności?
- [ ] Czy wykonałem **niezależną walidację** i pokazałem konkretne problemy geometrii oraz jakości lokalnej?
- [ ] Czy *hot spots* mają **numerację reszt i uzasadnienie**, a nie tylko czerwone oznaczenia na ilustracji?
- [ ] Czy wyniki **przed i po refinement** są policzone porównywalnymi metodami?
- [ ] Czy jasno oddzielam **geometrię, pewność predykcji, podobieństwo strukturalne i stabilność termodynamiczną**?
- [ ] Czy dyskusja przytacza również wyniki **niezgodne z hipotezą lub słabsze**?
- [ ] Czy podaję **dwie sensowne dalsze analizy** i wyjaśniam, jakie pytanie pomogą rozstrzygnąć?
- [ ] Czy wszystkie ryciny, tabele, pliki wynikowe i cytowania są kompletne, podpisane oraz możliwe do odnalezienia?

**Ostateczne zadanie:** nie przekonuj czytelnika, że komputer „wyprodukował prawidłową strukturę”. Pokaż, **na jakiej podstawie i w jakich granicach naukowo uzasadnione jest korzystanie z Twojego modelu**.
