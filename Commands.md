# De Novo Genome Assemblers Commands

#  Paired-End Data:

For Velvet:

velveth results/ 31 -fastq.gz -shortPaired datafastq/OrgName.fastq.gz

velvetg results/



For Edena:

edena -paired datafastq/FileName_1.fastq datafastq/FileName_2.fastq

edena -e out.ovl



For ABySS:

abyss-pe k=31 name=OrgName in='datafastq/FileName_1.fastq datafastq/FileName_2.fastq'



For PERGA:

perga all -f datafastq/FileName_1.fastq datafastq/FileName_2.fastq -p 1 -o pergaERR



For Ray:

Ray -k 31 -p datafastq/FileName_1.fastq datafastq/FileName_2.fastq -o RayResult



For SSAKE:

Fastq to Fasta (.fa):
TQSfastq.py -f datafastq/FileName_1.fastq -t 30 -c 100 -e 33

TQSfastq.py -f datafastq/FileName_2.fastq -t 30 -c 100 -e 33


Modify Fasta file according to assembler need:
cat datafastq/FileName_1.fastq_T30C100E33.trim.fa |perl -ne 'if(/^(\>\@\S+)/){print "$1b\n";}else{print;}' > ~/datafastq/FileName_1.fastq.FIX.fa

cat datafastq/FileName_2.fastq_T30C100E33.trim.fa |perl -ne 'if(/^(\>\@\S+)/){print "$1b\n";}else{print;}' > ~/datafastq/FileName_2.fastq.FIX.fa

makePairedOutput2UNEQUALfiles.pl ~/datafastq/FileName_1.fastq.FIX.fa ~/datafastq/FileName_2.fastq.FIX.fa 400

SSAKE -f paired.fa -p 1 -g unpaired.fa -m 20 -w 5 -b results/




For SGA:

sga preprocess --pe-mode 1 datafastq/FileName_1.fastq datafastq/FileName_2.fastq > ~/datafastq/FileNameSGA.fastq

sga index datafastq/FileNameSGA.fastq

sga correct datafastq/FileNameSGA.fastq

sga rmdup datafastq/FileNameSGA.fastq

sudo cp FileNameSGA.bwt FileNameSGA.ec.fa FileNameSGA.rbwt FileNameSGA.rmdup.bwt 
FileNameSGA.rmdup.dups.fa FileNameSGA.rmdup.fa FileNameSGA.rmdup.rbwt 
FileNameSGA.rmdup.rsai FileNameSGA.rmdup.sai FileNameSGA.rsai FileNameSGA.sai  
datafastq/

sga overlap datafastq/FileNameSGA.fastq

sudo cp FileNameSGA.asqg.gz datafastq/

sga assemble datafastq/FileNameSGA.asqg.gz




#  Single-End Data:

For Velvet:

velveth results/ 31 -fastq -short datafastq/FileName_1.fastq

velvetg results/



For Edena:

edena -r datafastq/FileName_1.fastq

edena -e out.ovl



For ABySS:

abyss-pe k=31 name=ERR270988 se='datafastq/FileName_1.fastq'



For PERGA:

perga all -f datafastq/FileName_1.fastq -p 0 -o pergaERR



For Ray:

Ray -k 31 -s datafastq/FileName_1.fastq -o RayResult



For SSAKE:

SSAKE -f datafastq/FileName_1.fastq -w 5 -m 20 -o 2 -r 0.7 -t 0 -h 1 -p 0 -b results/



# Other commands:

1.  To unzip datasets
gunzip FileName_1.fastq.gz
gunzip FileName_2.fastq.gz

2.  To download perl script “shuffleSequences_fastq.pl” to merge paired-end data files
wget https://raw.githubusercontent.com/hacchy/MetaVelvet/master/scripts/shuffleSequences_fastq.pl

3.  To merge paired-end files FileName_1.fastq.gz and FileName_1.fastq.gz
perl shuffleSequences_fastq.pl FileName_1.fastq FileName_2.fastq OrgName.fastq.gz

4. To download perl script “assemblathon_stats.pl” and perl module “FAlite.pm”
wget https://raw.githubusercontent.com/ucdavis-bioinformatics/assemblathon2-analysis/master/assemblathon_stats.pl
wget http://korflab.ucdavis.edu/Unix_and_Perl/FAlite.pm

5.  To calculate time usage, memory usage and CPU usage
time /usr/bin/time -f "%P %M"




# Running assemblathon script:

1.	ABySS:
~/perl5/perlbrew/perls/perl-5.10.0/bin/perl assemblathon_stats.pl results/name-contigs.fa

2.	Velvet: 
~/perl5/perlbrew/perls/perl-5.10.0/bin/perl assemblathon_stats.pl results/contigs.fa

3.	Perga:
~/perl5/perlbrew/perls/perl-5.10.0/bin/perl assemblathon_stats.pl results/name_contigs.fa

4.	Edena: 
~/perl5/perlbrew/perls/perl-5.10.0/bin/perl assemblathon_stats.pl results/out_contigs.fasta

5.	Ray: 
~/perl5/perlbrew/perls/perl-5.10.0/bin/perl assemblathon_stats.pl results/Contigs.fasta

6.	SGA: 
~/perl5/perlbrew/perls/perl-5.10.0/bin/perl assemblathon_stats.pl results/default-contigs.fa

7.	SSAKE:
~/perl5/perlbrew/perls/perl-5.10.0/bin/perl assemblathon_stats.pl results/.contigs
