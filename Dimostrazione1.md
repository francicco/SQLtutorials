# SQL: Preparare un Database Step by Step.
## Di cosa parliamo.
Questo dataset proviene da un dataset a cui sto lavorando. Sto cercando di trovare elementi nel genome di differenti specie di farfalle che possa essere legato ad un determinato fenotipo, in questo caso bruchi con un comportamento gregario.

![IMG_5578](https://github.com/francicco/SQLtutorials/assets/9006870/5bea9cb4-30e8-4ad0-9041-882497d90588)

Ho gia' eseguito differenti analisi tra cui analisi filogenetiche, cercando di ricostruire le relazioni filogenetiche tra le varie species appartenenti alla tribu' degli Heliconiini

![41467_2023_41412_Fig1_HTML](https://github.com/francicco/SQLtutorials/assets/9006870/7b1db0ce-0080-47d9-bdd8-0391bc2485ae)

## Che dati abbiamo.
1. Genomi su cui sono stati annotati i geni codificanti per proteine.
2. La loro classificazione in [ortogruppi]([https://link-url-here.org](https://en.wikipedia.org/wiki/Sequence_homology#Orthology)).
3. Caratterizzazioni di [regioni conservate (CR)](https://en.wikipedia.org/wiki/Conserved_non-coding_sequence).
4. Evidenza di una accelerazione di sostituzioni nucleotidiche in queste CR nelle specie con comportamento gregario.
5. Una serie di analisi sui geni codificanti, tra cui convergenza (`CSUBST`), rilassamento (`Relaxed`) o intensificzione (`Intensified`) della pressione evolutiva e differenza di selezione tra specie gregarie e non gregarie (`BUSTEDPH`).

![41467_2023_41412_Fig6_HTML](https://github.com/francicco/SQLtutorials/assets/9006870/6040c545-8e84-481a-89b2-4af7b1505010)
