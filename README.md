# SICK-fr-rebanked

Rebanked version of the [French SICK dataset](https://huggingface.co/datasets/maximoss/sick-fr) for  use with the [GrailLight](https://github.com/RichardMoot/GrailLight) parser and the  [French Neurosymbolic Natural Language Inference](https://github.com/mskandalis/hybrid_nli_fr) engine of Skandalis e.a. (2025).

All sentences have manually corrected part-of-speech tags and supertags. Although  quite a few translation errors of the original dataset have been corrected, there are likely many others left. Sentence numbers have been preserved from the Skandalis e.a. (2025) paper.

The recommended way to run the [GrailLight](https://github.com/RichardMoot/GrailLight) parser on this dataset is to start [SWI Prolog](https://www.swi-prolog.org),  then run the following commands to load the necessary files and start the parser.

```
[grail_light_nd].      % loads the GrailLight parser
[sickfr_superpos].     % loads the rebanked SICK dataset
[sick_crosses].        % loads the bootstrap parser output
chart_parse_all.       % parses all sentences
```

Depending on your installation, you may need to use full paths for loading the different files, e.g. using  `[/path/to/sick/sickfr_superpos].` where `/path/to/sick` is the path to this repository on your computer.

This will generate a number of files:
- The text file `unparsed` contains all sentence ids for which no proof was found. This file should be empty at the end of the parse, but  
- The Prolog file `proofs.pl` will contain all proved sentences in the GrailLight natural deduction format.
- The Prolog file `semantics.pl` generates the meaning of each sentence as computed by GrailLight's French Discourse Representation Theory (DRT) grammar.
- The LaTeX file `latex_proofs.tex` contains the proofs in  LaTeX format.
- The LaTeX file  `semantics.tex` contains the semantics in LaTeX format.

If you find this work useful, please cite the following papers.

### References

Skandalis, M., Moot, R., Retoré, C., & Robillard, S. (2024) _New Datasets for Automatic Detection of Textual Entailment and of Contradictions between Sentences in French_ In Proceedings of the 2024 Joint International Conference on Computational Linguistics, Language Resources and Evaluation (LREC-COLING 2024) (pp. 12173-12186).

Skandalis, M., Abzianidze, L., Moot, R., Retoré, C., & Robillard, S. (2025) _Neurosymbolic AI for Natural Language Inference in French: combining LLMs and theorem provers for semantic parsing and natural language reasoning_ In Proceedings of the 16th International Conference on Computational Semantics (pp. 242-253).
