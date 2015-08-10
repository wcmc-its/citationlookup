# Citation Lookup Tool

The Citation Lookup Tool (CLT) is a PHP-based interface for converting manually entered citations into structured records from PubMed. 

## Business case

During their annual review cycle, faculty paste in blocks of citations from their CV's. In order to import these records into a VIVO profile, they first need to be validated (some publications have not even published) and, possibly, corrected.

However such submissions contain various features which make automated lookup challenging.
  * misspellings
  * formatting issues like line breaks in the middle of a citation
  * "meta" words (e.g., "In Process") 
  * multiple citations in the same submission


## Demo

  * Go to this website: http://183.82.43.238:8888/ 
  * Select "Upload File." 
  * Upload this file: https://dl.dropboxusercontent.com/u/2014679/CitationLookupToolDemo.xlsx
  * Interface returns back suggestions


## How it currently works

### User upload
User uploads a spreadsheet with the following columns: cwid, name, sequence, title, volume, number, page_end, page_start, pmid, eval_year, publication_full. The only field which is evaluated is publication_full; the cwid and name fields are merely used as a key. (Note that the remaining fields are a legacy of the Faculty Review Tool and are not assessed in any way.)

### Pre-processing
Software pre-processes citations in the following ways:
  * separates out individual citations (in cases where multiple citations are in a single submission)
  * ignores premature line breaks so a given citation may be associated with no more than one citation
  * drops stop words
  * drops numbers from the start of citations (e.g., "54. Cancer and the...")

### Parsing using AnyStyle.io
Parses the citations using the AnyStyle.io web service. For example, this record: 
> A molecular signature of tissues with pacemaker activity in the heart and upper urinary tract involves coexed hyperpolarization-activated cation and T-type Ca2+ channels. Hurtado R, Bub G, Herzlinger D. FASEB J. 2014 Feb;28(2):730-9. doi: 10.1096/fj.13-237289. Epub 2013 Nov 4.

Is converted into this: 
> Author: R, Hurtado and G, Bub and J, Herzlinger D.F.A.S.E.B.

> Title: A molecular signature of tissues with pacemaker activity in the heart and upper urinary tract involves coexed hyperpolarization-activated cation and T-type Ca2+ channels

> Type: incollection

> Language: en

### PubMed Search

Application constructs a query for PubMed which includes first two author names and the five longest words. For example, in the above example, the PubMed search would be: "Hurtado Bub hyperpolarization-activated pacemaker signature molecular."

PubMed returns one of the following for each record:
  * no PMID's
  * one PMID
  * two or more PMID's
