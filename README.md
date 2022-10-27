ENGLISH BELOW

# Tagiwr Rhannau Ymadrodd Arbrofol Dwyieithog Cymraeg a Saesneg

[Gweler yma am y ffeiliau](https://github.com/techiaith/spacy-tagiwr-ency/releases/tag/22.10)

Dyma dagiwr **arbrofol** sy'n gallu tagio rhannau ymadrodd testunau Cymraeg a Saesneg o fewn llyfrgell Python 'spaCy'. 

Ysgogiad yr arbrawf hwn oedd y gallai modelau dwyieithog fod yn ddefnyddiol iawn yn y cyd-destun Cymreig gan fod y gall y ddwy iaith ymddangos ochr yn ochr neu'n gymysg yn ei gilydd (yn enwedig fel enwau, teitlau a dyfyniadau).

Hyfforddwyd y model hwn drwy gyfuno data Universal Dependencies Saesneg [XXX](xxxx) gyda data Cymraeg [XXX](xxxx). Rydym yn ddiolchgar i X a Y am roi'r data ar gael o dan drwydded agored.

Defnyddwyd 953 brawddeg hyfforddi Gymraeg o'r CC, a 614 brawddeg brofi Gymraeg fel brawddegau datblygu. Ar gyfer yr elfen Saesneg, defnyddwyd 12,543 brawddeg hyfforddi o'r EWT, a 2007 brawddeg brofi fel brawddegau datblygu.

Y cywirdeb tagio a adroddwyd yn dilyn y broses hyfforddi oedd **92.7%** ar y set brofi.

Oherwydd bod y data profi a'r data hyfforddi wedi'u ffurfio o frawddegau cymysg Cymraeg a Saesneg, nid oes modd inni dorri'r ffigwr hwnnw i lawr fesul iaith. Bwriadwn gynnal gwerthusiad ychwanegol gyda data profi newydd i ddadansoddi hynny. Disgwyliwn y bydd y ffigwr fesul iaith yn is na 92.7% o fewn defnydd cyffredinol, gyda thuedd tuag at y Saesneg gan fod mwy o ddata Saesneg ar gael na data Cymraeg. Serch hynny, mae dadansoddiad cychwynnol o'r canlyniadau hyn yn hynod o addawol, fel y gwelwn yn yr enghreifftiau isod.

## Gosod spaCy

I ddefnyddio'r tagiwr, rhaid yn gyntaf gosod spaCy. Argymhellwn osod fersiwn 2.3.x am y tro gan nad ydym eto wedi dal i fyny gyda'r newidiadau sylweddol i'r llyfrgell yn fersiwn 3 (mae'n fwriad gennym ddiweddaru'r cydrannau Cymraeg a'u trosglwyddo i ddarparwyr spaCy cyn diwedd Mawrth 2023). 

Gallwch ei osod fel a ganlyn:

```
xxx spacy 2.3.2
```

Gweler https://v2.spacy.io/ am fwy o fanylion am spaCy v2.

## Gosod pecyn iaith
Crewyd pecyn iaith dwyieithog penodol ar gyfer y cyfuniad Saesneg a Chymraeg gyda'r cod iaith `ency`. Rhaid dadzipio'r ffeil `ency.zip` a geir yn y storfa hon a gosod y ffolder `ency` o fewn ffolder `lang` spaCy yn eich amgylchedd python. Dyma enghraifft o'i leoliad mewn amgylchedd Python 3.10:

```
.env/lib/python3.10/site-packages/spacy/lang/ency
```

Mae'r ffolder iaith `ency` yn cynnwys map tagiau estynedig sy'n ychwanegu tagiau xpos Cymraeg y corpws UD CCG at dagiau'r corpws Saesneg. Gan fod y tagiau xpos yn wahanol rhwng y ddwy iaith, ond bod y tagiau UPOS yn cyfateb, mae modd dadansoddi i ba iaith mae'r model wedi priodoli pob tocyn o'i xpos, tra'n cynnal cysondeb trawsieithol trwy'r UPOS.

Nid ydym eto wedi cynnwys lemateiddio Cymraeg yn y model, felly dim pnd lemau ar gyfer tocynnau sydd wedi eu tagio gyda thag xpos Saesneg sydd gan y model ar hyn o bryd.

## Gosod y model
Ar ¾ol lawrlwytho a dadsipio ffeil y model, sef XXX, gallwch ei osod y model unrhyw le ar eich system, dim ond eich bod yn gosod y llwybr priodol ato wrth ei lwytho yn eich cod Python:

```
nlp = spacy.load("ency_bi_model")
```

y ffordd hawsaf yw gosod ffolder y model yn yr un cyfeiriadur ¾a'ch ffeil.

```
nlp = spacy.load("ency_bi_model")
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

Noder mai arfer bwriadol CCG yw trin berfenwau fel 'syllu' fel enwau yn htrach na berfenau, hyd yn oed pan f¾ont yn ymddwyn yn fwy berfol. Mae hyn yn un dehongliad dilys o'r berfenw, ond gall fod yn arfer dieithr i rai sydd wedi arfer trin berfenwau berfol fel berfau. 

# Experimental Bilingual Part-of-Speech Tagger for English and Welsh
