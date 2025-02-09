# SQL: Preparare un Database Step by Step.
## Di cosa parliamo.
Questo dataset proviene da un dataset a cui sto lavorando. Sto cercando di trovare elementi nel genome di differenti specie di farfalle che possa essere legato ad un determinato fenotipo, in questo caso bruchi con un comportamento gregario.

![IMG_5578](https://github.com/francicco/SQLtutorials/assets/9006870/5bea9cb4-30e8-4ad0-9041-882497d90588)

Ho gia' eseguito differenti analisi tra cui analisi filogenetiche, cercando di ricostruire le relazioni filogenetiche tra le varie species appartenenti alla tribu' degli Heliconiini

![41467_2023_41412_Fig1_HTML](https://github.com/francicco/SQLtutorials/assets/9006870/7b1db0ce-0080-47d9-bdd8-0391bc2485ae)
[Cicconardi et al 2024](https://www.nature.com/articles/s41467-023-41412-5)

## Che dati abbiamo.
1. Genomi su cui sono stati annotati i geni codificanti per proteine.
2. La loro classificazione in [ortogruppi](https://en.wikipedia.org/wiki/Sequence_homology#Orthology).
3. Caratterizzazioni di [regioni conservate (RC)](https://en.wikipedia.org/wiki/Conserved_non-coding_sequence).
4. Evidenza di una accelerazione di sostituzioni nucleotidiche in queste CR nelle specie con comportamento gregario.
5. Una serie di analisi sui geni codificanti, tra cui: convergenza evolutiva (`CSUBST`), rilassamento (`Relaxed`) o intensificzione (`Intensified`) della pressione evolutiva, e differenza di selezione tra specie gregarie e non gregarie (`BUSTEDPH`).

![41467_2023_41412_Fig6_HTML](https://github.com/francicco/SQLtutorials/assets/9006870/6040c545-8e84-481a-89b2-4af7b1505010)
[Cicconardi et al 2024](https://www.nature.com/articles/s41467-023-41412-5)

## Come organizzare i dati.
Per prima cosa scarichiamo i dati e vediamo come sono organizzati.
Creaiamo una cartella, entriamo e scarichiamo i file.

Clicca [qui](https://drive.google.com/file/d/1SZdS5arUWnRTxnR6biGmm73irsrS_ga3/view?usp=sharing) e scegli di scaricarlo nella cartella appena creata.
Decomprimiamo il file e guardiamo cosa contiene
```
tar xvf Esercitazione01.data.tar.gz
ls -lh
```

*i)* La lista dei gruppi ortologhi e il risultato delle analisi sui geni codificanti.

```bash
head UpSetdata.tsv
```

|gene|BUSTEDPH|Relaxed|Intensified|CSUBST|
|--|--|--|--|--|
|OG_17702|1|1|0|0|
|OG_3830|1|0|1|0|
|OG_4378|1|1|0|0|
|OG_13014|1|0|1|0|
|OG_12146|1|0|1|0|
|OG_8811|1|0|1|0|
|OG_9465|1|1|0|0|
|OG_13053|1|0|1|0|
|OG_12245|1|0|0|0|

*ii)* La lista delle regioni conservate non-esoniche (CNEE) analizzate, le loro coordinate su un genoma di riferimento, in questo caso *Heliconius melpomene*, e la associazione di queste con i gruppi ortologhi.
```bash
head AllOGs.CNEEs.tsv
```

|--|--|--|--|--|
|--|--|--|--|--|
|Hmel216002o|8361562|8361614|CNEE103574659208.Hmel216002oG470|OG_1|
|Hmel216002o|8361618|8361684|CNEE103575659209.Hmel216002oG470|OG_1|
|Hmel216002o|8361713|8361789|CNEE103576659211.Hmel216002oG470|OG_1|
|Hmel216002o|8364103|8364161|CNEE103577659427.Hmel216002oG470|OG_1|
|Hmel216002o|8364200|8364251|CNEE103578659431.Hmel216002oG470|OG_1|
|Hmel216002o|8365097|8365148|CNEE103579659525.Hmel216002oG470|OG_1|
|Hmel216002o|8365150|8365202|CNEE103580659526.Hmel216002oG470|OG_1|
|Hmel216002o|8373285|8373358|CNEE103581660168.Hmel216002oG470|OG_1|
|Hmel216002o|8376749|8376799|CNEE103582660394.Hmel216002oG470|OG_1|
|Hmel216002o|8382335|8382445|CNEE103583660873.Hmel216002oG470|OG_1|

*iii)* Il risultato dell'analisi dell'accelerazione delle mutazioni delle RC. Questa e' la tebella piu' grande che verra' osservata piu' nel dettaglio e parzialmente filtrata.

```
# PhyloAcc element likelihoods and ID key
# SUMMARY: 148211 loci
# SUMMARY: 297 batches completed
# SUMMARY: 0 batches failed
# SUMMARY: 110907 loci best fit M0
# SUMMARY: 14565 loci best fit M1
# SUMMARY: 22739 loci best fit M2
#
# COLUMN DEFINITIONS
# phyloacc.id:               The ID for the locus as assigned by PhyloAcc
# original.id:               The ID provided in the input bed file
# best.fit.model:            The model (M0, M1, M2) with the best fit based on Bayes Factors
# marginal.likelihood.m0:    The marginal likelihood from M0
# marginal.likelihood.m1:    The marginal likelihood from M1
# marginal.likelihood.m2:    The marginal likelihood from M2
# logbf1:                    The Bayes factor between M1 and M0 (logbf1 = marginal.likelihood.m1 - marginal.likelihood.m0, specified cutoff was 1.0)
# logbf2:                    The Bayes factor between M1 and M2 (logbf2 = marginal.likelihood.m1 - marginal.likelihood.m2, specified cutoff was 1.0)
# logbf3:                    The Bayes factor between M2 and M1 (logbf3 = marginal.likelihood.m2 - marginal.likelihood.m0, specified cutoff was 1.0)
# conserved.rate.m0:         The posterior median of the conserved substitution rate under M0
# accel.rate.m0:             The posterior median of the accelerated substitution rate under M0
# conserved.rate.m1:         The posterior median of the conserved substitution rate under M1
# accel.rate.m1:             The posterior median of the accelerated substitution rate under M1
# conserved.rate.m2:         The posterior median of the conserved substitution rate under M2
# accel.rate.m2:             The posterior median of the accelerated substitution rate under M2
# num.accel.m1:              The number of lineages inferred to be accelerated under M1
# num.accel.m2:              The number of lineages inferred to be accelerated under M2
# accel.lineages.m1:         A comma separated list of the accelerated lineages under M1
# conserved.lineages.m2:     A comma separated list of the conserved lineages under M2
# accel.lineages.m2:         A comma separated list of the accelerated lineages under M2
#
# NOTE: M0 = null model   (no acceleration, classifed when BFs are below cutoffs for M1 and M2)
# NOTE: M1 = target model (acceleration on target lineages only, classifed when logbf1 and logbf2 > cutoffs)
# NOTE: M2 = full model   (acceleration on any branch, classifed when BFs are below cutoffs for M1 and logbf3 > cutoff)
#
# TREE: (((((((((((((((((Htim:0.0122368,Hheu:0.0155344):0.0100966,(Hpac:0.0349386,Hcyd:0.0120393):0.0067774):0.00574459,Hmel:0.0209234):0.0138027,(((Hhel:0.0222092,Hatt:0.026489):0.00621392,(Helv:0.020115,Hpar:0.0201982):0.00801848):0.00333235,Heth:0.0435016):0.00422065):0.00401516,((Hism:0.0326588,Hnum:0.0267505):0.00448093,Hbes:0.0365211):0.00219404):0.00647657,Hnat:0.0410641)MelpomeneSilvaniformis:0.0473324,((Hwal:0.0340401,Hbur:0.0221718):0.0185508,Hege:0.0310878)Wallacei:0.0577313)WallaceiMelpomeneSilvaniformis:0.00891214,(((Hdor:0.0270413,Hxan:0.00928906):0.013169,Hhie:0.0194857):0.0270979,Hheb:0.0352488)Doris:0.0441543)DorisWallaceiMelpomeneSilvaniformis:0.0096769,Haoe:0.134518)AoedesDorisWallaceiMelpomeneSilvaniformis:0.0186028,((((((((Hsap:0.0120231,Hhew:0.00759982):0.00113007,(Hcon:0.00584151,Hele:0.00720072):0.00273662):0.00606778,Hant:0.0195675):0.00800219,(Hsar:0.0166305,Hleu:0.0213261):0.00891472):0.02318,Hric:0.0538056):0.00300561,(Hdem:0.00192607,Hert:0.00183014):0.0301496):0.00448719,(Hper:0.0175261,Hcha:0.0134905):0.0397565)SaraSapho:0.0110336,((((((Hhim:0.01862,Hlat:0.0185209):0.00715708,Heet:0.0234849):0.00273549,(Herd:0.0217735,Hpet:0.0244781):0.00575494):0.0140703,Hher:0.0316699):0.011388,Hhec:0.0385576):0.00510314,((Hhor:0.0243723,Hcly:0.0330448):0.00940957,Htel:0.0265748):0.0135093)Erato:0.00414406)EratoSaraSapho:0.0484619)Heliconius:0.0941642,(((Elyb:0.0995373,Etal:0.0637861):0.0219972,Eali:0.0926351):0.0235548,((Evib:0.0211347,Elam:0.0202407):0.0380538,Eisa:0.0716879):0.0333271)Eueides:0.104856)EueidesHeliconius:0.0977711,(((Avcr:0.0320897,Avpe:0.040991):0.0214878,Avfl:0.0473602)Agraulis:0.125224,Djun:0.118011)DioneAgraulis:0.118057)DioneAgraulisEueidesHeliconius:0.0455916,(((Ptel:0.170937,Diul:0.210993):0.0350486,Dpha:0.238209):0.0174565,Pdid:0.207113)OtherGenera:0.044589)Heliconiini:0.233372,Smor:0.32706)Heliconiinae:0.238019,(Mcin:0.396059,Jcoe:0.323926)Nymphalinae:0.193912)HeliconiinaeNynmphalinae:0.0928239,Bany:0.543547)BanyHeliconiinaeNymphalinae:0.424895,Dple:0.424895)Nymphalidae;
#
phyloacc.id	original.id	best.fit.model	marginal.likelihood.m0	marginal.likelihood.m1	marginal.likelihood.m2	logbf1	logbf2	logbf3	conserved.rate.m0	accel.rate.m0	conserved.rate.m1	accel.rate.m1	conserved.rate.m2	accel.rate.m2	num.accel.m1	num.accel.m2	conserved.lineages.m1	accel.lineages.m1	conserved.lineages.m2	accel.lineages.m2
160-0	CNEE7950895261.Hmel213001oG54	M2	-2031.0059	-2032.2159	-2025.2613	-1.2100104	-6.9546094	5.7446	0.0905013	1	0.0886073	1.73072	0.0817801	0.92104	3	7	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Bany,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,BanyHeliconiinaeNymphalinae	Hcon,Hele,Hel16	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Hpar,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hheb,Haoe,Hsap,Hhew,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Bany,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,BanyHeliconiinaeNymphalinae	Helv,Heth,Hhie,Hcon,Hele,Hlat,Hel16
160-1	CNEE7950995342.Hmel213001oG54	M0	-761.01201	-764.5194	-773.27256	-3.5073882	8.7531613	-12.26055	0.0851524	1	0.0872021	1.88576	0.0753418	0.735617	2	21	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae	Hxan,Evib	Htim,Hheu,Hmel,Hhel,Hatt,Helv,Heth,Hbes,Hnat,Hwal,Hbur,Hege,Hheb,Haoe,Hsap,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Heet,Herd,Hpet,Hher,Hhor,Elyb,Etal,Eali,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Hel00,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae	Hpac,Hcyd,Hpar,Hism,Hnum,Hdor,Hxan,Hhie,Hhew,Hlat,Hhec,Hcly,Htel,Evib,Elam,Eisa,Hel01,Hel09,Hel13,Hel14,Eu03
160-2	CNEE7951095492.Hmel213001oG55	M2	-1104.7675	-1105.6574	-1091.3758	-0.88992946	-14.281638	13.3917	0.0769214	1	0.0753877	1.80304	0.0665552	1.75913	2	9	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hant,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae	Hele,Hsar	Htim,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hheb,Haoe,Hsap,Hhew,Hcon,Hant,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Htel,Etal,Eali,Evib,Elam,Eisa,Avcr,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,Nymphalidae	Hheu,Hnum,Hhie,Hele,Hsar,Hhim,Hcly,Elyb,Avpe
160-3	CNEE7951195575.Hmel213001oG55	M0	-639.45896	-640.8584	-643.43559	-1.3994376	2.5771891	-3.97663	0.0950197	1	0.0928026	1.91433	0.0943468	1.85376	1	3	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Bany,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,BanyHeliconiinaeNymphalinae	Hsap	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Bany,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,BanyHeliconiinaeNymphalinae	Hism,Hsap,Elyb
160-4	CNEE7951295576.Hmel213001oG55	M2	-777.29907	-777.38126	-739.56413	-0.082184729	-37.817134	37.73494	0.0943345	1	0.0782331	0.8125	0.061902	2.62062	3	6	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hege,Hdor,Hxan,Hhie,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae	Hbur,Hheb,Haoe	Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Hism,Hnum,Hbes,Hnat,Hwal,Hege,Hdor,Hxan,Hhie,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae	Htim,Heth,Hbur,Hheb,Haoe,Elyb
160-5	CNEE7951395577.Hmel213001oG55	M1	-728.69455	-722.68555	-727.04983	6.0090007	4.3642769	1.64472	0.0927636	1	0.0855335	1.98756	0.0848213	1.80562	6	10	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hbur,Hege,Hdor,Hhie,Hheb,Haoe,Hcon,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae	Hwal,Hxan,Hsap,Hhew,Hele,Hel15	Htim,Hpac,Hcyd,Hhel,Hatt,Helv,Heth,Hism,Hnum,Hbes,Hnat,Hbur,Hege,Hdor,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hhim,Hlat,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,Nymphalidae	Hheu,Hmel,Hpar,Hwal,Hxan,Hele,Hper,Hcha,Heet,Elyb
160-6	CNEE7951495626.Hmel213001oG55	M0	-471.65959	-475.02065	-480.64973	-3.3610651	5.6290833	-8.99014	0.0517118	1	0.050928	1.79254	0.0503794	1.84365	4	7	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Bany,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,BanyHeliconiinaeNymphalinae,Nymphalidae	Hdem,Hert,Evib,Hel22	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hper,Hcha,Hhim,Hlat,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Bany,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,BanyHeliconiinaeNymphalinae,Nymphalidae	Hdem,Hert,Heet,Evib,Elam,Hel22,Eu03
160-7	CNEE7951595628.Hmel213001oG55	M0	-766.88826	-767.57665	-771.68067	-0.68838899	4.1040181	-4.79241	0.0735423	1	0.0661047	1.11201	0.0609992	0.921342	7	14	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hant,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Bany,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,BanyHeliconiinaeNymphalinae,Nymphalidae	Hhew,Hcon,Hele,Hsar,Hleu,Evib,Hel16	Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hant,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Herd,Hpet,Hher,Hhec,Hhor,Htel,Elyb,Etal,Eali,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Bany,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel17,Hel18,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae,BanyHeliconiinaeNymphalinae,Nymphalidae	Htim,Hnat,Hhew,Hcon,Hele,Hsar,Hleu,Heet,Hcly,Evib,Elam,Hel16,Hel19,Eu03
160-8	CNEE7951695676.Hmel213001oG55	M2	-743.65097	-749.73543	-742.27436	-6.0844557	-7.4610702	1.37661	0.0916361	1	0.0899056	1.48875	0.0828851	2.49365	7	14	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hhew,Hant,Hsar,Hleu,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae	Hwal,Hsap,Hcon,Hele,Hric,Evib,Hel16	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hbur,Hege,Hdor,Hxan,Hheb,Haoe,Hsap,Hhew,Hant,Hsar,Hleu,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Herd,Hher,Hhec,Htel,Elyb,Etal,Eali,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Dple,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae	Hwal,Hhie,Hcon,Hele,Hric,Heet,Hpet,Hhor,Hcly,Evib,Elam,Hel16,Hel31,Eu03
160-9	CNEE7951795804.Hmel213001oG55	M0	-556.31045	-559.66119	-560.18396	-3.3507363	0.52277029	-3.87351	0.104559	1	0.106162	1.75659	0.103658	1.75111	4	6	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hhie,Haoe,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae	Hxan,Hheb,Hsap,Evib	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Hpar,Heth,Hism,Hbes,Hwal,Hbur,Hege,Hdor,Hhie,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae,Nymphalinae,HeliconiinaeNynmphalinae	Helv,Hnum,Hnat,Hxan,Hheb,Evib
160-10	CNEE7951895887.Hmel213001oG55	M0	-685.59165	-686.95241	-689.69476	-1.3607584	2.7423512	-4.10311	0.107191	1	0.109689	1.90383	0.107945	1.83522	1	3	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini	Hdor	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini	Hege,Hdor,Hcly
160-11	CNEE7951995933.Hmel213001oG55	M2	-1098.2348	-1101.7252	-1093.2795	-3.4903709	-8.4457401	4.9553	0.102962	1	0.102993	1.91317	0.0929403	1.62384	1	5	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae	Hleu	Htim,Hheu,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hsar,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Mcin,Jcoe,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae	Hpac,Hhie,Hant,Hleu,Eali
160-12	CNEE7952095971.Hmel213001oG55	M2	-1087.6404	-1088.986	-1047.8082	-1.3456007	-41.177761	39.8322	0.0525832	1	0.0533277	1.93858	0.0381435	1.52474	0	2	Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hper,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Hcly,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini,Heliconiinae		Htim,Hheu,Hpac,Hcyd,Hmel,Hhel,Hatt,Helv,Hpar,Heth,Hism,Hnum,Hbes,Hnat,Hwal,Hbur,Hege,Hdor,Hxan,Hhie,Hheb,Haoe,Hsap,Hhew,Hcon,Hele,Hant,Hsar,Hleu,Hric,Hdem,Hert,Hcha,Hhim,Hlat,Heet,Herd,Hpet,Hher,Hhec,Hhor,Htel,Elyb,Etal,Eali,Evib,Elam,Eisa,Avcr,Avpe,Avfl,Djun,Ptel,Diul,Dpha,Pdid,Smor,Hel00,Hel01,Hel02,Hel03,Hel04,Hel05,Hel06,Hel07,Hel08,Hel09,Hel10,Hel11,MelpomeneSilvaniformis,Hel12,Wallacei,WallaceiMelpomeneSilvaniformis,Hel13,Hel14,Doris,DorisWallaceiMelpomeneSilvaniformis,AoedesDorisWallaceiMelpomeneSilvaniformis,Hel15,Hel16,Hel17,Hel18,Hel19,Hel20,Hel21,Hel22,Hel23,Hel24,SaraSapho,Hel25,Hel26,Hel27,Hel28,Hel29,Hel30,Hel31,Hel32,Erato,EratoSaraSapho,Heliconius,Eu01,Eu02,Eu03,Eu04,Eueides,EueidesHeliconius,Au01,Agraulis,DioneAgraulis,DioneAgraulisEueidesHeliconius,N01,N02,OtherGenera,Heliconiini	Hper,Hcly
...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  ...  
```

## Creazione del Database (un passo alla volta) e Preparazione dei dati.
Iniziamo con il creare il nostro database (DB). Apriamo phpMyAdmin andando al link del nostro [localhost](http://localhost/phpmyadmin), verifichiamo che tutto funzioni, poi clicchiamo su `SQL` da qui eseguiremo i nostri comandi.
As esempio il primo, la creazione del nostro DB.
	
```sql
CREATE DATABASE `GenomicRegionDB`;
```

e clicchiamo su `GO`.
Avremo a questo punto creato il nostro DB che andremo a popolare con le nostre tabelle.
Per prima cosa pero' dobbiamo attivarlo:
	
```sql
USE `GenomicRegionDB`;
```
	
### Creazione delle nostre tabelle.
1. La prima tabella sara' quella dove ospiteremo la lista dei nostri gruppi di ortologhi e le analisi fatte su queste.
- Qual e' il file?
- Come vogliamo chiamare la tabella?
- Quanti campi contiene?
- Che tipologia di dati?

Diamogli ancora un'occhiata, poi decidiamo...
	
```sql
CREATE TABLE `AnalisiOG` (
    `OGid` VARCHAR(25) PRIMARY KEY,
    `BUSTEDPH` TINYINT(1) NOT NULL CHECK (`BUSTEDPH` IN (0, 1)),
    `Relaxed` TINYINT(1) NOT NULL CHECK (`BUSTEDPH` IN (0, 1)),
    `Intensified` TINYINT(1) NOT NULL CHECK (`Intensified` IN (0, 1)),
    `CSUBST` TINYINT(1) NOT NULL CHECK (`CSUBST` IN (0, 1))
);
```
	
Possiamo eliminarla se pensiamo di aver fatto degli errori durante la creazione:
	
```sql
DROP TABLE `AnalisiOG`;
```

Poi popoliamo la cartella con `LOAD DATA` se phpmyadmin ha i permessi di lettura al file,
	
```sql
LOAD DATA INFILE 'Path/to/UpSetdata.csv'
INTO TABLE `AnalisiOG`
FIELDS TERMINATED BY ',' 
ENCLOSED BY ''
LINES TERMINATED BY '\n'
IGNORE 0 LINES;
```

Alternativamente possiamo usare la GUI di phpMyAdmin...

Per svuotare la tebella...
	
```sql
TRUNCATE TABLE `AnalisiOG`;
```

Il file va modificato leggermente per non avere problemi durante il caricamente dei dati...
Convertiamo il file `UpSetdata.tsv` in un `CSV`. Possiamo usare un comando di `BASH`? Come?

<details>
<summary>Soluzione</summary>
	
```bash
sed 's/\t/,/g' UpSetdata.tsv | grep -v gene > UpSetdata.csv
```
</details>

2. La seconda tabella ospitera' le coordinate delle regioni conservate.
	
```sql
CREATE TABLE `CNEEtable` (
    `Chr` VARCHAR(25),
    `Start` INT(10),
    `End` INT(10),
    `CNEEid` VARCHAR(250) PRIMARY KEY,
    `OGid` VARCHAR(25)
);
```

e popoliamola!
Ci sono problemi? Se si, come possiamo risolverli?

<details>
<summary>Soluzione</summary>
	
```bash
sed 's/\t/,/g' AllOGs.CNEEs.tsv > AllOGs.CNEEs.csv
```
</details>

3. La tabella `elem_lik.txt` e' una tabella molto grande, alcuni campi non ci interessano e ci sono delle linee commentate, che andranno levate prima di importare il file. Per prima cosa dobbiamo decidere che campi scegliere.

I campi che ci interessano maggiormente saranno:

`original.id	best.fit.model	logbf1	logbf2	logbf3	conserved.rate.m0	accel.rate.m0	conserved.rate.m1	accel.rate.m1	conserved.rate.m2	accel.rate.m2	num.accel.m1	num.accel.m2`

Possiamo anche notare che in realta' `original.id` contiene una doppia informazione... il nome del gene in cui il CNEE si trova. Teniamolo ma diamogli un altro campo.
Come possiamo tagliare il file e spezzare il campo `original.id`?

<details>
<summary>Soluzione</summary>
	
```bash
grep -v '#' elem_lik.txt | grep -v phyloacc.id | cut -f2,3,7-17 | sed 's/\./\t/' | sed 's/\t/,/g' > elem_lik.csv
```
</details>

Ok, a questo punto creaiamo la tabella:

```sql
CREATE TABLE `phyloAccCNEE` (
    `CNEEid` VARCHAR(25),
    `GeneName` VARCHAR(25),
    `Model` CHAR(2),
    `logbf1` float(7),
    `logbf2` float(7),
    `logbf3` float(7),
    `conservedRateM0` float(7),
    `accelRateM0` float(7),
    `conservedRateM1` float(7),
    `accelRateM1` float(7),
    `conservedRateM2` float(7),
    `accelRateM2` float(7),
    `NumAccelM1` int(10),
    `NumAccelM2` int(10),
    PRIMARY KEY (CNEEid, GeneName)
);
```

E popoliamola...

C'e' qualcosa che non va? Se si, cosa?

<details>
<summary>Soluzione</summary>
Forse sarebbe meglio cambiare il campo `GeneName` cambiando la lunghezza dei caratteri.
</details>

Come possiamo ovviare al problema?
<details>
<summary>Soluzione</summary>
	
```sql
ALTER TABLE `phyloAccCNEE`
MODIFY COLUMN `GeneName` VARCHAR(50);
```
</details>

Ora Carichiamo i dati e finiamo di popolare il nostro DB.

Ora vai alla [parte 2](https://github.com/francicco/SQLtutorials/blob/main/EsercitazioneSQL/DimostrazioneSQL.2.md)

