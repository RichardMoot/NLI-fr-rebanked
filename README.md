# SICK-fr-rebanked

## Datasets

The [French SICK dataset](https://huggingface.co/datasets/maximoss/sick-fr) of Skandalis e.a. (2024) is an automatic translation of the original [English SICK dataset](https://huggingface.co/datasets/RobZamp/sick) of Marelli e.a. (2014).

The [French GQNLI dataset](https://github.com/mskandalis/gqnli-french) of Skandalis e.a. (2024), is the French translation of the [GQNLI dateset](https://github.com/ruixiangcui/gqnli) for evaluating reasoning with generalised quantifiers of Cui e.a. (2022). 

The [French FraCaS dataset](https://gitlab.inria.fr/semagramme-public-projects/resources/french-fracas) of Amblard e.a. (2020) is the French translation of the FraCaS dataset of Cooper e.a. (1996).

## Rebanked versions

The repository contains rebanked versions of the following French language Natural Language Inference datasets.
- SICK (19 681 sentences, in separate files `sickfr_superpos.pl` and `sick_crosses.pl` due to size)
- GQNLI (703 sentences, single file `gqnli_fr_superpos.pl`)
- FraCaS (877 sentences, single file `fracas_fr_superpos.pl`)

These dataset are intended to be used with the [GrailLight](https://github.com/RichardMoot/GrailLight) parser, and the [French Neurosymbolic Natural Language Inference](https://github.com/mskandalis/hybrid_nli_fr) engine of Skandalis e.a. (2025).

All sentences have manually corrected part-of-speech tags and supertags. Although quite a few translation errors of the original SICK-fr dataset have been corrected, there are likely many others left. Sentence numbers have been preserved from the Skandalis e.a. (2025) paper.

The recommended way to run the [GrailLight](https://github.com/RichardMoot/GrailLight) parser on this dataset is to start [SWI Prolog](https://www.swi-prolog.org),  then run the following commands to load the necessary files and start the parser.

```
[grail_light_nd],      % loads the GrailLight parser
[sickfr_superpos],     % loads the rebanked SICK dataset
[sick_crosses],        % loads the bootstrap parser output
chart_parse_all.       % parses all sentences
```

Depending on your installation, you may need to use full paths for loading the different files, e.g. using  `[/path/to/sick/sickfr_superpos].` where `/path/to/sick` is the path to this repository on your computer.

Running the parser will generate a number of files:
- The text file `unparsed` contains all sentence ids for which no proof was found. This file should be empty at the end of the parse, but making changes to the `sickfr_superpos.pl` file can make sentences underivable.
- The Prolog file `proofs.pl` will contain all proved sentences in the GrailLight natural deduction format.
- The Prolog file `semantics.pl` generates the meaning of each sentence as computed by GrailLight's French Discourse Representation Theory (DRT) grammar.
- The LaTeX file `latex_proofs.tex` contains the proofs in LaTeX format.
- The LaTeX file  `semantics.tex` contains the semantics in LaTeX format.

## File format

The files in this repository are presented in the form expected by the GrailLight parser, giving word, part-of-speech tag, lemma and formula for each item. However, the  [GrailLight](https://github.com/RichardMoot/GrailLight) repository contains a Prolog script [`print_sents.pl`](https://github.com/RichardMoot/GrailLight/blob/master/print_sents.pl) which takes one of these files as input and produces outputs in different formats.

For example, the following sequence of Prolog commands will transform the SICK dataset into the input expected by the supertagger and output it to `FILE.txt`.
```
tell('FILE.txt'),
[print_sents],
[sickfr_superpos],
print_sents,
told.
```

Similarly, the following sequence of Prolog commands will transform the GQNLI dataset into tokenized text input for evaluating the entire NLI treatment chain and output it to `FILE.txt`.
```
tell('FILE.txt'),
[print_sents],
[gqnli_fr_superpos],
print_words,
told.
```


## Comments

These rebanked versions allow us to separate errors at different levels and evaluate the [French Neurosymbolic Natural Language Inference](https://github.com/mskandalis/hybrid_nli_fr) engine of Skandalis e.a. (2025) with silver input data containing fewer errors than starting from raw input text. For example, the supertagger, trained on a journalistic corpus with longer sentences has trouble with short, declarative sentences like we find in SICK. It will tend to analyse phrases like "Un chien court" (_a dog runs_) as the noun phrase _a short dog_. This is indeed a possible reading: "court" is polysemous in French and can be a noun (tennis court), an adjective meaning "short" and a present tense verb meaning "runs". Cases like these have been manually corrected with the intended analysis.

As another example, the FraCaS dataset contains a section on gapping, where the supertagger tends to have problems with gapping of intransitive verbs due to these being nearly absent from the training data. However, with the correct supertag, this section becomes very easy.

Another possible use case is to add the training data from the different datasets to the [TLGbank](https://github.com/RichardMoot/TLGbankLight) training data of (Moot 2015) and retrain the [DeepGrail supertagger](https://gitlab.irit.fr/pnria/global-helper/deepgrail_tagger) with these additional data.

## License

This work is presented _as is_ to help to research community and under the most general license compatible with all the datasets from which it is derived (CC-BY-NC-SA-4.0). If you find any remaining errors in these files (translation, part-of-speech tag, lemma, formula, parse), please notify me and I will fix these as soon as possible.

If you find this work useful, please cite Skandalis e.a. (2024, 2025) and the authors of the original datasets.

### References

Amblard, M., Beysson, C., de Groote, P., Guillaume, B. & Pogodalla, S. (2020), A French Version of the FraCaS Test Suite _In_ Language Resources and Evaluation Conference (LREC 2020), Marseille, France

Cooper, R., Crouch, D., van Eijck, J., Fox, C., van Genabith, J., Jaspars, J., Kamp, H., Pinkal, M., Milward, D., Poesio, M., Pulman, S. (1996) FraCaS: Using the framework, Deliverable D16, University of Edinburgh.

Cui, R., Hershcovich, D., & Søgaard, A. (2022) Generalized Quantifiers as a Source of Error in Multilingual NLU Benchmarks. _In_ Proceedings of the 2022 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies (NAACL 2022), pages 4875–4893, Seattle, United States.

Marelli, M., Menini, S., Baroni, M., Bentivogli, L., Bernardi, R., & Zamparelli, R. (2014) A SICK cure for the evaluation of compositional distributional semantic models _In_ Proceedings of the 9th International Conference on Language Resources and Evaluation, LREC 2014 (pp. 216-223). European Language Resources Association (ELRA).

Moot, R. (2015). A type-logical treebank for French. _Journal of Language Modelling_, 3(1), 229-264.

Skandalis, M., Moot, R., Retoré, C., & Robillard, S. (2024) New Datasets for Automatic Detection of Textual Entailment and of Contradictions between Sentences in French _In_ Proceedings of the 2024 Joint International Conference on Computational Linguistics, Language Resources and Evaluation (LREC-COLING 2024) (pp. 12173-12186).

Skandalis, M., Abzianidze, L., Moot, R., Retoré, C., & Robillard, S. (2025) Neurosymbolic AI for Natural Language Inference in French: combining LLMs and theorem provers for semantic parsing and natural language reasoning _In_ Proceedings of the 16th International Conference on Computational Semantics (pp. 242-253).
