grep
	ex:
		% grep Name *
		This will search all files in the directory with Name
cat 
	ex:
		% cat nameoffile
		This will display the file

sort
	ex:
		% sort pet.txt
		Will sort the lines on the order of spaces, numbers, then letters.
uniq
	ex:
		% uniq pet.txt
		will give results like cat but omits adjacent repeated lines

uniq -c
	ex:
		% uniq -c pet.txt
		will giver results like cat but will omit adjacent repeated lines and gives the number of occurrences before each no.

Problem 1:
	my answer: tr -cd A-Za-z' ' < genesis | tr ' ' '\n' | sort | uniq -c
	better answer: tr -sc 'A-Za-z' '\012' < genesis | sort | uniq c-

	*if multiple adjacent characters are replaced with '\012' or '\n' (they are both the same), result will only give an output of one '\n'

	*all adjacent line breaks are \n or \012 will be converted to only one '\n'

	Sub Problems:
		Sort by dictionary order:
			default of sort is the dictionary order.

		Sort by ryhming order and count upper case and lower case the same:
			tr -sc A-Za-z '\012' < genesis | tr 'A-Z' 'a-z' | rev | sort | rev | uniq -c

sed
	ex:
		% sed 5q < genesis.txt
		returns the first no. of lines depending on the argument value.
	link:
		https://www.digitalocean.com/community/tutorials/the-basics-of-using-the-sed-stream-editor-to-manipulate-text-in-linux
mv
	moves then renames the file

tail
	ex:
		% tail textfile
		Will return the last 10 lines of the textfile
		
		% tail -n x textfile
		Will return the last x lines of the textfile
		
		% tail -n +x texfile
	ex:
	% paste catfile dogfile
	Will return an output combining the lines of bothfiles separated by a tab. So on line1 it will have the line1 of catfile, tab, then the line2 of dogfile. Same goes for line2 and so on.
	
	% paste -d ' ' catfile dogfile
	same as above but intead of a tab, the spacing is replaced by ' '



Problem 2:
	my answer:
		tr -sc 'A-Za-z' '\012' < genesis | tr 'A-Z' 'a-z' > genesis.1
		tail -n +2 genesis.1 > genesis.2
		paste -d ' ' genesis.1 genesis.2 | sort | uniq -c | sort -n > genesis.bigram

Exercise: Count the trigrams
	my answer:
		tr -sc 'A-Za-z' '\012' < genesis | tr 'A-Z' 'a-z' > genesis.1
		tail -n +2 genesis.1 > genesis.2
		tail -n +3 genesis.2 > genesis.3
		paste -d ' ' genesis.1 genesis.2 genesis.3 | sort | uniq -c | sort -n > genesis.trigram

		*paste -d ' ' means to change the default delimiter from tab to ' '

wc -l
	prints the newline count

grep -c
	prints the count of result entries

Grep Exercises:
	How many uppercase words are there in the Genesis? Lowercase?
		Uppercase: tr -sc 'A-Za-z' '\012' < genesis | grep -c '[A-Z]'
		Lowercase: tr -sc 'A-Za-z' '\012' < genesis | grep -c '[a-z]'

	How many 4-letter character?
		tr -sc 'A-Za-z' '\012' < genesis | grep -c '^....$'
		link:http://stackoverflow.com/questions/2887264/how-do-you-use-grep-to-find-terms-that-are-n-characters-long	

	Are there words with no vowels?
		there are
		tr -sc 'A-Za-z' '\012' < genesis | grep '^[^aeiouAEIOU]*$' | sort | uniq -c
		the breakdown:
			^ start
			[^aeiouAEIOU] letters that are not vowels
			* any no. of instances of [^aeiouAEIOU] followed from the initial [^aeiouAEIOU]
			$ ending

	words with one vowel?
		tr -sc 'A-Za-z' '\012' < genesis | grep -i '^[^aeiou]*[aeiou][^aeiou]*$'

	words with two vowels?
		tr -sc 'A-Za-z' '\012' < genesis | grep -i '^[^aeiou]*[aeiou][^aeiou]*[aeiou][^aeiou]*$'

	delete dipthongs
		tr -sc 'A-Za-z' '\012' < genesis | grep -i '^[^aeiou]*[aeiou][^aeiou]*[aeiou][^aeiou]*$' | grep -v 'e$

	find verses with the word light
		grep 'light' < genesis

		how many have two or more instances of 'light'
		grep -ic '.*light*.*light*.*' < genesis

		how many have exactly three?
		grep -ic '.*light.*light.*light.*' < genesis

sed editor chapter:
	
	print until it encounters the first instance of light:
		sed '/light/q' genesis 

	substitute all light instances with dark:
		sed 's/light/dark/g' genesis
		or
		sed 's_light_dark_g' genesis
		* the g is needed to replace all instances and not just the first instance encountered in the line. 
		* the s is used to state the we want a substitution.

	replace all words ending with ly to -ly:
		sed 's/ly /-ly /g' genesis | grep -e '-ly'
		*-e in grep to treat the dash as the pattern and not as an argument

	replacing everything after the first white space with nothing:
		sed 's/ .*//' genesis
		or
		sed 's/[ ].*//' genesis
		the space is the white space

Sed Exercises:
	1. Count morphs in genesis: I do not know what this question is asking.

	2. Count word  initial consonant sequences.
		tr -sc 'A-Za-z' '\012' < genesis | sed 's/[aeiouAEIOU].*//' | sed '/^$/d' | sort | wc -l

		*sed '/^$/d' deletes blank lines.

	3. Count word final consonant sequences.
		tr -sc 'A-Za-z' '\012' < genesis | rev | sed 's/[aeiouAEIOU].*//' | rev | sed '/^$/d' | sort | wc -l

cut

awk
	awk "{print NF}"
	prints the number of fields

awk exercise:
	sort words by syllable (assumption: 1 vowel 1 syllable, 2 non consecutive vowels, 2 syllables)
		tr -sc 'A-Za-z' '\012' < genesis > genesis.words	
		sed 's/[^aeiouAEIOU]/ /g' < genesis.words | awk '{print NF}' > genesis.syllable_count
		paste genesis.syllable_count genesis.words | sort | uniq | sort | awk '{print $2}'

	return words with more than 300 occurences:
		tr -sc 'A-Za-z' '\012' < genesis | sort | uniq -c | awk '$1 > 300 {print $0}'
		or
		tr -sc 'A-Za-z' '\012' < genesis | sort | uniq -c | awk '$1 > 300'

	return vowel sequences with more than 1000 occurences:
		sed 's_[^aeiouAEIOU]_\n_g' < genesis | sed '/^$/d' | tr 'A-Z' 'a-z' | sort | uniq -c | awk '$1 > 1000'

	return bigrams that appear exactly twice:
		tr -sc 'A-Za-z' '\012' < genesis | tr 'A-Z' 'a-z' > genesis.1
		tail -n +2 genesis.1 > genesis.2
		paste -d ' ' genesis.1 genesis.2 | sort | uniq -c | sort -n > genesis.bigram
		awk '$1 == 2' genesis.bigram

sort -u 
	its like calling sort file | uniq | sort

	Find palindrome
		tr -sc 'A-Za-z' '\012' < genesis | sort | uniq | sort > genesis.words
		or
		tr -sc 'A-Za-z' '\012' < genesis | sort -u  > genesis.words
		rev genesis.words | paste - genesis.words | awk '$1 == $2'

	Find words that can be spelled backwards
		tr -sc 'A-Za-z' '\012' < genesis | sort -u  > genesis.words
		rev genesis.words > genesis.reversed
		cat genesis.words genesis.reversed | sort | uniq -c | awk '$1 >= 2'

		* '' should only be used and not ""

	compare bible and kingjames if words are unique to each other.
		tr -sc 'A-Za-z' '\012' < bible | sort -u > bible.words
		tr -sc 'A-Za-z' '\012' < wsj | sort -u > wsj.words
		comm bible.words wsj.words

* on awk, regexs should have dashes  and ~ // but on grep only '' suffices.


awk exercises
	1. Find bigrams with both words ending in ing.
		awk '$2 ~ /ing$/' genesis.bigram | uniq -u > genesis.bigram.2
		awk '$1 ~ /ing$/' genesis.bigram | uniq -u > genesis.bigram.1
		cat genesis.bigram.1 genesis.bigram.2 | sort | uniq -c | sort -n | awk '$1 == 2' | sed 's/ *[0-9] //'

		or

		awk '$0 ~ /.*ing.*ing/' genesis.bigram

	2. 	Find bigrams with both words ending in ed.
		awk '$0 ~ /.*ed .*ed /' genesis.bigram

	3. 	write a uniq -c program in awk
		awk '$0 == prev { c++ } $0 != prev { print c, prev; c = 1; prev=$0 } END { print c, $0}' abc

* uniq is the same as uniq1 much like awk is the same as mawk