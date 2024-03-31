[See below for English](#experimental-bilingual-part-of-speech-tagger-for-english-and-welsh)

# Tagiwr Rhannau Ymadrodd Arbrofol Dwyieithog Cymraeg a Saesneg

[Gweler yma am y ffeiliau](https://github.com/techiaith/spacy-tagiwr-ency/releases/tag/22.10)

Dyma dagiwr **arbrofol** sy'n gallu tagio rhannau ymadrodd testunau Cymraeg a Saesneg o fewn llyfrgell Python 'spaCy'. 

Ysgogiad yr arbrawf hwn oedd y gallai modelau dwyieithog fod yn ddefnyddiol iawn yn y cyd-destun Cymreig gan fod y ddwy iaith yn ymddangos ochr yn ochr neu'n gymysg yn ei gilydd yn aml (yn enwedig fel enwau, teitlau a dyfyniadau).

Hyfforddwyd y model hwn drwy gyfuno data Universal Dependencies Saesneg [English Web Treebank](https://universaldependencies.org/treebanks/en_ewt/index.html) (EWT) gyda data Cymraeg [Corpws Cystrawennol y Gymraeg](https://universaldependencies.org/treebanks/cy_ccg/index.html) (CCG). Rydym yn ddiolchgar i awduron yr EWT a'r CCG am roi'r data ar gael o dan drwydded agored.

Defnyddiwyd 953 brawddeg hyfforddi Gymraeg o'r CCG, a 614 brawddeg brofi Gymraeg fel brawddegau datblygu. Ar gyfer yr elfen Saesneg, defnyddiwyd 12,543 brawddeg hyfforddi o'r EWT, a 2007 brawddeg brofi fel brawddegau datblygu.

Y cywirdeb tagio a adroddwyd yn dilyn y broses hyfforddi oedd **92.7%** ar y set brofi.

Oherwydd bod y data profi a'r data hyfforddi wedi'u ffurfio o frawddegau cymysg Cymraeg a Saesneg, nid oes modd inni dorri'r ffigwr hwnnw i lawr fesul iaith. Bwriadwn gynnal gwerthusiad ychwanegol gyda data profi newydd i ddadansoddi cywirdeb model dwyieithog gyda'r Gymraeg a'r Saesneg ar wahân. O hyfforddi modelau unigol Saesneg a Chymraeg ar yr un data, roedd y cywirdeb yn 93% ar gyfer y Saesneg a 90% ar gyfer y Gymraeg. Disgwyliwn y bydd y ffigwr fesul iaith ar gyfer y model dwyieithog yn is na 92.7% o fewn defnydd cyffredinol, gyda thuedd tuag at y Saesneg gan fod mwy o ddata Saesneg ar gael na data Cymraeg. Serch hynny, mae dadansoddiad cychwynnol o ganlyniadau'r model dwyieithog yn hynod o addawol. Gweler yr adran [Enghreifftiau Defnydd](#enghreifftiau-defnydd) isod am enghreifftiau.

## Gosod spaCy

I ddefnyddio'r tagiwr, rhaid yn gyntaf gosod spaCy. Argymhellwn osod fersiwn 2.3.9 am y tro gan nad ydym eto wedi dal i fyny gyda'r newidiadau sylweddol i'r llyfrgell yn fersiwn 3 (mae'n fwriad gennym ddiweddaru'r cydrannau Cymraeg a'u trosglwyddo i ddarparwyr spaCy). 

Gallwch ei osod fel a ganlyn:

```
python -m venv .env
source .env/bin/activate
pip install -U pip setuptools wheel
pip install -U spacy==2.3.9
```

Gweler https://v2.spacy.io/ am fwy o fanylion am spaCy v2.

## Gosod pecyn iaith
Crewyd pecyn iaith dwyieithog penodol ar gyfer y cyfuniad Saesneg a Chymraeg gyda'r cod iaith `ency`. Rhaid dadzipio'r ffeil `ency.zip` a geir yn y storfa hon a gosod y ffolder `ency` o fewn ffolder `lang` spaCy yn eich amgylchedd python. Dyma enghraifft o'i leoliad mewn amgylchedd Python 3.10:

```
.env/lib/python3.10/site-packages/spacy/lang/ency
```

Mae'r ffolder iaith `ency` yn cynnwys map tagiau estynedig sy'n ychwanegu tagiau XPOS Cymraeg y corpws UD CCG at dagiau'r corpws Saesneg. Gan fod y tagiau xpos yn wahanol rhwng y ddwy iaith, ond bod y tagiau UPOS yn cyfateb, mae modd dadansoddi i ba iaith mae'r model wedi priodoli pob tocyn o'i XPOS, tra'n cynnal cysondeb trawsieithol trwy'r UPOS.

Nid ydym eto wedi cynnwys lemateiddio Cymraeg yn y model, felly dim ond lemau ar gyfer tocynnau sydd wedi eu tagio gyda thag XPOS Saesneg sydd gan y model ar hyn o bryd.

## Gosod y model
Ar ôl lawrlwytho a dadzipio ffeil y model, sef `ency_bi_model.zip`, gallwch osod y model, sef `ency_bi_model` unrhyw le ar eich system, dim ond eich bod yn gosod y llwybr priodol ato wrth ei lwytho yn eich cod Python:

```
nlp = spacy.load("ency_bi_model")
```

y ffordd hawsaf yw gosod ffolder y model yn yr un cyfeiriadur a'ch ffeil.

```
nlp = spacy.load(path/to/model)
```

# Enghreifftiau Defnydd

```
import spacy

nlp = spacy.load("ency_bi_model")

text = "Dywedodd wrthi 'Don't do that, you might get hurt', ond dim ond syllu arni a wnaeth hi."

doc = nlp(text)

for token in doc:
    print (token.text, token.tag_, token.pos_, token.lemma_)
```

Allbwn:

(geirffurf, XPOS, UPOS, lema os Saesneg)

```
Dywedodd verb VERB Dywedodd
wrthi cprep ADP wrthi
' `` PUNCT '
Do VBP AUX do
n't RB PART not
do VB AUX do
that DT DET that
, , PUNCT ,
you PRP PRON -PRON-
might MD VERB may
get VB AUX get
hurt VBN VERB hurt
' '' PUNCT '
, , PUNCT ,
ond cconj CCONJ ond
dim noun NOUN dim
ond cconj CCONJ ond
syllu verbnoun NOUN syllu
arni cprep ADP arni
a rel PRON a
wnaeth verb VERB wnaeth
hi indep PRON hi
. punct PUNCT .
```

Noder mai arfer bwriadol CCG yw trin berfenwau fel 'syllu' fel enwau yn htrach na berfenwau, hyd yn oed pan fônt yn ymddwyn yn fwy berfol. Mae hyn yn un dehongliad dilys o'r berfenw, ond gall fod yn arfer dieithr i rai sydd wedi arfer trin berfenwau berfol fel berfau. Mae'r tagiwr hwn wedi etifeddu'r arfer hwnnw o'r corpws.

## Enghreifftiau Eraill

```
The DT DET the
cat NN NOUN cat
sat VBD VERB sit
on IN ADP on
the DT DET the
mat NN NOUN mat
and CC CCONJ and
closed VBD VERB close
her PRP$ DET -PRON-
eyes NNS NOUN eye
```

```
Eisteddodd verb VERB Eisteddodd
y art DET y
gath noun NOUN gath
ar prep ADP ar
y art DET y
mat noun NOUN mat
a cconj CCONJ a
chau noun NOUN chau
ei dep PRON ei
llygaid noun NOUN llygaid
```

### Trin ffurf amwys fel 'plant' (to plant, a plant, y plant (=children))
```
plant NN NOUN plant
growing VBG VERB grow
for IN ADP for
begginers NNS NOUN begginer
```

```
I PRP PRON -PRON-
will MD VERB will
plant VB VERB plant
many JJ ADJ many
trees NNS NOUN tree
```

```
mae'r verb VERB mae'r
plant noun NOUN plant
yn pred PART yn
tyfu'n verbnoun NOUN tyfu'n
gyflym pos ADJ gyflym
```

## Creu Priodoledd 'lang' i ddynodi iaith y tocyn

Er mwyn hwyluso mynediad at y dadansoddiad cystrawenol o iaith y tocyn, gellir creu a defnyddio priodoledd gyfaddas ar gyfer nodi'r iaith.

Mae'r cod canlynol yn rhoi'r iaith (un ai 'cy' am Gymraeg neu 'en' am Saesneg) ar gael ar y gwrthrych tocyn fel a ganlyn: `token._.lang`

```
import spacy
from spacy.tokens import Token

def get_lang(token):
    cy_xposes = {'noun': {'POS': 'NOUN'},
                'prep': {'POS': 'ADP'},
                'art': {'POS': 'DET'},
                'pos': {'POS': 'ADJ'},
                'rel': {'POS': 'PRON'},
                'verb': {'POS': 'VERB'},
                'pred': {'POS': 'PART'},
                'place': {'POS': 'PROPN'},
                'punct': {'POS': 'PUNCT'},
                'cconj': {'POS': 'CCONJ'},
                'aux': {'POS': 'AUX'},
                'dem': {'POS': 'PRON'},
                'dep': {'POS': 'PRON'},
                'impf': {'POS': 'AUX'},
                'verbnoun': {'POS': 'NOUN'},
                'adv': {'POS': 'ADV'},
                'cprep': {'POS': 'ADP'},
                'sym': {'POS': 'SYM'},
                'sconj': {'POS': 'SCONJ'},
                'cmp': {'POS': 'ADJ'},
                'person': {'POS': 'PROPN'},
                'refl': {'POS': 'PRON'},
                'num': {'POS': 'NUM'},
                'ante': {'POS': 'AUX'},
                'indep': {'POS': 'PRON'},
                'aff': {'POS': 'PART'},
                'neg': {'POS': 'PART'},
                'sup': {'POS': 'ADJ'},
                'eq': {'POS': 'ADJ'},
                'ord': {'POS': 'ADJ'},
                'pron': {'POS': 'PRON'},
                'work': {'POS': 'PROPN'},
                'int': {'POS': 'PART'},
                'post': {'POS': 'AUX'},
                'org': {'POS': 'PROPN'},
                'card': {'POS': 'NUM'},
                'intr': {'POS': 'PRON'},
                'contr': {'POS': 'PRON'},
                'event': {'POS': 'PROPN'}
                }
    en_xposes = {".",",","-LRB-","-RRB-","``",'""',"''",":","$",
                 "#","AFX","CC","CD","DT","EX","FW","HYPH","IN",
                 "JJ","JJR","JJS","LS","MD","NIL","NN","NNP",
                 "NNPS","NNS","PDT","POS","PRP","PRP$","RB","RBR",
                 "RBS","RP","SP","SYM","TO","UH","VB","VBD","VBG",
                 "VBN","VBP","VBZ","WDT","WP","WP$","WRB","ADD",
                 "NFP","GW","XX","BES","HVS", "_SP"}
    if token.tag_ in cy_xposes:
        return "cy"
    elif token.tag_ in en_xposes:
        return "en"
    else:
        return "unknown"

Token.set_extension("lang", getter=get_lang, force=True)

doc = nlp("The car drove over the plant. Aeth y plant am dro yn y car.")
[(t.text, t._.lang) for t in doc]

# CANLYNIAD/RESULT
# [('The', 'en'),
#  ('car', 'en'),
#  ('drove', 'en'),
#  ('over', 'en'),
#  ('the', 'en'),
#  ('plant', 'en'),
#  ('.', 'cy'),
#  ('Aeth', 'cy'),
#  ('y', 'cy'),
#  ('plant', 'cy'),
#  ('am', 'cy'),
#  ('dro', 'cy'),
#  ('yn', 'cy'),
#  ('y', 'cy'),
#  ('car', 'cy'),
#  ('.', 'cy')]
```
Sylwer fod yr uchod yn gweithio ar sail dadansoddiad cystrawenol o rannau ymadrodd tocynnau'r brawddegau, ac y gellir felly ei ddefnyddio yn ogystal neu yn hytrach na dulliau ystadegol ar sail llythrennau'r tocyn (fel cld2 a cld3, er enghraifft). 

Diolchwn i Lywodraeth Cymru am ariannu'r gwaith hwn.

# Experimental Bilingual Part-of-Speech Tagger for English and Welsh

[See here for the files](https://github.com/techiaith/spacy-tagiwr-ency/releases/tag/22.10)

This is an **experimental** tagger that can tag parts of speech in Welsh and English texts within the Python library 'spaCy'.

The impetus for this experiment was that bilingual models could be very useful in the Welsh context, as the two languages often appear in the same documents in Wales (especially as names, titles and quotations).

This model was trained by combining English Universal Dependencies data [English Web Treebank](https://universaldependencies.org/treebanks/en_ewt/index.html) (EWT) with Welsh data [Corpws Cystrawenol y Gymraeg](https://universaldependencies .org/treebanks/cy_ccg/index.html) (CCG). We are grateful to their authors for making the data available under an open license.

953 Welsh training sentences from the CCG and 614 Welsh testing sentences were used as development sentences. For the English element, 12,543 training sentences and 2007 test sentences from the EWT corpus were used as development sentences.

The tagging accuracy reported following the training process was **92.7%** on the test set.

Because the testing data and the training data are made up of mixed Welsh and English sentences, we are unable to break that figure down by language. We intend to carry out an additional evaluation with new test data to analyze the accuracy of a bilingual model both in Welsh and English separately. From training individual English and Welsh models on the same data, the accuracy was 93% for the English language and 90% for the Welsh language. We expect that the figure per language for the bilingual model will be lower than 92.7% within general use, with a tendency towards English as there is more English data available than Welsh data. Nevertheless, an initial analysis of the results of the bilingual model is extremely promising. See the [Usage Examples](#usage-examples) section below for examples.

## Installing spaCy

To use the tagger, spaCy must first be installed. We currently recommend installing version 2.3.9 as we have not yet caught up with the significant changes to the library in version 3 (we intend to update the Welsh components and submit them to spaCy's creators).

You can install spaCy as follows:

```
python -m venv .env
source .env/bin/activate
pip install -U pip setuptools wheel
pip install -U spacy==2.3.9
```

See https://v2.spacy.io/ for more info regarding spaCy v2.

## Installing the language pack
A specific bilingual language pack with the language code `ency` was created for the English and Welsh language combination. To install, the `ency.zip` file found in this repository under the Releases menu must be unzipped and the `ency` folder placed within the spaCy `lang` folder in your Python environment. Here is an example of its location in a Python 3.10 environment:

```
.env/lib/python3.10/site-packages/spacy/lang/ency
```

The `ency` language folder contains an extended tag map that adds the Welsh XPOS tags of the UD CCG corpus to the tags of the English corpus. Since the XPOS tags are different for the two languages but the UPOS tags don't, it is possible to analyze to which language the model has assigned each token from its XPOS, while maintaining cross-linguistic consistency through the UPOS.

We have not yet included Welsh lemmatization in the model, so the model currently has no lemmas for tokens that have been tagged with an English XPOS tag.

## Installing the model
After downloading and unzipping the model file, `ency_bi_model.zip`, you can install the model, `ency_bi_model`, anywhere on your system, as long as you set the appropriate path to it when loading it in your Python code:

```
nlp = spacy.load(path/to/model)
```

the easiest way is to place the model folder in the same directory as your file, and then use:

```
nlp = spacy.load("ency_bi_model")
```

# Usage Examples

```
import spacy

nlp = spacy.load("ency_bi_model")

text = "Dywedodd wrthi 'Don't do that, you might get hurt', ond dim ond syllu arni a wnaeth hi."

doc = nlp(text)

for token in doc:
    print (token.text, token.tag_, token.pos_, token.lemma_)
```

Output:

(wordform, XPOS, UPOS, lemma if English)

```
Dywedodd verb VERB Dywedodd
wrthi cprep ADP wrthi
' `` PUNCT '
Do VBP AUX do
n't RB PART not
do VB AUX do
that DT DET that
, , PUNCT ,
you PRP PRON -PRON-
might MD VERB may
get VB AUX get
hurt VBN VERB hurt
' '' PUNCT '
, , PUNCT ,
ond cconj CCONJ ond
dim noun NOUN dim
ond cconj CCONJ ond
syllu verbnoun NOUN syllu
arni cprep ADP arni
a rel PRON a
wnaeth verb VERB wnaeth
hi indep PRON hi
. punct PUNCT .
```

Note that CCG deliberately treats verbnouns such as 'syllu' as nouns rather than verbs, even when they are verbal in nature. Whilst this is one valid interpretation, it may be unfamiliar to many who are used to conceptualizing verbnouns as verbs. This tagger has inherited this behaviour from the corpus.

## Other Examples

```
The DT DET the
cat NN NOUN cat
sat VBD VERB sit
on IN ADP on
the DT DET the
mat NN NOUN mat
and CC CCONJ and
closed VBD VERB close
her PRP$ DET -PRON-
eyes NNS NOUN eye
```

```
Eisteddodd verb VERB Eisteddodd
y art DET y
gath noun NOUN gath
ar prep ADP ar
y art DET y
mat noun NOUN mat
a cconj CCONJ a
chau noun NOUN chau
ei dep PRON ei
llygaid noun NOUN llygaid
```

### Dealing with the ambiguity of 'plant' (to plant, a plant, y plant (=children))
```
plant NN NOUN plant
growing VBG VERB grow
for IN ADP for
begginers NNS NOUN begginer
```

```
I PRP PRON -PRON-
will MD VERB will
plant VB VERB plant
many JJ ADJ many
trees NNS NOUN tree
```

```
mae'r verb VERB mae'r
plant noun NOUN plant
yn pred PART yn
tyfu'n verbnoun NOUN tyfu'n
gyflym pos ADJ gyflym
```

## Creating a'lang' attribute to indicate a token's language

To facilitate access to a syntactical analysis of the token's language, a custom attribute can be created to indicated the language.

The following code provides the language (either as 'cy' for Welsh or 'en' for English) on the token object, as follows: `token._.lang`

```
import spacy
from spacy.tokens import Token

def get_lang(token):
    cy_xposes = {'noun': {'POS': 'NOUN'},
                'prep': {'POS': 'ADP'},
                'art': {'POS': 'DET'},
                'pos': {'POS': 'ADJ'},
                'rel': {'POS': 'PRON'},
                'verb': {'POS': 'VERB'},
                'pred': {'POS': 'PART'},
                'place': {'POS': 'PROPN'},
                'punct': {'POS': 'PUNCT'},
                'cconj': {'POS': 'CCONJ'},
                'aux': {'POS': 'AUX'},
                'dem': {'POS': 'PRON'},
                'dep': {'POS': 'PRON'},
                'impf': {'POS': 'AUX'},
                'verbnoun': {'POS': 'NOUN'},
                'adv': {'POS': 'ADV'},
                'cprep': {'POS': 'ADP'},
                'sym': {'POS': 'SYM'},
                'sconj': {'POS': 'SCONJ'},
                'cmp': {'POS': 'ADJ'},
                'person': {'POS': 'PROPN'},
                'refl': {'POS': 'PRON'},
                'num': {'POS': 'NUM'},
                'ante': {'POS': 'AUX'},
                'indep': {'POS': 'PRON'},
                'aff': {'POS': 'PART'},
                'neg': {'POS': 'PART'},
                'sup': {'POS': 'ADJ'},
                'eq': {'POS': 'ADJ'},
                'ord': {'POS': 'ADJ'},
                'pron': {'POS': 'PRON'},
                'work': {'POS': 'PROPN'},
                'int': {'POS': 'PART'},
                'post': {'POS': 'AUX'},
                'org': {'POS': 'PROPN'},
                'card': {'POS': 'NUM'},
                'intr': {'POS': 'PRON'},
                'contr': {'POS': 'PRON'},
                'event': {'POS': 'PROPN'}
                }
    en_xposes = {".",",","-LRB-","-RRB-","``",'""',"''",":","$",
                 "#","AFX","CC","CD","DT","EX","FW","HYPH","IN",
                 "JJ","JJR","JJS","LS","MD","NIL","NN","NNP",
                 "NNPS","NNS","PDT","POS","PRP","PRP$","RB","RBR",
                 "RBS","RP","SP","SYM","TO","UH","VB","VBD","VBG",
                 "VBN","VBP","VBZ","WDT","WP","WP$","WRB","ADD",
                 "NFP","GW","XX","BES","HVS", "_SP"}
    if token.tag_ in cy_xposes:
        return "cy"
    elif token.tag_ in en_xposes:
        return "en"
    else:
        return "unknown"

Token.set_extension("lang", getter=get_lang, force=True)

doc = nlp("The car drove over the plant. Aeth y plant am dro yn y car.")
print ([(t.text, t._.lang) for t in doc])

# CANLYNIAD/RESULT
# [('The', 'en'),
#  ('car', 'en'),
#  ('drove', 'en'),
#  ('over', 'en'),
#  ('the', 'en'),
#  ('plant', 'en'),
#  ('.', 'cy'),
#  ('Aeth', 'cy'),
#  ('y', 'cy'),
#  ('plant', 'cy'),
#  ('am', 'cy'),
#  ('dro', 'cy'),
#  ('yn', 'cy'),
#  ('y', 'cy'),
#  ('car', 'cy'),
#  ('.', 'cy')]
```
Note that the above works on the basis of a syntactical analysis of the sentences's tokens, and can therefore be used in place of or in conjunction of statistical analyses based on the letters occuring in texts (as in cld2 and cld3, for example). 

We thank the Welsh Government for financing this work.
