# sekwencjonowanie_doktorat
Opis oraz dokładną procedurę obróbki danych w celu otrzymania i identyfikacji wariantów genomu oraz oszacowania potencjalnych pojedynczych polimorfizmów nukleotydów (SNIP) oraz większych insercji/delecji w obszarze w stosunku do genomu referencyjnego. 
Poniższy plik zawiera opis oraz dokładną procedurę obróbki danych w celu otrzymania i identyfikacji wariantów genomu oraz oszacowania potencjalnych pojedynczych polimorfizmów nukleotydów (SNIP) oraz większych insercji/delecji w obszarze w stosunku do genomu referencyjnego.

Jako pierwszy etap procesu obróbki danych otrzymanych w wyniku sekwencjonowania  można uznać przekształcenie formatu BCL w którym firma wykonująca procedurę sekwencjonowania udostępnia wyniki reakcji na format FASTQ zawierający bezpośrednio sekwencję oraz informacje jakościowe na temat sekwencji (FASTQ jest pozbawiony między innymi sekwencje starterów, usuwa też część błędnie namnożonych sekwencji). Służy do tego biblioteka BCL2FASTQ z strony iluminy.





Komenda: bcl2fastq -R (Ścieżka do folderu z wynikami run’u) -o (Ścieżka do folderu docelowego)

Kolejnym etapem jest rozpoznanie odpowiadających sobie sekwencji (forward i revers) oraz usunięcie barcodów które zostały dodane do analizowanych sekwencji w celu identyfikacji. W tym celu wykorzystano program Sabre.  Program dzięki barkodom dodanym do reakcji program potwierdza komplementarność pojedynczych sekwencji.

Następnie usuwamy szczątkowe sekwencje adaptera z odczytów FASTQ typu paired-end (PE), opcjonalnie przycinając końce o niskiej jakości (duża ilość N odczytów lub sporna długość końców) i łącząc nakładające się na siebie sekwencje typu paired-end w jeden odczyt. Do tego wykorzystano program AdapterRemoval.

#AdapterRemoval – pokazuje instrukcje
#--file1 → plikR1
#--file2 → plikR2
#--collapse - połącz
#--trimns – usuwa N z 3’ i 5’ końca
#--trimqualities – obcina odczyty o niskiej jakości
#--gzip – skompresuj
#-- minalignmentlength 4 – usuwa odczyty mniejsze niż 4
#--basename Saiga011A_NS018 – nazwa pliku wyjściowego

Otrzymaną sekwencję przyrównujemy do sekwencji referencyjnej. BWA to program do wyrównywania odczytów sekwencjonowania względem dużego genomu referencyjnego (np. genomu ludzkiego). Składa się z dwóch głównych komponentów, jednego dla odczytów krótszych niż 150bp i drugiego dla dłuższych odczytów.
#Ważne żeby po nadaniu nazwy plikowi wyjściowemu dopisać nazwę rozszerzenia (.sam)
SAMtools to zestaw narzędzi do interakcji i postprocesowania krótkich linii odczytu sekwencji DNA w SAM/BAM. Narzędzia te manipulują dopasowaniami w formacie BAM. Importuje i eksportuje do formatu SAM (Sequence Alignment / Map), sortuje, łączy i indeksuje.

	
#-S - Traktuj odczyty sparowanych końców i odczyty jednostkowe.

Następnie możemy poddać sekwencje indeksowaniu co pozwoli na wizualizacje w programie Tablet wydanym „The James Hutton Institute”




BCFtools - BCFtools to zestaw narzędzi, które manipulują wywołania wariantów w formacie Variant Call (VCF) i jego binarnym odpowiedniku BCF.

mpileup - Generuje VCF lub BCF zawierający prawdopodobieństwa genotypu dla jednego lub wielu plików dopasowania (BAM lub CRAM).

Po sprawdzeniu i uzupełnieniu vcf tworzymy plik zgodności (consensus) który jest podstawą do dalszych analiz m. in. tworzenia drzewa filogenetycznego.
