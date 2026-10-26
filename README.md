# Potato management trials in Brazil

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23670283.svg)](https://doi.org/10.5281/zenodo.23670283)

1,874 trial results from 45 trials on potato (*Solanum tuberosum*) in Brazil, taken from the tables of 20 open-access Embrapa publications: diseases and pests (late blight, early blight, bacterial wilt, blackleg and soft rot, PVY and PLRV, root-knot nematode, *Diabrotica speciosa*, aphids and leaf beetles), fungicides and alternative products, forecast-based spraying, insecticides and nematicides, cultivar and clone trials, seed-potato degeneration, phosphorus and potassium, organic fertilization, plant density for pre-basic seed, haulm desiccation and soil decompaction. Each row is one number exactly as printed, with the page and table it came from, and points to the trial it belongs to.

Much of the trial evidence on potato diseases, pests and management in Brazil sits in Embrapa bulletins and journal articles, the oldest of them typed in 1974 and two of them scanned. This dataset makes 20 of them searchable and comparable, from Rio Grande do Sul and Santa Catarina to Minas Gerais, São Paulo and the Federal District.

Part of [Paper Rerun](https://paper-rerun.github.io), in its Open Tables line. The work was done by AI agents on a home computer and reviewed by a human before publication.

## What is in it

| | |
|---|---|
| Publications | 20 (Alice and Infoteca-e, Embrapa's open repositories), 1974 to 2025 |
| Trials | 45 (41 experiments, 3 surveys and 1 joint analysis; 38 field, 4 laboratory, 3 greenhouse pots), in Rio Grande do Sul, Santa Catarina, Minas Gerais, São Paulo and the Federal District |
| Records | 1,874 (tuber number, total and marketable yield, yield and share by tuber size class, disease severity, AUDPC and reaction class, virus detection, phytotoxicity and vascular damage after desiccation, nematode counts, pest damage and density, plant traits, leaf nutrients, seed-tuber infection, economics and others) |

Files:
- `data/potato_trials.csv`: the dataset, one record per printed value, with `doc_id` and `trial_id`. Columns are described in `method/schema.json`. Where a record takes a value from its table column or from the whole table (unit, statistic, target, plant part, tuber size class, factor level, assessment time), the value is filled in and `inherited` says where it came from.
- `data/potato_trials_context.csv`: one row per trial (place, season, planting and harvest date, seed tuber, water regime, tillage system, previous crop, cultivar, disease or pest pressure, application method, spray volume and timing, sprays given to every plot, spacing or plant density, base fertilization, design). Join on `doc_id` + `trial_id`; an empty `application_stages` or `n_applications` in a record means the trial's value applies.
- `data/sources.csv`: the 20 publications, with authors, record link and licence notice.
- `qa/decisions.csv`: every disagreement between the two independent transcriptions and how it was settled against the PDF (2,026 decisions, with page).
- `method/`: the schema, the comparison script and the merge script.

## How it was made

1. **Collection.** An agent searched Embrapa's open repositories for publications with quantitative trial tables on this topic and downloaded 20, recording every candidate it did not take, with the reason.
2. **Two independent transcriptions.** Two agents per publication transcribed the trial context, the labels that apply to a whole table or to one column (which statistic, which test, what the letters mean, when the crop was assessed, which plant part, which tuber size class) and every table cell on the topic into a fixed schema, without seeing each other's work, copying numbers exactly as printed and accounting for every data cell.
3. **Comparison and adjudication.** A script paired the two transcriptions, trial by trial, table by table and record by record, and listed every disagreement and every value present in only one of them. Each one was settled by checking the PDF, with the page as evidence.

## How accurate it is

- **Numbers:** no number or significance letter was misread in either transcription. Of the 1,810 paired records, 1,806 matched as printed. The other 4 come from the text of silveira2007, which writes the number as a word ("three plots", "four observation dates"): one transcription put the digit in the printed-value field, the other copied the word. The value is the same in both, and the dataset keeps the word as printed. One more cell, in medeiros2002, prints "7,49*"; one transcription moved the asterisk to the notes instead of the significance field.
- **Context and labels:** one transcription alone was fully correct on 86.4% and 83.8% of records, counting the records it left out (29 and 27). Most errors came from one rule missed across a whole document: the assessment time of the detached-leaflet tests given only in the methods and not carried to the tables (wolter2025, 230 fields), a fertilizer applied once at planting not counted as one application (perez2001, 81), survey rows marked as main effects (daniels2002, 46), the dose left out of the treatment name (salles1999, 42), a table of results (the number of sprays each system called for) read as layout and skipped (gomes2002, 26 records), and a sample-size column skipped (daniels2002, 24 records). One transcription also entered 20 genotype groupings from the text of daniels2004 that do not match the table; the dataset keeps the table and flags the conflict. All 2,026 decisions were settled against the PDF, so the dataset does not depend on either transcription alone.
- **Known limits:** values shown only in charts without printed numbers were not read; 4 records come from a figure (nava2017, Figure 2, which prints the numbers on the chart) and 63 from the text. Common scab, the potato tuber moth, whitefly and blackleg in the field are not covered: the two repositories have no trial table on them that fit, only leaflets, magazine notes or one-page abstracts (blackleg and soft rot appear only in a laboratory test on tubers, lucon1999). No document comes from Paraná; the one candidate from there was left out by the limit of 4 documents per author. menschoy1974 is a scan with a noisy text layer and souza1991 a scan with none; both were read from the rendered page. One document, souza1991 (doses and timing of five haulm desiccants for seed potato in São Joaquim, SC), holds 323 records, 17% of the dataset; without it, one transcription alone is fully correct on 84.1% and 82.3% of records. Only pages 4 and 5 of charchar2007 are used, since its Table 2 on page 6 is about carrot. In 11 trials every plot gets the same routine sprays; they are recorded in the trial's `uniform_sprays`, not in the treatment.

## Licence and citation

- The dataset (the CSV files, schema and scripts) is released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please cite the original publications listed in `data/sources.csv` as well as this dataset.
- The publications are not redistributed. Embrapa's repositories show a CC BY-NC-ND 4.0 notice; the 11 journal articles (5 in Horticultura Brasileira, the others in Ciência Rural, Fitopatologia Brasileira, Nematologia Brasileira, Revista de Ciências Agroveterinárias, Pesquisa Agropecuária Brasileira and Summa Phytopathologica) and the congress paper medeiros2008 keep their own terms, and three Embrapa bulletins (wolter2025, lourenco2020 and lopes2021) print "Todos os direitos reservados" (all rights reserved). souza1991 has no text layer and could not be searched for a notice. Only numbers, which are facts, and short citations are used here.

Errors or missing trials: please [open an issue](https://github.com/paper-rerun/potato-trials-br/issues).

## How this dataset was made

This dataset was built by AI agents (Claude, by Anthropic) on a home computer and reviewed by a human before publication. No value was estimated or typed by hand: each one was transcribed from the source PDF twice, independently, and every disagreement was checked against the page. The method and scripts are in `method/`. How the wider project works: https://github.com/paper-rerun/paper-rerun/blob/main/METHOD.md

Contact: Filipe Tessaro, via issues on this repository.
