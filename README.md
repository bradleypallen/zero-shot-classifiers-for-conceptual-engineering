# zero-shot-classifiers-for-conceptual-engineering
 Experiments using prompt programming of large language models to implement targets of conceptual engineering as zero-shot chain-of-thought classifiers.

## Overview
This repository contains code for the paper [Conceptual Engineering Using Large Language Models](https://arxiv.org/abs/dddd.ddddd), accepted for presentation at the 5th Conference on Philosophy of Artificial Intelligence, Erlangen, 15 - 16 December 2023.

## Requirements
- Python 3.11 or higher. 
- A valid OpenAI API key. 

## Installation
Set the ‘OPENAI_API_KEY’ environment variable to your OpenAI account key, clone this repository, change directory into the repository directory, and then execute the following commands:
```
$ python -m venv env
$ source env/bin/activate
$ pip install -r requirements.txt
```

## License
MIT.

## Background
*Conceptual engineering* (CE) is a philosophical methodology concerned with the assessment and improvement of concepts [1]. Koch, Löhr and Pinder have surveyed recent work on the theory of CE, discussing different theories defining the targets of CE, i.e., "*what* conceptual engineers are (or should be) trying to engineer" [2]. In one such theory, Nado proposes as targets *classification procedures*, defined as abstract 'recipes' which sort entities "into an 'in'-group and an 'out'-group" [3]. Our work builds on Nado's idea by defining a method for implementing classification procedures consistent with this definition. 

A *large language model* (LLM) is a probabilistic model trained on a natural language corpus that, given a sequence of tokens from a vocabulary occurring in the corpus, generates a continuation of the input sequence. LLMs exhibit remarkable capabilities for natural language processing and generation [4]. Our work uses *prompt engineering* [5] of LLMs to implement classification procedures. 

A *knowledge graph* represents knowledge using nodes for entities and edges for relations [6]. Knowledge graphs are key information infrastructure for many Web applications [7]. Our work leverages knowledge graphs as a source of entities used to evaluate classification procedures.

## Implementation
Figure 1 illustrates our method for implementing classification procedures as zero-shot chain-of-thought [8] classifiers. Given a concept's name and intensional definition and an entity's name and description, we prompt an LLM to generate a rationale arguing for or against the entity as an element of the concept's extension, followed by a final 'positive' or 'negative' answer.

| ![classifier_example.png](classifier_example.png) | 
|:--:| 
| Figure 1. A classification procedure using the 24 August 2006 version of the IAU definition of PLANET, implemented as a zero-shot chain-of-thought classifier, and being applied to the description of the entity DENIS-P J08230313-491201 b. |

## Experiments
To evaluate classification procedures built using this method, we sampled positive and negative examples of a concept from the Wikidata collaborative knowledge graph [9], retrieving for each entity a summary of its Wikipedia page to use as its description. Next, we apply the classification procedure for a given definition of the concept to each example and compute a confusion matrix from the classifications, which provides performance metrics for the classification procedure. False positives/negatives are then reviewed to determine if a given error arises from the concept's definition or the entity's description. All concept definitions are used verbatim. For the LLM, we use GPT-4 [10] with a temperature setting of 0.1.

## Files

- [classification_procedure.py](classification_procedure.py): A Python file defining a Python class ```ClassificationProcedure```, which is used in the accompanying notebooks to define classification procedures for a given concept. 
- [planet_experiment.ipynb](planet_experiment.ipynb): A Python notebook that evaluates three definitions for PLANET: one from the Oxford English Dictionary (OED) [11] and two from the 2006 International Astronomical Union (IAU) General Assembly [12,13]. We sampled 50 positive examples that are instances (P31) of planet (Q634), and 50 negative examples that are instances of substellar object (Q3132741), but not of planet. 
- [woman_experiment.ipynb](woman_experiment.ipynb): A Python notebook that evaluates three definitions for WOMAN: one from the OED [14], the definition provided in Haslanger’s 2000 paper [15], and one from the Homosaurus vocabulary of LGBTQ+ terms [16,17]. We sampled 50 positive examples whose sex or gender (P21) is either female (Q6581072) or trans woman (Q1052281), and 50 negative examples whose sex or gender is either male (Q6581097), non-binary (Q48270), or trans man (Q2449503).
- [planet_experiment.json](planet_experiment.json): A JSON file containing the experimental results of the evaluation implemented in ```planet_experiment.ipynb```.
- [woman_experiment.json](woman_experiment.json): A JSON file containing the experimental results of the evaluation implemented in ```woman_experiment.ipynb```.

## Citation
```
@misc{allen2023conceptual,
      title={Conceptual Engineering Using Large Language Models}, 
      author={Bradley P. Allen},
      year={2023},
      eprint={dddd.ddddd},
      doi={10.48550/arXiv.dddd.ddddd},
      archivePrefix={arXiv},
      primaryClass={cs.AI}
}
```
## References

[1] Herman Cappelen. Fixing language: An essay on conceptual engineering. Oxford University Press, 2018.

[2] Steffen Koch, Guido Löhr, and Mark Pinder. Recent work in the theory of conceptual engineering. Analysis, pages 1–15, 2023.

[3] Jennifer Nado. Classification procedures as the targets of conceptual engineering. Philosophy and Phenomenological Research, 106(1):136–156, 2023.

[4] Tom Brown, Benjamin Mann, Nick Ryder, Melanie Subbiah, Jared D Kaplan, Prafulla Dhariwal, Arvind
Neelakantan, Pranav Shyam, Girish Sastry, Amanda Askell, et al. Language models are few-shot learners.
Advances in neural information processing systems, 33:1877–1901, 2020.

[5] Pengfei Liu, Weizhe Yuan, Jinlan Fu, Zhengbao Jiang, Hiroaki Hayashi, and Graham Neubig. Pre-train,
prompt, and predict: A systematic survey of prompting methods in natural language processing. ACM
Computing Surveys, 55(9):1–35, 2023.

[6] Aidan Hogan, Eva Blomqvist, Michael Cochez, Claudia D’amato, Gerard De Melo, Claudio Gutierrez,
Sabrina Kirrane, José Emilio Labra Gayo, Roberto Navigli, Sebastian Neumaier, Axel-Cyrille Ngonga
Ngomo, Axel Polleres, Sabbir M. Rashid, Anisa Rula, Lukas Schmelzeisen, Juan Sequeda, Steffen Staab,
and Antoine Zimmermann. Knowledge graphs. ACM Computing Surveys, 54(4):1–37, 2021.

[7] Nicolas Heist, Sven Hertling, Daniel Ringler, and Heiko Paulheim. Knowledge graphs on the web-an
overview. Knowledge Graphs for eXplainable Artificial Intelligence, pages 3–22, 2020.

[8] Takeshi Kojima, Shixiang Shane Gu, Machel Reid, Yutaka Matsuo, and Yusuke Iwasawa. Large language
models are zero-shot reasoners. Advances in neural information processing systems, 35:22199–22213,
2022.

[9] Denny Vrandečić and Markus Krötzsch. Wikidata: a free collaborative knowledgebase. Communications
of the ACM, 57(10):78–85, 2014.

[10] OpenAI. Gpt-4 technical report, 2023.

[11] Oxford English Dictionary. ”planet, n.”. https://www.oed.com/dictionary/planet_n, 2023. Accessed:
2023-10-17.

[12] International Astronomical Union. The iau draft definition of ”planet” and ”plutons”. https://www.
iau.org/news/pressreleases/detail/iau0601/, 2006. Accessed: 2023-10-17.

[13] IAU2006 General Assembly. Result of the iau resolution votes. URL:
http://www.iau.org/static/archives/releases/doc/iau0603.doc, 2006. Accessed: 2023-10-16.

[14] Oxford English Dictionary. ”woman, n.”. https://www.oed.com/dictionary/woman_n, 2023. Accessed:
2023-10-17.

[15] Sally Haslanger. Gender and race: (what) are they? (what) do we want them to be? Noûs, 34(1):31–55,
2000.

[16] Homosaurus. ”women (https://homosaurus.org/v3/homoit0001509)”. https://homosaurus.org/v3/
homoit0001509, 2023. Accessed: 2023-10-17.

[17] Marika Cifor and KJ Rawson. Mediating queer and trans pasts: The homosaurus as queer information
activism. Information, Communication & Society, pages 1–18, 2022.