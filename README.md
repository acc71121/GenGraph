# GenGraph

1) TR = table of relations --> retrievable from: https://osf.io/db2w8/
2) TL= table of lexemes  --> retrievable from: https://osf.io/db2w8/

=========================================

          SEQUENCE OF STEPS

========================================= 

    INITIAL_TABLES_PREPARATION.ipynb
  
1) In the TL removes white spaces

========================================= 

A) PREPARE THE DATASETS - A.ipynb

 1) In the TL, keep only the entries for which the para_phon column is not empty
If the entry is a verb, check that the stem_space column is not empty
Call RTL (restricted table of lexemes) the resulting sub-table of lexeme 

In the stem_space column (for verbs), replace the list of values with an attr:val list, where val is one of the stem-space values, and val is preceded by att, the attribute of equal rank in the following list of attributes (in order):

Vmii---    	(Multext value for Ind.Impft)
Vmip3p-		(Multext value for Ind.prs.pl.3)
Vmip-s-		(Multext value for Ind.prs.sg)
Vmpp---		(Multext value for Part.prs)
Vmmp-s-		(Multext value for Imp.prs.sg)
Vmmp-p-		(Multext value for Imp.prs.pl)
Vmsp-s-		(Multext value for Subj.prs.sg)
Vmsp-p-		(Multext value for Subj.prs.pl.12)
Vmn----		(Multext value for inf)
Vmif---		(Multext value for Ind.fut)
Vmis---		(Multext value for Ind.pst)
Vmps---		(Multext value for Part.pst)

For example: 

(cf last page, files https://www.demonext.xyz/wp-content/uploads/2023/02/Descriptif_Attributs_Valeurs_TableLexemes-en.pdf and https://www.demonext.xyz/wp-content/uploads/2023/02/Descriptif_Attributs_Valeurs_TableLexemes-fr.pdf
)

the stem-space of BOIRE (`drink'):

byv	bwav	bwa	byv	bwa	byv	bwav	byv	bwa	bwa	by	by

becomes:

Vmii---:byv	Vmip3p-:bwav	Vmip-s-:bwa	Vmpp---:byv	Vmmp-s-:bwa	Vmmp-p-:byv	Vmsp-s-:bwav	Vmsp-p-:byv	Vmn----:bwa	Vmif---:bwa	Vmis---:by	Vmps---:by


2) In RT, keep only the relations between lexemes present in RTL. Call RTR (restricted table of relations) the resulting sub-table of relations 

Below, I use "Word1" and "Word2" to name the lexemes that make up the relation (Word1, Word2) in the RTR.

===========================================

 SCRIPT_AFFIXES_EXTRACTION.ipynb

  3) Extract the construction patterns (cstr_1 / cstr_2) present in RTR. Arrange them in an array you will call TCP (Table of construction patterns)


  4) Identify (manually or automatically) the phonetic transcription of the affix occuring in each  construction pattern of TCP. Encode the value found in a new  column of TCP.
The encoding of the phonemes must follow the same transcription system as the one used in the lexeme table.

For example, we could have: 


cstr	phon_affix
Xeur	œʁ

========================================= 

B) PHONETIC ENCODING OF THE AFFIXES  - B.ipynb


5) 
(a) In RTR add two columns: one is called phon_aff_1, and encodes the phonetic transcription of the affix on Word1. The other one is called phon_aff_2, and encodes the phonetic transcription of the affix  on Word2.

-  In each row of RTR, use cstr_1 and the TCP table to code the phonetic transcription of the affix corresponding to cstr_1 in the corresponding phon_aff_1 column. If cstr_1=X, the value is "None". 


- In each row of RTR, use cstr_2 and the TCP table to code the phonetic transcription of the affix corresponding to cstr_2 in the corresponding phon_aff_2 column. If cstr_2=X, the value is "None". 

(b) In RTL add one column: para_desyll, which contains the phonetic transcription of  Word's paradigm where the syllable separator "." is removed.
- In each line of RTL, 
	• if Word is a verb, simply copy its stem-space value in para_desyll
	• otherwise,  remove the syllable separators "." everywhere in the value of the para-phon cell, and copy the result in para_desyll 

=============================================

C)PHONETIC TRANSRIPTION OF WORDi (REMOVE THE AFFIX (IF ANY))  - C.ipynb

6) identify the phonetic transcription of the stem(s) of Word1 and Word2 : 
 

-  if Wordi is a verb: 
• if the value of phon_aff_i = "None", the verb stems = its stem-space (ie the content of para_desyll)
• otherwise: remove then the affix from each cell of its stem space 

- otherwise:
•  if the value of phon_aff_i = "None", the word stems = its phonetically transcribed and unsyllabled paradigm (ie the content of para_desyll)
• otherwise : remove the affix from each cell of its phonetic paradigm 

- encode the values obtained in two new column of RTR called phon_stem_1 (for Word1's phonetical stems), and phon_stem_2 (for Word2's phonetical stems),
==============================================

D) STEM VARIATION BTW WORD1/WORD2 - D.ipynb

7) compare phon_X_1 and phon_X_2


