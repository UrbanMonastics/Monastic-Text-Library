# Urban Monastic Source Texts
A Public Repository of all the source texts used in the Monastic Platform.

This library is intended to be an injestable source of texts for use in digital platforms and projects.




## Formating of Sources

### General Source Information
Every text should have an information file seperate from the text itself. This is a seperate file to reduce loading times when simply desiring to skim through availble sources.

The source file shoud include the following fields: 

*	**Abbreviation**: [String|Required] The title or descriptive title in the orgional language. Used to reference the text
*	**Title**: [String|Required] The name of the text. 
*	**Description**: [String|Optional] Describe the text, or provide an introduction to the text.
*	**Type**: [Set|Required] What type of text is it? This tells us what primary file to look for, and how that file will be formated.  
	*	Book, Letter, Dictionary  
*	**SecondaryType**: [String|Optional] A freeform field to allow you to indicate a secondary text type.
*	**Language**: [String|Required] The language being used. Reference the keys in the languages.json file. These are often broken down by region and period of time. For Dictionaries this is the language of the words being defined.
*	**SecondaryLanguage**: [String|Optional] This is the language of the definitions for dictionaries.
*	**Version**: [Set|Required] Indicate what type of a version of the text we have. Many older texts will have origional, translations. Defaults to Origional
	*	Original, or Unique Abbreviation from the Versions file
*	**Date**: [Date|Optional] Date for this verion of the text
*	**Collection**: [String|Optional] Use the Abbreviation from the related Collections file.
*	**Chapters**: [Boolean|Optional] Books and Letters Only. Allows you to indicate if thie source text is broken into chapters the sentense breaks. Defaults to false.
*	**Segments**: [Boolean|Optional] Books and Letters Only. If there are not chapters, you can instead use titled segments to break down a book. One example of this would be the lists of Anitphons and Canticles in the psalter. Defaults to false.
*	**Verses**: [Boolean|Optional] Books and Letters Only. Allows you to indicate if thie source text is broken into verses above the sentense breaks. Defaults to false.
*	**Notes**: [String|Optional] This section is optional, but it allows you to leave any notes or comments about the source that someone looking at it might find useful. One example could be that the book of psalms uses the hebrew/modern numbering and that latin sources have been adjusted to match.
*	**Extra**: [Array|Optional] If you need to add metadata to an item you can place it into this object. You should use a consistant key accross the related items.
*	**Source**:
	*	**DateAccessed**: [Date|Required] When was the source first accessed
	*	**License**: [String|Optional] What type of license is on this content
	*	**Attribution**: [HTML|Optional] Any requested attibution to include with the source.
	*	**Link**: [URL|Optional] Where did you access the source
	*	**LastUpdated**: [Date|Optional] When was the source most recently updated



`source.json`

	{
		"Abbreviation": "",
		"Title": "",
		"Description": "",
		"Type": "",
		"Date": "",
		"Language": "",
		"SecondaryLanguage": "",
		"Version": "",
		"Collection": "",
		"Chapters": false
		"Segments": false
		"Verses": false,
		"Notes": "",
		"Extra": {}
	}


### Collections

Collections are important as they allow us to group together similar content in a desired order. The bible is an example of two collections of texts. These collections simply layout the order of books to make them easier to navigate.

The collections file should include the following fields:  


*	**Abbrevation**: [String|Required] A descriptive short hand for this particular collection
*	**Title**: [String|Required] The name of the text. 
*	**Description**: [String|Optional] Describe the text, or provide an introduction to the text.
*	**Collection**: [String|Optional] Use the Abbreviation from the related Collections file. Only define if this collection is a part of another collection
*	**Order**: [Array|Required] List the texts included in the collection by their abreviations in the order that they should appear.


`Collection.json`

	{
		"Abbreviation": "",
		"Title": "",
		"Description": "",
		"Collection": "",
		"Order": [
			"BOOK-1",
			"BOOK-B",
			"BOOK-the-THIRD"...
		]
	}


### Books & Letters
These can have mulitple sources. This could be because of various languages. When an origional version is available it should be next to the `source.json` in the folder for the book. Additional versions of the book should be included in a folder named with the version abbreviation.

When there are chapters or sections you need to seperate out these into their own files. Filenames should follow:

*	Default: `text.json`
*	Chapters Enabled: `chapter-####.json`	ex: `chapter-0001.json`
*	Segments Enabled: `SEGEMENT-ABREVIATION.json` ex: `gloria.json`


The source file shoud include the following fields: 

*	**SourceAbbreviation**: [String|Required] The title or descriptive title in the orgional language. Used to reference the text it is a part of.
*	**Title**: [String|Optional] If there is a title for this given segment of the text. This can be helpful for chapters or segments of a larger text.
*	**Description**: [String|Optional] Describe this portion of the text, or provide an introduction to the text.
*	**Source**: [String|Optional] Certian documents may benifit from having the ability to indicate the source of a given portion of text (ex: Antiphons, Canticales, etc).
*	**FormatAs**: [Set|Required] What type of text is it? This tells us what primary file to look for, and how that file will be formated.  
	*	Book, Letter, Poetry  
*	**Version**: [Set|Required] Indicate what type of a version of the text we have. Many older texts will have origional, translations. Defaults to Origional
	*	Original, or Unique Abbreviation from the Versions file
*	**Text**: [String|Optional] If there are not verses for this text, then the text itself should go here. Verses are defined in the source document.
*	**Verses**: [Array|Optional] If this text is broken down into verses those verses should be individual lines inside of this key.
*	**Extras**:	[Array|Optional] This is a Key-Pair list of additional related content.


Each file should follow the following strucutre.

`chapter-0001.json` with verses

	{
		"SourceAbbreviation": "bible-ot-psalms",
		"Title": "",
		"FormatAs": "poetry",
		"Version": "GRAIL"
	}




### Dictionaries
These texts should be formated into JSON files where each top level is the infinitive. Underneith should be the simplified form, part of speech [verb, noun, pronoun, article, conjunction, adjective, adverb], definition, and a meta section including additional field information (like Strongs Numbering, etc).

Each dictionary should be placed inside of a folder for the primary language, and then a child folder for the specific dictionary. 

The collections file should include the following fields for each word:  

*	**Simplified**: [String|Optional] This is a simplified form of the word. Often in latin type when the origional word is not latin based.
*	**PartOfSpeech**: [Set|Required] What part of speech is this word.
	*	Noun, Pronoun, Propper Noun, Adjective, Verb, Adverb, Preposition, Conjuction, Interjection, or  Article
*	**Definition**: [String|Required] The definition of the word.
*	**SameAs**: [String|Optional] If the word has the same definition as a different word in the dictionary, you can use this to point to that definition. When used the definition filled in here will be ignored.
*	**Meta**: [Object|Optional] Include additional information about the word here. This could be things like Strongs numbers.


`dictionary.json`

	{
		"λόγος": {
		"Simplified":"logos",
		"PartOfSpeech":"noun",
		"Definition":"a word, a thing uttered, Mt. 12:32, 37; 1 Cor. 14:19; speech, language, talk, Mt. 22:15; Lk. 20:20; 2 Cor. 10:10; Jas. 3:2; converse, Lk. 24:17; mere talk, wordy show, 1 Cor. 4:19, 20; Col. 2:23; 1 Jn. 3:18; language, mode of discourse, style of speaking, Mt. 5:37; 1 Cor. 1:17; 1 Thess. 2:5; a saying, a speech, Mk. 7:29; Eph. 4:29; an expression, form of words, formula, Mt. 26:44; Rom. 13:9; Gal. 5:14; a saying, a thing propounded in discourse, Mt. 7:24; 19:11; Jn. 4:37; 6:60; 1 Tim. 1:15; a message, announcement, 2 Cor. 5:19; a prophetic announcement, Jn. 12:38; an account, statement, 1 Pet. 3:15; a story, report, Mt. 28:15; Jn. 4:39; 21:23; 2 Thess. 2:2; a written narrative, a treatise, Acts 1:1; a set discourse, Acts 20:7; doctrine, Jn. 8:31, 37; 2 Tim. 2:17; subject-matter, Acts 15:6; reckoning, account, Mt. 12:36; 18:23; 25:19; Lk. 16:2; Acts 19:40; 20:24; Rom. 9:28; Phil. 4:15, 17; Heb. 4:13; a plea, Mt. 5:32; Acts 19:38; a motive, Acts 10:29; reason, Acts 18:14; ὁ λόγος, the word of God, especially in the Gospel, Mt. 13:21, 22; Mk. 16:20; Lk. 1:2; Acts 6:4; ὁ λόγος, the divine WORD, or Logos, Jn. 1:1 </def>→ message; report; word.",
		"Meta":{
			"Strongs": 3056,
			"GK": 3364,
			"Instances": 330
			}
		},
		"NextWord"...
	}


### Versions
Define versions here.

The versions file should include the following fields for each entry. The entry should be referenced by its version abbreviation.

*	**Abbreviation**: [String|Required] The title or descriptive title in the orgional language. Used to reference and group the version together. If this is a follow on version you should include the year of this current version.
*	**Title**: [String|Required] The name of the text. 
*	**Description**: [String|Optional] Describe the text, or provide an introduction to the text.
*	**Language**: [String|Required] The language being used. Reference the keys in the languages.json file. These are often broken down by region and period of time. For Dictionaries this is the language of the words being defined.
*	**Date**: [Date|Optional] Date for this verion of the text


`versions.json`

	{
		"UMT-EN":{
			"Abbreviation":"UMT-EN",
			"Title":"Urban Monastic English Translation",
			"Description":"The english translation used by Urban Monasticism",
			"Language": "eng",
			"Date": "2020"
		},
		"TMU-FR":{
			"Abbreviation":"TMU-FR",
			"Title":"Traduction Monastique Urbain Français",
			"Description":"La traduction française utilisée par Urban Monasticism..",
			"Language": "fra-sta",
			"Date": "2020"
		},
		"OTHER VERSIONS KEEP GOING":{}...	
	}


#### Adding a Version of a Text



### Languages
When possible we should be using the aberviations/codes from the Linguiusts List. You can look up languages here from the [Multitree page](http://multitree.org/codes/). There is only one language file structured on the high level by langague, then nested arrays with keyed by their code from Linguiusts List.

The array has 2 values and ***no keys***: 

*	*title*: This is the name of the language in english
*	*Date Range*: As a second array with 2 objects the start and end dates. If the second date is excluded the language is in modern usage. (like 1900-present)

	"Greek": {
		"grc-koi": [
			"Koine Greek",
			[-300, 300]
		],
		"": []
	}




## Formating Notes:

We use Markdown formating for text, and all linebreaks and tabs should be correctly encoded. Ideally these will be managed for you by the eventual editor put in place.

*	**Newline** is replaced with \n
*	**Carriage Return** is replaced with \r
*	**Tab** is replaced with \t
*	**Double Quote** is replaced with \"
*	**Backslash** is replaced with \\
*	**Backspace** is replaced with \b 
*	**Form Reed** is replaced with \f
