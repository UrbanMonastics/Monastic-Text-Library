# Urban Monastic Source Texts
A Public Repository of all the source texts used by [Urban Monasticism](https://urbanmonastic.org) in their Monastic Platform, and other resources.


## How Source Text Libraries work

This is a [Source Text library](https://github.com/UrbanMonastics/Source-Text-Library). You can learn how they work by looking at that documentation. In essence a library allows one to store complex related files without the need for databases in a format that is easily portable.


## Annotating the Texts

### Extra keys sometimes employed 

-  **Reference**: This is an array of scripture passages referenced by the text. This is most often used with antiphons, and prayers.
-  **BreviaryInstructions**: When specific instructions are associated with a given text that would need to be displayed in a proper, or printed version of the breviary they should go here. This needs to be in the language of the text.
-  **LiturgiaeHorarum-Index**: The _Thesaurus Liturgiae Horarum Monasticae_ references canticles by an index by out of either the old or new testament. This is that index value. (Numeric)
-  **LiturgiaeHorarum-Cantica**: The _Thesaurus Liturgiae Horarum Monasticae_ references canticles by an index by out of either the old or new testament. This is either **VT** (Old Testament) or **NT** (New Testament).


## Formatting of the Texts

`source.json`

	{
		"Abbreviation": "",
		"Title": {
			"default": "",
			"UMB-EN": "",
			"fra-sta": ""
		},
		"AlternativeTitles":{
			"eng": ["", "", ""],
			"fra-sta": ["", "", ""]
		},
		"Description": "",
		"Type": "",
		"Date": "",
		"Language": "",
		"SecondaryLanguage": "",
		"Version": "",
		"Collection": [""],
		"Chapters": false,
		"SegmentTitles": false,
		"Segments": false,
		"Verses": false,
		"Notes": "",
		"Extra": {}
	}



### Text Files - `text.json`
These can have multiple sources. This could be because of various languages. When an original version is available it should be next to the `source.json` in the folder for the book. Additional versions of the book should be included in a folder named with the version abbreviation. For the rare instances when more than one version of a text is provided (like with First Chronicles in the Codex Sinaiticus) in the same version you should suffix the abbreviation with `-v#.json`.

The type of segments a source is broken into determines its filename. When there are chapters or sections you need to separate out these into their own files. Filenames should follow:  
  
*	**None**: `text.json`  
*	**Chapters**: `chapter-####.json`	ex: `chapter-0001.json`  
	*	Multiple versions: `chapter-0001.json` and `chapter-0001-v2.json`  
*	**Abbreviations**: `SEGEMENT-ABREVIATION.json` ex: `gloria.json`  
  
  
The source file should include the following fields:  
  
*	**TextAbbreviation**: [String|Required] This should match the filename without the file extension.
*	**SourceAbbreviation**: [String|Required] The Source for which this file belongs.
*	**Title**: [String|Optional] If there is a title for this given segment of the text. This can be helpful for chapters or segments of a larger text.
*	**Chapter**: [String|Optional] If this source is broken into chapters you should include the chapter number here.
*	**Description**: [String|Optional] Describe this portion of the text, or provide an introduction to the text.
*	**Source**: [String|Optional] Certain documents may benefit from having the ability to indicate the source of a given portion of text (ex: Antiphons, Canticles, etc).
*	**FormatAs**: [Set|Required] What type of text is it? This tells us what primary file to look for, and how that file will be formatted.  
	*	Book, Letter, Poetry  
*	**Version**: [Set|Required] Indicate what type of a version of the text we have. Many older texts will have original, translations. Defaults to Original  
	*	Original, or Unique Abbreviation from the Versions file
*	**License**: [String MD|Optional] The License or Copyright that should accompany the display of this text. This ensures we keep the license with the texts, but we should use the license from the version to display.
*	**Translation**: [Array|Optional] Include this element when the text/verses are a translation. This should allow us to identify what the source version is, and who was involved in the translation process.
	*	**Versions**: [Set|Required] The list of the versions which are being use to generate the translation. 
	*	**People**: [Set|Required] The list of individuals involved in the translation. Ideally ordered by scope of their contribution to the work included in this file.  
*	**Text**: [String|Optional] If there are not verses for this text, then the text itself should go here. Verses are defined in the source document.  
*	**Verses**: [Array|Optional] If this text is broken down into verses those verses should be individual lines inside of this key. When using verses you must include all formatting (line breaks, indentations, etc). Indentations should lead verses, and line breaks end verses.
*	**Extras**:	[Array|Optional] This is a Key-Pair list of additional related content.  
*	**InlineTitles**:	[Array|Optional] A list of inline titles to include in the text. The array key should be the location of the title. For Text files it should be the word position as a number For verses it can be either a whole or decimal number with whole numbers representing the verses, and the number after the decimal representing the word in that verse. The Title would be placed immediately before the referenced word. For this case counting starts at 1.
*	**Notes**: [String|Optional] This section is optional, but it allows you to leave any notes or comments about the source that someone looking at it might find useful. This could be a simple as the date a text was imported.  
*	**Changes**: [Array|Optional] A list of what changes were made, when, and by whom. Each line should be formatted as `YYYY-DD-MM - NAME OF PERSON: Note about what changed`.  
*	**Footnotes**: [Array|Optional] This allows us to place notes inline if we choose to render them. They should be keyed in a way that allows us to position them correctly. This should be by the word position in the `Text` or within a specific `Verse`. These may be displayed as footnotes or - when digitally allowed - with visual indicators to hover over the word/s to reveal the note/s. Footnotes are placed *after* the positioned word.  
  
  
Each file should follow the following structure.  
  
`chapter-0119.json` with verses

	{
		"SourceAbbreviation": "bible-ot-psalms",
		"Title": "",
		"Chapter": "119",
		"FormatAs": "poetry",
		"Version": "UMT-EN",
		"Translation":{
			"Versions": ["CODEX-SINAITICUS", "LH"],
			"People": ["Paul Prins"]
		},
		"Verses": {
			"1": "Blessed are they whose ways are blameless,\r\n\twho walk according to the law of the LORD.\r\n",
			"2": "Blessed are they who keep his statutes\r\n\tand seek him with all their heart.\r\n",
			"3": "They do nothing wrong;\r\n\tthey walk in his ways.\r\n",
			"4": "You have laid down precepts that are to be fully obeyed.\r\n"
		},
		"InlineTitles": {
			"3": "Nothing Done Wrong"
		},
		"Changes": [
			"2021-03-04 - Paul Prins: Added support in the documentation for changes.",
			"2021-02-25 - Paul Prins: Thought about adding changes, but decided future Paul gets to do that."
		],
		"FootNotes": {
			"1": ["This is a note which would appear at the end of the verse."],
			"1.1": ["This will appear after the first word of the first verse."]
			"1.6": [
				"This is a note which appears with the word Blameless",
				"Along with a second note when it makes sense to do so"
			],
			"3.5-9": ["This would be a note for the last 5 words of verse three"],
			"3-4": ["They can also span multiple verses within the scope of the same file. So not across chapters where it would need to be added to both files."]
		},
		"License": "© YYYY by [Organization](http://website.org/)."
	}



### Collections

Collections are important as they allow us to group together similar content in a desired order. The bible is an example of two collections of texts. These collections simply layout the order of books to make them easier to navigate.

The collections file should include the following fields:  


*	**Abbreviation**: [String|Required] A descriptive short hand for this particular collection
*	**Title**: [Array|Required] The name of the text. This should follow the same pattern as the title on the Sources supporting: default, version abbreviations, and languages. 
*	**Description**: [String|Optional] Describe the text, or provide an introduction to the text.
*	**Collection**: [Array|Optional] Use the Abbreviation from the related Collections file. Only define if this collection is a smaller part of another collection
*	**Order**: [Array|Required] List the texts included in the collection by their abbreviations in the order that they should appear.


`Collection.json`

	{
		"Abbreviation": "",
		"Title": {
			"default": "",
			"UMB-EN": "",
			"fra-sta": ""
		},
		"Description": "",
		"Collection": [""],
		"Order": [
			"BOOK-1",
			"BOOK-B",
			"BOOK-the-THIRD"...
		]
	}



### Dictionaries
These texts should be formatted into JSON files where each top level is the infinitive. Underneath should be the simplified form, part of speech [verb, noun, pronoun, article, conjunction, adjective, adverb], definition, and a meta section including additional field information (like Strongs Numbering, etc).

Each dictionary should be placed inside of a folder for the primary language, and then a child folder for the specific dictionary. 

The collections file should include the following fields for each word:  

*	**Simplified**: [String|Optional] This is a simplified form of the word. Often in latin type when the original word is not latin based.
*	**PartOfSpeech**: [Set|Required] What part of speech is this word.
	*	Noun, Pronoun, Proper Noun, Adjective, Verb, Adverb, Preposition, Conjunction, Interjection, or  Article
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

*	**Abbreviation**: [String|Required] The title or descriptive title in the original language. Used to reference and group the version together. If this is a follow on version you should include the year of this current version.
*	**Title**: [String|Required] The name of the text. 
*	**Description**: [String|Optional] Describe the text, or provide an introduction to the text.
*	**License**: [String MD|Optional] The License or Copyright that should accompany the display of this text.
*	**PublicDomain**: [Bool|Optional] If the version in question is known to be in the public domain you can indicate that here.
*	**Permission**: [String|Optional] Explain the current state of permission for this version in this library.
*	**Language**: [String|Required] The language being used. Reference the keys in the languages.json file. These are often broken down by region and period of time. For Dictionaries this is the language of the words being defined.
*	**Date**: [Date|Optional] Date for this version of the text
*	**Options**: [Object|Optional] This allows us to store if certain rendering should be preformed on the text of this version  
	*	**Selah**: [Bool|Optional] Does this text include the term Selah, and should it be formatted in a specific way.  
	*	**SmallCaps**: [Bool|Optional] Does the text have words included in all caps which should be transformed into small caps. 
*	**Notes**: [String MD|Optional] This section is optional, but it allows you to leave any notes or comments about the source that someone looking at it might find useful. One example could be that the book of psalms uses the Hebrew/modern numbering and that latin sources have been adjusted to match.


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
When possible we should be using the abbreviations/codes from the Linguists List. You can look up languages here from the [Multitree page](http://multitree.org/codes/). There is only one language file structured on the high level by language, then nested arrays with keyed by their code from Linguists List.

The array has 2 values and ***no keys***: 

*	*title*: This is the name of the language in english
*	*Date Range*: As a second array with 2 objects the start and end dates. A null value should be supplied to indicate an open ended nature of a date set [null, 200] for leading up to the year 200, or [1900, null] for since 1900.

	"greek": {
		"grc-koi": [
			"Koine Greek",
			[-300, 300]
		],
		"": []
	}


### Picking an Abbreviation for the Breviary
Many of the texts for our Breviary are filtered by abbreviations. To make this easier during the development phase we should give them abbreviations that follow this pattern: _season_-_week_-_day of week_-_office_-_part of office_  

When there are texts which are used in multiple offices, or in different parts of an office you should exclude that section from the abbreviation. For abreviations which come out of the 4 week schedule simply exclude the 'season' so that it starts with the week instead (or in the case of compline you would also exclude the week since there is only one week). For the Breviary prayers you should preface the abbreviation with either 'intercessions' or 'concluding' so that they are more easily grouped in the list of files.


## Formatting Notes:
This section should be kept up to date with changes made in the Text Formatting libraries.

We use Markdown formatting for text, and all line breaks and tabs should be correctly encoded. Ideally these will be managed for you by the eventual editor put in place.

*	**Carriage Return** is replaced with \r
*	**Newline** is replaced with \n
*	**Tab** is replaced with \t
*	**Double Quote** is replaced with \"
*	**Backslash** is replaced with \\
*	**Backspace** is not supported
*	**Form Reed** is not supported


As a best practice we use `\r\n` to indicate a single new line return. For multiple lines you simply add another set as needed (example: `\r\n\r\n`).

When using verses you should - as possible - place line breaks at the end of a verse, and tabs at the beginning of a verse.

### Styling the Text

There are different styling needs of the text which we need to support. The base of it is markdown with some specialized support added. Below are the supported formatting syntaxes with examples. For encased text which spans between different text enclosures you need to close and then re-open the tag with the new text enclosure.

*	\*\*bold text\*\*	Make the encased text **Bold**  
*	\*italic text\*		Make the encased text *Italic*  
*	\*\*\*bold and italic\*\*\*		Make the encased text ***bold and italic***  
*	\~strike through\~		Make the encased text ~struck through~  
*	\_underline\_		Make the encased text underlined (not supported by Github).  
*	‾over line‾		Make the encased text over-lined. Only supported in HTML formatting. (not supported by Github).  
*	\_‾under and over line‾\_		Make the encased text both underlined and over-lined. Only supported in HTML formatting, in other outputs will show as underlined. (not supported by Github).  
*	[red]red text[/red]		Make the encased text red.  
*	\1.		Ordered lists should list every item with `1.` to ensure that the numbering is always correct. It will be transformed into an ordered lists and displayed correctly to users counting from 1.  
*	\*		Unordered lists should list every item with a `*`.  
*	SMALLCAPS	When enabled by the version file, this text will be rendered in with small caps, or will be transformed into a simple capitalized string otherwise. Used for rendering of Lord in reference to YHWH, but it may have wider application.

#### Styling for Breviary and Liturgies
There are some unique markings to facilitate liturgical texts. These will allow us to parse and style these portions of the 

*	[+]		This will insert the symbol to prompt the reader to cross themselves. Rendered as ✛ in non HTML [U+271B or `&#10011;`].
*	[*]		This is the for denoting a mid-point in chanted texts.  
*	[t]		This is the dagger/obelisk that indicates the current line continues below. Helpful with chanted texts with more than two lines. Rendered as † in non HTML [U+2020 or `&#8224;` or `&dagger;`].  
*	[V]		During the *Responsory* it denotes a **Versicle** line with the leader speaking. Rendered as ℣ in non HTML [U+2123 or `&#8483;`].  
*	[R]		During the *Responsory* it denotes **Response** line with all speaking. Rendered as ℟ in non HTML [U+211F or `&#8479;`].  
*	[II]	During the *Intercessions* this indicates the **Introduction** to the intentions. When prayed in a group it should be read only by the leader.  
*	[IR]	During the *Intercessions* this indicated the **Response**. It should only be placed in the source text on the line after the introduction. It will be placed in other locations when formatted. When prayed in a group it should be read only by the leader.  
*	[I1]	During the *Intercessions* this indicates the **first** part of an **intention**. 
*	[I2]	During the *Intercessions* this indicates the **second** part of an **intention**. 
*	`selah`	This term appears in the book of psalms and it is unclear what it means. It is often just placed directly into the text while right aligned. 
*	[VERSE #]	This will insert the verse numbering if/when it is enabled.

[V], [R], [II], [IR], [I1], and [I2] must appear at the beginning of a line without any other styling, dashes, or indentations. Those styling elements will be processed when the text is formatted for it's desired output.
