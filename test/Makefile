

FASTQBIN=../bamhash_checksum_fastq
BAMBIN=../bamhash_checksum_bam
WGSIM=wgsim

REF=ecoliMG1655.fa

.PHONY: FORCE clean reallyclean


#r.namesorted.fastq.md5sum FORCE
r: r.sorted.bam.md5sum r.unsorted.bam.md5sum r.fastq.md5sum r.namesorted.bam.md5sum r.single.sorted.bam.md5sum r.single.unsorted.bam.md5sum r.single.fastq.md5sum r.single.namesorted.bam.md5sum FORCE
	for x in r*.md5sum; do echo $$x $$(cat $$x); done

# retrieve ref from internet if not already available
${REF}:
	wget -nH https://github.com/ctb/edda/raw/master/doc/tutorials-2012/files/ecoliMG1655.fa.gz
	gunzip ${REF}.gz

${REF}.bwt: ${REF}
	bwa index ${REF}

r1.fastq: ${REF}.bwt
	${WGSIM} -N 100000 ${REF} r1.fastq r2.fastq
	#sed '1~4N; s/\/1//' r1.tmp.fastq > r1.fastq
	#sed '1~4N; s/\/2//' r2.tmp.fastq > r2.fastq
	#rm r1.tmp.fastq r2.tmp.fastq


r2.fastq: r1.fastq
	#noop


r.unsorted.bam: r1.fastq r2.fastq
	bwa mem ${REF} r1.fastq r2.fastq | samtools view -bSh - > r.unsorted.bam

r.single.unsorted.bam: r1.fastq 
	bwa mem ${REF} r1.fastq | samtools view -bSh - > r.single.unsorted.bam


r.sorted.bam: r.unsorted.bam
  #samtools sort -o r.sorted.bam -T test -O bam r.unsorted.bam
	samtools sort  -f r.unsorted.bam r.sorted.bam

r.namesorted.bam: r.unsorted.bam
  #samtools sort -o r.namesorted.bam -T test -O bam -n r.unsorted.bam
	samtools sort -f -n r.unsorted.bam r.namesorted.bam



r.single.sorted.bam: r.single.unsorted.bam
	samtools sort  -f r.single.unsorted.bam r.single.sorted.bam

r.single.namesorted.bam: r.single.unsorted.bam
  #samtools sort -o r.namesorted.bam -T test -O bam -n r.unsorted.bam
	samtools sort -f -n r.single.unsorted.bam r.single.namesorted.bam


#r.namesorted.1.fastq: r.namesorted.bam
#	bam2fastq -p -o r.namesorted.fastq r.namesorted.bam



r.sorted.bam.md5sum: r.sorted.bam FORCE
	${BAMBIN} r.sorted.bam > r.sorted.bam.md5sum

r.namesorted.bam.md5sum: r.namesorted.bam FORCE
	${BAMBIN} r.namesorted.bam > r.namesorted.bam.md5sum

r.unsorted.bam.md5sum: r.unsorted.bam FORCE
	${BAMBIN} r.unsorted.bam > r.unsorted.bam.md5sum


r.single.sorted.bam.md5sum: r.single.sorted.bam FORCE
	${BAMBIN} r.single.sorted.bam > r.single.sorted.bam.md5sum

r.single.namesorted.bam.md5sum: r.single.namesorted.bam FORCE
	${BAMBIN} r.single.namesorted.bam > r.single.namesorted.bam.md5sum

r.single.unsorted.bam.md5sum: r.single.unsorted.bam FORCE
	${BAMBIN} r.single.unsorted.bam > r.single.unsorted.bam.md5sum

#r.namesorted.fastq.md5sum: r.namesorted.1.fastq FORCE
#	${FASTQBIN} r.namesorted.1.fastq r.namesorted.2.fastq > r.namesorted.fastq.md5sum

r.fastq.md5sum: r1.fastq r2.fastq FORCE
	${FASTQBIN} r1.fastq r2.fastq > r.fastq.md5sum 

r.single.fastq.md5sum: r1.fastq  FORCE
	${FASTQBIN} r1.fastq  > r.single.fastq.md5sum 


FORCE:


clean:
	rm -f *.md5sum

reallyclean: clean
	rm -f *.bam *.fastq

