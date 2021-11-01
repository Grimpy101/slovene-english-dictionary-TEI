# Slovene-English Dictionary

This is an attempt at a Slovene-English dictionary, intended for FreeDict project and other similar uses.

**NOTE: This is still heavily in development. I have yet to create a piece of code to convert current XML files to TEI dictionary.**

## Project structure

The project's main content files are stored inside ``xml`` folder. Inside, there are multiple files, each representing one section of the dictionary - for example, in ``slv_eng-a.xml`` there are all entries that start with the letter "a", etc. The files are written in TEI format, but are of the type XML for ease of use and editing.

## XML file structure

The text below is more of a "crash course" and not really that detailed or accurate. If one wishes to know much more about the way TEI files are structured, I suggest this documentation: <https://dariah-eric.github.io/lexicalresources/pages/TEILex0/TEILex0.html#collocates>. Alternatively, FreeDict project has a Wiki that explains a few things as well: <https://github.com/freedict/fd-dictionaries/wiki>.

NOTE: The values and types I mention below are my own restriction. The list can extend and change.

Each XML file contains entries, which represent information on words and phrases. This is not really a TEI structure yet, since it's missing some information, but this would be added later programmatically.

In XML and similar languages for storing information, data is wrapped in tags. These can also have attributes that define additional information. Tags can contain plain-text data and/or other tags, which have their own data.

Below is an example of a TEI dictionary entry as used in this project:

```xml
<entry xml:id="a">
    <form type="lemma">
        <orth>a</orth>
    </form>
    <gramGrp>
        <gram type="pos">conj.</gram>
    </gramGrp>
    <sense xml:id="a.1">
        <usg type="dom">Lit.</usg>
        <cit type="trans">
            <quote>but</quote>
            <quote>however</quote>
        </cit>
        <cit type="example">
            <quote>Iščejo dom, a ga ne najdejo.</quote>
            <cit type="trans">
                <quote>They are searching for home but cannot find it.</quote>
            </cit>
        </cit>
    </sense>
</entry>
```
### Entry
The ``entry`` tag marks a new entry in the dictionary. It usually only has the ``xml:id`` attribute which acts as a unique ID for the entry. It is usually just the word or phrase in the entry.

If there are two entries with the same word, the IDs should be written with a ``.x`` suffix, where x represents an integer. For example, there are two entries with the name ``atika``, so we add a suffix, and the resulting IDs of the entries are ``atika.1`` and ``atika.2``.

There are some suggested conventions to follow when writing IDs:
 - always use lowercase letters,
 - replace any spaces with underscores (example: ``abonirati se`` -> ``abonirati_se``),
 - replace non-English letters with their ASCII representations where possible (example: ``užaloščen`` -> ``uzaloscen``).

Entry usually contains ``form``s, ``gramGrp``s, and ``sense``s.

### Form

The ``form`` tag contains much of the information on the original word or phrase. It has the attribute ``type`` which provides information on what kind of information form contains. More in the table below...

Form can contain ``orth`` tag which holds the actual word/phrase, as well as ``gramGrp`` group for any additional grammatical properties which may hold true only for this particular form.

A table of some **type** values:

| Value      	| Meaning                                            	|
|------------	|----------------------------------------------------	|
| lemma      	| The headword - main word that represents the entry 	|
| inflected  	| Word in other than usual dictionary form           	|
| variant    	| A variant form                                     	|
| simple     	| A single free lexical item                         	|
| compound   	| Word formed from simple lexical items              	|
| derivative 	| Word derived from headword                         	|
| phrase     	| Multiple-word lexical item                         	|
| paradigm   	| A collection of inflected forms                    	|

### Grammatical Group

The ``gramGrp`` tag groups together grammatical properties that define the word/phrase in question. The tag can be found directly in the ``body`` (see example above), in which case it holds true for all possible forms in the entry, or it can reside in any ``form`` tag, in which case it applies only to this particular form.

A ``gramGrp`` group contains a bunch of ``gram`` tags. Each ``gram`` tag is given a ``type`` attribute to specify what kind of grammatical property it holds. Below is a table of some of these types and values.

| Type 	| About 	| Values 	|
|---	|---	|---	|
| pos 	| Defines the type of word (noun, verb...) 	| n. (noun)<br>v. (verb)<br>adj. (adjective)<br>conj. (conjugate)<br>adv. (adverb)<br>int. (interjection)<br>prep. (preposition)<br>pron. (pronoun)<br>art. (article)<br>num. (numeral)<br>pref. (prefix) 	|
| case 	| Defines the case of the word 	| nom. (nominative)<br>gen. (genitive)<br>dat. (dative)<br>acc. (accusative)<br>loc. (locative)<br>instr. (instrumental) 	|
| gender 	| Defines the gender of the word 	| m. (male)<br>f. (female)<br>n. (neutral) 	|
| mood 	| Defines the mood of the verb 	| indic. (indicative)<br>imper. (imperative)<br>condit. (conditional) 	|
| number 	| Defines the number of the word 	| sg. (singular)<br>pl. (plural)<br>du. (dual) 	|
| per 	| Defines the person of the verb 	| 1st<br>2nd<br>3rd 	|
| tns 	| Tense 	| Present<br>Future<br>Past 	|
| colloc 	| A collocate - any sequence of words that co-occur with the headword with significant frequency 	| example: [+ conj.] 	|

### Sense

Sense contains information on the English counterpart to the Slovene word/phrase. It has its own ID, which is almost the same as the ``entry`` ID but with added ``.x`` at the end (where x is an integer).

There can be multiple ``sense``s in an entry if the word/phrase has many meanings.

A list of tags that can be found in a ``sense``:

| Tag 	| Description 	|
|---	|---	|
| usg 	| Defines a type of usage - for example, where is the word used, what kind of situation it is used in, etc. 	|
| cit 	| It can contain actual translation or example of usage (all of these are stored in <quote></quote> 	|
| quote 	| Holds data 	|
| def 	| Holds any definitions of words - can be used for extra explanation of the word or when there is no proper translation	|

Types of **usage**:

| Type     	| Description      	| Values                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 	|
|----------	|------------------	|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| dom      	| Domain           	| Adm. (administration)<br>Aero. (aeronautics)<br>Agr. (agriculture)<br>Anat. (anatomy)<br>Antr. (antropology)<br>Arch. (architecture)<br>Archae. (archaeology)<br>Art<br>Astr. (astronomy)<br>Bibl. (bibliography)<br>Biol. (biology)<br>Bot. (botany)<br>Buil. (building trade)<br>Chem. (chemistry)<br>Chess<br>Comp. (computation)<br>Craft. (craftsmanship)<br>Econ. (economy)<br>Engin. (engineering)<br>Film<br>Fin. (finances)<br>For. (forestry)<br>Gast. (gastronomy)<br>Geol. (geology)<br>Geog. (geography)<br>Hist. (history)<br>Hunt. (hunting)<br>Law<br>Lit. (literature)<br>Ling. (linguistics)<br>Math. (mathematics)<br>Med. (medicine)<br>Meteo. (meteorology)<br>Milit. (military)<br>Mus. (music)<br>Myth. (mythology)<br>Naut. (nautic)<br>Pedag. (pedagogics)<br>Pharm. (pharmacy)<br>Phil. (philosophy)<br>Phys. (physics)<br>Psych. (psychiatry)<br>Rail. (rail transport)<br>Rel. (religion)<br>Sci. (science)<br>Sport<br>Tech. (technic)<br>Text. (textile)<br>Theat. (theatre)<br>Vet. (veterinary)<br>War<br>Zoo. (zoology) 	|
| plev     	| Preference level 	| rare<br>occas. (occasional)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            	|
| geo      	| Geographic data  	| dial. (dialect)<br>Inner Carniola (Notranjska)<br>Upper Carniola (Gorenjska)<br>Lower Carniola (Dolenjska)<br>Littoral Region (Primorje)<br>Styria (Štajerska)<br>Prekmurje<br>Carinthia (Koroška)              <br>White Carniola (Bela krajina)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       	|
| time     	| Usage by time    	| archaic<br>old                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         	|
| register 	|                  	| child. (childlike)<br>slang<br>lingo<br>vulgar<br>formal<br>casual<br>affect. (affectionate)<br>colloq. (colloquial)<br>pejor. (pejorative)<br>iron. (ironicaly)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       	|
| style    	|                  	| fig. (figurative)<br>lit. (literal)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    	|

## Some more examples of entries

```xml
<entry xml:id="ah">
    <form type="lemma">
        <orth>ah</orth>
    </form>
    <gramGrp>
        <gram type="pos">int.</gram>
    </gramGrp>
    <sense xml:id="ah.1">
        <cit type="trans">
            <quote>ah</quote>
            <quote>oh</quote>
        </cit>
        <cit type="example">
            <quote>Ah, seveda!</quote>
            <cit type="trans">
                <quote>Oh, right!</quote>
            </cit>
        </cit>
        <def>Expresses awe, contentment, or when getting an idea or thought.</def>
    </sense>
    <sense xml:id="ah.2">
        <cit type="trans">
            <quote>ah</quote>
            <quote>oh</quote>
        </cit>
        <cit type="example">
            <quote>Ah, ti si.</quote>
            <cit type="trans">
                <quote>Oh, it's you.</quote>
            </cit>
        </cit>
        <def>Expresses regret, tiredness.</def>
    </sense>
</entry>
```
```xml
<entry xml:id="aktuar">
    <form type="lemma">
        <orth>aktuar</orth>
    </form>
    <form type="variant">
        <orth>aktuarka</orth>
        <gramGrp>
            <gram type="gender">f.</gram>
        </gramGrp>
    </form>
    <gramGrp>
        <gram type="pos">n.</gram>
        <gram type="gender">m.</gram>
        <gram type="number">sg.</gram>
    </gramGrp>
    <sense xml:id="aktuar.1">
        <cit type="trans">
            <quote>actuary</quote>
        </cit>
    </sense>
</entry>
```

```xml
<entry xml:id="amortizirati_se">
    <form type="lemma">
        <orth>amortizirati se</orth>
    </form>
    <gramGrp>
        <gram type="pos">v.</gram>
    </gramGrp>
    <sense xml:id="amortizirati_se.1">
        <usg type="dom">Econ.</usg>
        <cit type="trans">
            <quote>to be depreciated</quote>
        </cit>
        <cit type="example">
            <quote>Avto se amortizira v petih letih.</quote>
            <cit type="trans">
                <quote>The car is depreciated in five years.</quote>
            </cit>
        </cit>
    </sense>
</entry>
```
