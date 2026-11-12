# Projekt indywidualny: modelowanie białka w AlphaFold Server

**Kurs:** Podstawy Modelowania Molekularnego  
**Grupa:** AlphaFold Server (AlphaFold 3)  
**Temat:** Od sekwencji aminokwasowej do zweryfikowanego modelu 3D po refinement  
**Forma:** praca indywidualna; wyniki TARGET porównywane z TEMPLATE

> **Cel:** opracować model struktury 3D wybranego białka, określić pewność predykcji i jakość geometrii, wskazać regiony problematyczne, wykonać refinement i sprawdzić, czy model uległ poprawie. **Minimalizacja energii nie jest dowodem stabilności termodynamicznej białka.**

## Przed rozpoczęciem

Musisz mieć zatwierdzone:

- **TARGET:** pełna sekwencja białka (preferowana długość 150–400 aa), identyfikator UniProt/NCBI, plik `target.fasta`.
- **TEMPLATE:** homolog o **eksperymentalnie** wyznaczonej strukturze PDB, wybrany na podstawie BLASTp (preferowana identyczność 60–95%, query coverage ≥85%, najlepiej ≥90%); zapisz PDB ID, identyfikator łańcucha i metodę eksperymentalną.
- **Przyrównanie sekwencji TARGET–TEMPLATE:** identity, query coverage, zakres przyrównania, luki, E-value.
- Pliki `template.fasta` (pełna sekwencja homologu odpowiadającego wybranemu łańcuchowi) oraz `template.cif` lub `template.pdb`.

**Uwaga metodyczna:** AlphaFold Server nie pozwala wskazać własnego, konkretnego pliku TEMPLATE tak jak SWISS-MODEL. Można kontrolować wykorzystanie szablonów PDB ogólnie (włączyć, wyłączyć lub ustawić datę graniczną). Wybrany przez Ciebie TEMPLATE służy do **porównania**. AlphaFold 3 nie jest klasycznym modelowaniem homologicznym. Jeżeli wymagane jest również modelowanie ściśle oparte na wybranym szablonie, należy wykonać dodatkowy model w SWISS-MODEL.

Wszystkie analizy sekwencyjne wykonuj **osobno dla TARGET i TEMPLATE**, z tymi samymi ustawieniami. Nie myl pełnej sekwencji białka z fragmentem reszt widocznych w strukturze eksperymentalnej.

---

## 1. Podstawowe właściwości fizykochemiczne

**Gdzie:** [ExPASy ProtParam](https://web.expasy.org/protparam/); opcjonalnie [Compute pI/Mw](https://web.expasy.org/compute_pi/).

- [ ] Otwórz ProtParam, wklej sekwencję TARGET (lub identyfikator obsługiwany przez serwis) i uruchom obliczenia.
- [ ] Powtórz analizę dla TEMPLATE.
- [ ] Zapisz: długość, masę cząsteczkową, teoretyczne pI, skład aminokwasowy, liczbę reszt naładowanych dodatnio/ujemnie, współczynnik ekstynkcji, instability index, aliphatic index i GRAVY.
- [ ] Zestaw wyniki w **Tabeli 1: TARGET vs TEMPLATE** i wyjaśnij najważniejsze różnice.

**Wzór organizacji tabel porównawczych:** tabele 1–3 publikacji [DOI: 10.1002/jnr.23177](https://doi.org/10.1002/jnr.23177) wskazanej przez prowadzącego. Nie należy kopiować tabel bezpośrednio; opracuj własne zestawienie dla badanych sekwencji.

**Oddaj:** `table_01_physicochemical.csv` lub tabelę w raporcie + 3–5 zdań interpretacji.

## 2. Profile właściwości wzdłuż sekwencji

**Gdzie:** [ExPASy ProtScale](https://web.expasy.org/protscale/); opcjonalnie notebook Python/Colab.

- [ ] Wklej sekwencję TARGET do ProtScale.
- [ ] Oblicz profil **hydrofobowości** według **Kyte’a–Doolittle’a** (zapisz długość okna; np. 9 i 19 aa).
- [ ] Oblicz profil **hydrofilowości** (np. skala Hopp–Woods, jeśli dostępna w interfejsie).
- [ ] Oblicz co najmniej **jeden profil tendencji strukturalnej**, np. skłonność do tworzenia helisy α lub nici β (skale typu Chou–Fasman). Jest to profil skali aminokwasowej, **nie** pełna predykcja struktury drugorzędowej.
- [ ] Powtórz wszystkie profile dla TEMPLATE, stosując identyczne skale i parametry.
- [ ] Zaznacz istotne maksima/minima i porównaj pozycje z uwzględnieniem przyrównania sekwencji.

**Oddaj:** co najmniej **3 rodzaje profili** dla obu sekwencji (hydrofobowość, hydrofilowość, tendencja strukturalna), opis skal, wielkości okien i interpretację.

## 3. Regiony transmembranowe i topologia

**Gdzie:** [DeepTMHMM](https://dtu.biolib.com/DeepTMHMM), [TOPCONS](https://topcons.cbr.su.se/); dodatkowo [TMHMM](https://services.healthtech.dtu.dk/services/TMHMM-2.0/) lub SOSUI, jeśli dostępne.

- [ ] Uruchom predykcję TARGET w **dwóch dostępnych narzędziach** (preferowane DeepTMHMM + TOPCONS).
- [ ] Powtórz dla TEMPLATE.
- [ ] Zapisz liczbę i pozycje przewidywanych odcinków transmembranowych, typ białka oraz orientację, jeżeli podana.
- [ ] Porównaj te regiony z profilem hydrofobowości i adnotacjami UniProt.
- [ ] Jeśli białko jest rozpuszczalne i narzędzia nie przewidują helis TM, **wynik negatywny jest prawidłowym wynikiem analizy**.

**Oddaj:** **Tabela 2: topologia TARGET vs TEMPLATE** oraz schemat lub ilustracja predykcji.

## 4. Regiony nieuporządkowane

**Gdzie:** [IUPred3](https://iupred3.elte.hu/) (opcjonalnie ANCHOR2).

- [ ] Wprowadź sekwencję TARGET i wybierz predykcję **long disorder**.
- [ ] Powtórz dla TEMPLATE.
- [ ] Zaznacz fragmenty o podwyższonym prawdopodobieństwie nieuporządkowania i podaj ich pozycje.
- [ ] Zachowaj wykresy; później porównasz te regiony z wartościami pLDDT modelu AlphaFold.

**Oddaj:** 2 wykresy + krótką interpretację. **Niski pLDDT nie jest automatycznie dowodem nieuporządkowania.**

## 5. Predykcja struktury drugorzędowej

**Gdzie:** [JPred](https://www.compbio.dundee.ac.uk/jpred4/), a do porównania druga dostępna metoda, np. GOR IV, SOPMA lub PSIPRED.

- [ ] Wykonaj predykcję dla TARGET metodą JPred i **jedną drugą metodą**.
- [ ] Powtórz dla TEMPLATE (co najmniej metodą JPred).
- [ ] Wyznacz udział helis α, nici β i pozostałych stanów strukturalnych; wskaż rozbieżności między metodami.
- [ ] Dla TEMPLATE sprawdź przypisanie struktury drugorzędowej z jego **eksperymentalnej struktury** (np. adnotacje PDB lub DSSP) i porównaj je z predykcją.

**Oddaj:** **Tabela 3: struktura drugorzędowa**, wykres/mapa pozycjonująca elementy w sekwencji i komentarz do rozbieżności.

## 6. Dodatkowa charakterystyka biologiczna (obowiązkowy krótki opis)

**Gdzie:** [UniProt](https://www.uniprot.org/), [RCSB PDB](https://www.rcsb.org/).

- [ ] Sprawdź, czy białko zawiera domeny funkcjonalne, motywy, miejsca aktywne, kofaktory lub wiązania disiarczkowe (jeśli są opisane).
- [ ] Zapisz, czy biologicznie funkcjonuje jako monomer czy część kompleksu.
- [ ] Zwróć uwagę, czy brak partnerów, jonów, ligandów lub modyfikacji może ograniczać interpretację modelu pojedynczego łańcucha.

**Oddaj:** krótki opis (5–8 zdań) i źródła adnotacji.

## 7. Predykcja struktury 3D — AlphaFold Server

**Gdzie:** [AlphaFold Server](https://alphafoldserver.com/) (bezpłatny do dozwolonego użytku niekomercyjnego; wymagane konto Google).

1. Otwórz AlphaFold Server i zaloguj się.
2. Rozpocznij nowe zadanie i wybierz encję typu **Protein**.
3. Wklej **samą sekwencję TARGET** (standardowe jednoliterowe kody aminokwasów; bez nieokreślonych reszt). Ustaw **jedną kopię białka**, chyba że prowadzący zatwierdzi inny układ.
4. Nie dodawaj DNA, RNA, ligandów, jonów ani PTM bez biologicznego uzasadnienia i uzgodnienia z prowadzącym.
5. Sprawdź ustawienie korzystania z szablonów PDB (**Use templates**); zapisz, czy użyto ustawienia domyślnego, wyłączono szablony czy zastosowano datę graniczną. Nie jest to miejsce do ręcznego przesłania wybranego TEMPLATE.
6. Kliknij **Continue and preview job**, nadaj nazwę np. `TARGET_accession_AF3`, sprawdź dane i wybierz **Confirm and submit job**.
7. Po zakończeniu otwórz wynik w **History** i obejrzyj model 3D, kolory pLDDT, wykres PAE i pTM.
8. Kliknij **Download** i zachowaj całe archiwum ZIP. Serwer zwykle udostępnia **pięć uszeregowanych predykcji** w formacie **mmCIF** wraz z plikami **JSON** zawierającymi miary pewności.
9. Do głównej analizy wybierz model o najwyższym rankingu (`model_0`); porównaj go pobieżnie z co najmniej jednym innym modelem z zestawu.

**Oddaj:** archiwum ZIP, plik mmCIF modelu 0, pliki JSON, zrzut ekranu z wynikami oraz opis parametrów zadania.

**Dokumentacja:** [oficjalny przewodnik EMBL-EBI krok po kroku](https://www.ebi.ac.uk/training/online/courses/alphafold/alphafold-3-and-alphafold-server/alphafold-server-your-gateway-to-alphafold-3/a-step-by-step-guide-to-generating-predictions-with-alphafold-server/).

## 8. Ocena pewności predykcji — raport AlphaFold

**Gdzie:** ekran wyników AlphaFold Server i pliki JSON z pobranego archiwum.

- [ ] Zapisz **pTM** (0–1): oszacowanie pewności globalnego ułożenia struktury.
- [ ] Oceń **pLDDT** (0–100): lokalną pewność predykcji; w AlphaFold 3 może być raportowana dla poszczególnych atomów. Przy podsumowaniu regionów używaj spójnej definicji (np. atomów Cα).
- [ ] Zapisz odsetek reszt w przedziałach pLDDT: **>90**, **70–90**, **50–70**, **<50**.
- [ ] Obejrzyj **PAE (Predicted Aligned Error)**: wskaż, które fragmenty lub domeny mają niepewne wzajemne ułożenie.
- [ ] Zapisz **ranking_score**, ale używaj go wyłącznie do porównania i uszeregowania pięciu predykcji; **nie** traktuj go jak procentu jakości.
- [ ] Wskaż regiony o niskiej pewności i porównaj je z predykcją nieuporządkowania oraz strukturą drugorzędową.

**Uwaga:** AlphaFold Server **nie podaje GMQE** (to metryka znana z SWISS-MODEL). Dla pojedynczego łańcucha podstawowe są pLDDT, PAE i pTM; **ipTM** służy do oceny interfejsów w kompleksach, więc nie jest tu obowiązkowe.

**Oddaj:** **Tabela 4: pewność predykcji AlphaFold**, wykres/obraz PAE i lista regionów o obniżonej pewności.

## 9. Wizualizacja i porównanie modelu z TEMPLATE

**Gdzie:** [PyMOL](https://pymol.org/); ewentualnie RasMol/RasTop do podstaw wizualizacji; [TM-align](https://zhanggroup.org/TM-align/) do porównania struktur.

- [ ] Otwórz strukturę eksperymentalną TEMPLATE i model `model_0.cif` w PyMOL.
- [ ] Wyświetl oba białka jako **cartoon**; pokoloruj je odmiennie i wykonaj superpozycję odpowiadających sobie łańcuchów.
- [ ] Zapisz **RMSD**, liczbę dopasowanych atomów/reszt i zakres sekwencji uwzględniony w porównaniu.
- [ ] Jeśli używasz TM-align, zapisz także **TM-score** oraz pokrycie dopasowania.
- [ ] Zaznacz co najmniej dwa regiony, które najbardziej różnią się między TARGET a TEMPLATE (o ile takie istnieją).
- [ ] Wykonaj ilustrację superpozycji i osobną ilustrację modelu pokolorowanego według pLDDT.

**Oddaj:** ilustracje 3D i **Tabela 5: podobieństwo strukturalne TARGET vs TEMPLATE**.

## 10. Niezależna walidacja jakości struktury

**Gdzie:** [MolProbity](https://molprobity.biochem.duke.edu/), [SWISS-MODEL Structure Assessment / QMEAN](https://swissmodel.expasy.org/assess), [ProSA-web](https://prosa.services.came.sbg.ac.at/prosa.php). **PDBsum** jest materiałem uzupełniającym, ponieważ jego serwis internetowy może być niedostępny.

- [ ] Wgraj model pierwotny do **MolProbity** i przeprowadź walidację geometrii.
- [ ] Zapisz: odsetek reszt w regionach dozwolonych i niedozwolonych wykresu **Ramachandrana**, **clashscore** oraz nieprawidłowe rotamery, jeśli raportowane.
- [ ] Przeprowadź ocenę w **QMEAN / QMEANDisCo**; zapisz miary globalne i profile lokalne, jeżeli są dostępne.
- [ ] Przeanalizuj model w **ProSA-web** i zapisz Z-score oraz wskazane regiony problematyczne.
- [ ] Wskaż **hot spots**: reszty lub odcinki o problematycznej geometrii, niskiej pewności lub niekorzystnych ocenach lokalnych. Podaj numery reszt, źródło oceny i możliwe przyczyny.
- [ ] Nie wymyślaj problemów: jeśli nie ma trzech ewidentnych regionów problematycznych, opisz rzeczywiście wykryte i uzasadnij ich liczbę.

**Oddaj:** **Tabela 6: walidacja modelu pierwotnego**, wykres Ramachandrana i tabela hot spots.

## 11. Refinement / minimalizacja energii

**Gdzie:** [GalaxyRefine](https://galaxy.seoklab.org/cgi-bin/submit.cgi?type=REFINE) **lub** [YASARA Energy Minimization Server](https://www.yasara.org/minimizationserver.htm).

- [ ] Zachowaj oryginalny model AlphaFold **bez zmian**.
- [ ] Jeśli serwis refinementu wymaga **PDB**, otwórz model mmCIF w PyMOL i wyeksportuj odpowiedni łańcuch do `target_initial.pdb`; sprawdź zgodność liczby i numeracji reszt.
- [ ] Wybierz **jedną** metodę refinementu: GalaxyRefine (udoskonalanie struktury) albo YASARA (minimalizacja energii). Zapisz nazwę, ustawienia i datę wykonania.
- [ ] Prześlij `target_initial.pdb` do wybranego serwisu; zapisz identyfikator zadania i pobierz wynik jako `target_refined.pdb`.
- [ ] Jeśli użyto YASARA, pamiętaj, że wyniki trafiają do publicznego obszaru pobierania — nie przesyłaj danych poufnych.
- [ ] Jeśli planujesz GalaxyRefine2, sprawdź jego ograniczenia wejścia: opisana metoda jest przeznaczona dla monomerów do 300 aa, dlatego **nie jest domyślnym wyborem** dla całego zakresu 150–400 aa.

**Oddaj:** pliki modelu pierwotnego i po refinement, raport/identyfikator zadania oraz opis metody.

## 12. Ponowna walidacja: czy refinement poprawił model?

- [ ] Ponownie wykonaj **MolProbity**, **QMEAN** i (jeżeli użyto wcześniej) **ProSA** dla modelu po refinement.
- [ ] Wykonaj superpozycję `target_initial.pdb` i `target_refined.pdb`; zapisz RMSD dla porównywalnego zbioru atomów.
- [ ] Sprawdź, czy poprawiły się geometria, clashscore, rotamery oraz oceny lokalne; sprawdź także, czy nie powstały nowe problemy.
- [ ] Oceń, czy model po refinement zachował sensowną architekturę i zgodność z TEMPLATE.
- [ ] Jeżeli serwis podaje energię potencjalną, porównuj ją **wyłącznie** w ramach tego samego protokołu, pola siłowego, przygotowania układu i zestawu atomów. Nie porównuj bezpośrednio energii pochodzących z różnych programów.
- [ ] **Nie przenoś pLDDT modelu AlphaFold na nowy model jako nowo obliczonej miary pewności**: po refinement jest to nadal ocena pierwotnej predykcji, chyba że wykonano nową, właściwą analizę.

**Tabela 7 — obowiązkowe porównanie:**

| Parametr | Przed refinement | Po refinement |
|---|---|---|
| Ramachandran outliers (%) | | |
| Clashscore | | |
| Rotamer outliers (%) | | |
| QMEAN / QMEANDisCo (ta sama metryka) | | |
| ProSA Z-score (jeżeli wykonano) | | |
| RMSD względem TEMPLATE (ten sam zakres) | | |
| RMSD względem modelu pierwotnego | — | |
| Liczba problematycznych regionów | | |
| Energia w tym samym protokole (jeśli dostępna) | | |

**Wniosek:** czy refinement poprawił **jakość geometryczną** modelu? Czy dostępne dane wystarczają do oceny **stabilności termodynamicznej**? (Zazwyczaj nie — wymaga to dodatkowych, odpowiednio zaprojektowanych analiz.)

---

## Co należy oddać?

- [ ] **Raport PDF**: wstęp, metody, wyniki, dyskusja, wnioski i źródła.
- [ ] **Tabele 1–7** i ilustracje: profile, topologia, nieuporządkowanie, struktura drugorzędowa, PAE, wykres Ramachandrana, superpozycje 3D.
- [ ] **Dane wejściowe**: `target.fasta`, `template.fasta`, `template.pdb`/`.cif` oraz wynik przyrównania BLAST.
- [ ] **Dane AlphaFold**: oryginalne ZIP, `model_0.cif`, pliki JSON i parametry zadania.
- [ ] **Modele przed/po**: `target_initial.pdb`, `target_refined.pdb` (lub udokumentowane odpowiedniki).
- [ ] **Raporty walidacji i refinementu**, identyfikatory zadań oraz wersje/daty użytych narzędzi.

### Pytania, na które raport musi odpowiedzieć

1. Czy struktura TARGET jest **wiarygodna**? Na jakiej podstawie?
2. Które regiony mają **niską pewność predykcji** lub nieprawidłową geometrię?
3. Czy TARGET i TEMPLATE mają podobne właściwości fizykochemiczne, profile i architekturę przestrzenną?
4. Czy refinement rzeczywiście poprawił model — **co poprawił, a czego nie**?
5. Czy można na podstawie tych analiz stwierdzić, że model jest **stabilny termodynamicznie**? Jeśli nie, jakie dalsze analizy byłyby potrzebne?

**Najważniejsza zasada:** dobre pLDDT, mało konfliktów sterycznych i korzystna energia po minimalizacji nie są tym samym. **Pewność predykcji, jakość geometrii i stabilność termodynamiczna** to trzy różne zagadnienia.

## Oficjalne materiały uzupełniające

- [EMBL-EBI: AlphaFold Server — instrukcja krok po kroku](https://www.ebi.ac.uk/training/online/courses/alphafold/alphafold-3-and-alphafold-server/alphafold-server-your-gateway-to-alphafold-3/a-step-by-step-guide-to-generating-predictions-with-alphafold-server/)
- [EMBL-EBI: interpretacja wyników AlphaFold Server](https://www.ebi.ac.uk/training/online/courses/alphafold/alphafold-3-and-alphafold-server/alphafold-server-your-gateway-to-alphafold-3/interpreting-results-from-alphafold-server/)
- [EMBL-EBI: ocena jakości AlphaFold 3](https://www.ebi.ac.uk/training/online/courses/alphafold/alphafold-3-and-alphafold-server/how-to-assess-the-quality-of-alphafold-3-predictions/)
