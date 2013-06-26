FALDO
=====

FALDO is the Feature Annotation Location Description Ontology.
It is a simple ontology to describe sequence feature positions and regions as found in 
[GFF3](http://www.sequenceontology.org/gff3.shtml), [DBBJ](http://www.ddbj.nig.ac.jp),
[EMBL](http://www.embl.org), [GenBank](http://www.ncbi.nlm.nih.gov/genbank) files,
[UniProt](http://www.uniprot.org), and many other bioinformatics resources.

The aim of this ontology is to describe the position of a sequence region or a feature.
It does not aim to describe features or regions itself, but instead depends on resources
such as the Sequence Ontology or the UniProt core ontolgy.

Examples
--------

### Known positions

A genomic region where we know exactly where it starts and ends on the reference genome sequence:

```turtle
<_:1> a :Region ;
           :begin <_:1b> ;
           :end <_:1e> .

<_:1b> a :Position ; 
           a :ExactlyKnownPosition ;
           a :ForwardStrandPosition ;
            :position "1"^^xsd:int ;
            :reference ddbj:XXXDSDS .

<_1e> a :Position ; 
           a :FuzzyPosition ;
           a :ForwardStrandPosition ;
           :begin <_:1ea> ;
           :end <_:1eb> ;
           :reference ddbj:XXXDSDS .

<_:1ea> a :Position ;
        a :ExactlyKnownPosition ;
        a :ForwardStrandPosition ;
           :position "3"^^xsd:int ;
           :reference ddbj:XXXDSDS .

<_:1eb> a :Position ;
        a :ExactlyKnownPosition ;
        a :ForwardStrandPosition ;
           :position "7"^^xsd:int ;
           :reference ddbj:XXXDSDS .
```

A genomic region where the begin is on one contig and the end on an other:

```turtle
<_:2> a :Region
           :begin <_:2b> ;
           :end <_:2e> .
<_:2b> a :Position ; 
            a :ExactlyKnownPosition ;
            :position "1"^^xsd:int ;
            :reference <_:contig17> .
<_2e> a :Position; 
           a :ExactlyKnownPosition ;
           :position "4"^^xsd:int ;
           :reference <_:contig29> .
```

=======
### Fuzy positions

Assume we have a protein aminoacid sequence "ACK" and a massspectrometry experiment says the amino acid 
A or C is glycosylated. But we don't know which of the two it is. We do know it is not "K".



```turtle
<_:glysolyatedAminoAcid>            a 	glyco:glycosylated_AA ;
				faldo:location <_:fuzzyPosition> .
_:fuzzyPosition 	a 	faldo:FuzzyPosition ,
				faldo:InRangePosition ;
			faldo:begin <_:exactBegin> ;
			faldo:end   <_:exactEnd> .
<_:faldoBegin>		a	faldo:ExactPosition ;
			faldo:position 1 ;
			faldo:refence <_:sequence> .
<_:faldoEnd>		a	faldo:ExactPosition ;
			faldo:position 2 ;
			faldo:refence <_:sequence> .
<_:sequence> a uniprot:Sequence ;
           rdf:value "ACK" .
```
In the above example uniprot and glyco refer to the glycoprotein and uniprot schema's.

### Probabilistic fuzzy positions

Here we have a begin position that could be one of two nucleotides. This case uses
a probablisitic model that denotes that the feature could start at both positions 1 or 2. Position 1
has a likelihood of 0.1 and position 2 has a likelihood of 0.9. 

```turtle
<_:3> a :Region ;
           :begin <_:3b> ;
           :end <_:3e> .
<_:3b> a faldop:ProbablePosition ;
           faldop:posibilities(<_:3bp1>,<_:3bp2>) .
<_:3bp1> a faldop:ProbablePosition ;
           faldop:probability "0.1"^^xd:double ;
           faldop:location <_:3bb1> .
<_:3bp2> a faldop:ProbablePosition ;
           faldop:probability "0.9"^^xd:double ;
           faldop:location <_:3bb2> .
<_:3bb1> a :Position ;
            a :ExactlyKnown ;
            :position "1"^^xsd:int ;
            :reference <_:1Strand> .
<_:3bb2> a :Position ;
            a :ExactlyKnown ;
            :position "2"^^xsd:int ;
            :reference <_:1Strand> .
```
