So these are my notes for Unix for Poets. I wrote them in the order that I encountered them while reading said book.

Gnome-Terminal Command: grep
	ex:
		% grep name *
		This will search all files in the directory using name.
Gnome-Terminal Command: cat 
	ex:
		% cat file
		This will display the file

Gnome-Terminal Command: sort
	ex:
		% sort pet.txt
		Will sort the lines on the order of spaces, numbers, then letters.

Gnome-Terminal Command: uniq
	ex:
		% uniq pet.txt
		will give results like cat but omits adjacent repeated lines

Gnome-Terminal Command: uniq -c
	ex:
		% uniq -c pet.txt
		will giver results like cat but will omit adjacent repeated lines and gives the number of occurrences before each no.

Problem 1: List all the words used in the Genesis of the Bible.
	my answer: tr -cd A-Za-z' ' < genesis | tr ' ' '\n' | sort | uniq -c
	better answer: tr -sc 'A-Za-z' '\012' < genesis | sort | uniq -c

	*if multiple adjacent characters are replaced with '\012' or '\n' (\012 and \n is the same), result will only give an output of one '\n'

	Sub Problems:
		Sort by dictionary order:
			default of sort is the dictionary order.

		Sort by ryhming order and count upper case and lower case the same:
			tr -sc A-Za-z '\012' < genesis | tr 'A-Z' 'a-z' | rev | sort | rev | uniq -c

Gnome-Terminal Command: uniq -c
	ex:
		% sed 5q < genesis.txt
		returns the first no. of lines depending on the argument value.
	link:
		https://www.digitalocean.com/community/tutorials/the-basics-of-using-the-sed-stream-editor-to-manipulate-text-in-linux

Gnome-Terminal Command: mv
	ex:
		% mv genesis /home/some/folder/genesis.file
		This will delete move the genesis file to /home/some/folder/ and rename it to genesis.file

Gnome-Terminal Command: tail
	ex:
		% tail file
		Will return the last 10 lines of the text file
		
		% tail -n x textfile
		Will return the last x lines of the text file
	
Gnome-Terminal Command: Paste
	ex:
		% paste catfile dogfile
		Will return an output combining the lines of bothfiles separated by a tab. So on line1 it will have the line1 of catfile, tab, then the line2 of dogfile. Same goes for line2 and so on.
	
		% paste -d ' ' catfile dogfile
		same as above but intead of a tab, the spacing is replaced by ' '



Problem 2:
	Create bigrams on the Genesis Chapter. A pair of consecutive written units like words per newline.
		Answer:
		% tr -sc 'A-Za-z' '\012' < genesis | tr 'A-Z' 'a-z' > genesis.1
		% tail -n +2 genesis.1 > genesis.2
		% paste -d ' ' genesis.1 genesis.2 | sort | uniq -c | sort -n > genesis.bigram

Exercise: Count the trigrams based on their frequency of their appearance in the specified textfile genesis.
	my answer:
		% tr -sc 'A-Za-z' '\012' < genesis | tr 'A-Z' 'a-z' > genesis.1
		% tail -n +2 genesis.1 > genesis.2
		% tail -n +3 genesis.2 > genesis.3
		% paste -d ' ' genesis.1 genesis.2 genesis.3 | sort | uniq -c | sort -n > genesis.trigram

Gnome-Terminal Command: wc -l
	ex:
		% wc -l file
		prints the newline count.

Gnome-Terminal Command: grep -c
	ex:
		% grep -c 'word to search or regex' filename
		prints the count of result entries

Grep Exercises:
	How many uppercase words are there in the Genesis? Lowercase?
		Uppercase: 
			% tr -sc 'A-Za-z' '\012' < genesis | grep -c '[A-Z]'
		Lowercase:
			% tr -sc 'A-Za-z' '\012' < genesis | grep -c '[a-z]'

	How many 4-letter character?
		% tr -sc 'A-Za-z' '\012' < genesis | grep -c '^....$'	

	Are there words with no vowels?
		there are.
		% tr -sc 'A-Za-z' '\012' < genesis | grep '^[^aeiouAEIOU]*$' | sort | uniq -c
		the breakdown of the regex:
			^ start
			[^aeiouAEIOU] characters that are not vowels. This also includes numbers and special characters.
			* any no. of instances of [^aeiouAEIOU] even zero.
			$ ending

	words with one vowel?
		% tr -sc 'A-Za-z' '\012' < genesis | grep -i '^[^aeiou]*[aeiou][^aeiou]*$'

	words with two vowels?
		% tr -sc 'A-Za-z' '\012' < genesis | grep -i '^[^aeiou]*[aeiou][^aeiou]*[aeiou][^aeiou]*$'

	delete dipthongs.
		% tr -sc 'A-Za-z' '\012' < genesis | grep -i '^[^aeiou]*[aeiou][^aeiou]*[aeiou][^aeiou]*$' | grep -v 'e$

	find verses with the word light.
		grep 'light' < genesis

		how many have two or more instances of 'light'
		grep -ic '.*light*.*light*.*' < genesis

		how many have exactly three?
		grep -ic '.*light.*light.*light.*' < genesis

sed exercises
	
	print until it encounters the first instance of light:
		% sed '/light/q' genesis 

	substitute all light instances with dark:
		% sed 's/light/dark/g' genesis
		or
		% sed 's_light_dark_g' genesis
		* the g is needed to replace all instances and not just the first instance encountered in the line. If g is not included, it will only replace the first instance it finds in the line.
		* the s is used to state the we want a substitution.

	replace all words ending with ly to -ly:
		% sed 's/ly /-ly /g' genesis | grep -e '-ly'
		*-e in grep to treat the dash as the pattern and not as an argument

	replacing everything after the first white space with nothing:
		% sed 's/ .*//' genesis
		or
		% sed 's/[ ].*//' genesis
		the space is the white space

Sed Exercises:
	1. Count morphs in genesis: I do not know what this question is asking.

	2. Count word  initial consonant sequences.
		% tr -sc 'A-Za-z' '\012' < genesis | sed 's/[aeiouAEIOU].*//' | sed '/^$/d' | sort | wc -l

		*sed '/^$/d'
		deletes blank lines.

	3. Count word final consonant sequences.
		% tr -sc 'A-Za-z' '\012' < genesis | rev | sed 's/[aeiouAEIOU].*//' | rev | sed '/^$/d' | sort | wc -l

Gnome-Terminal Command: awk
	ex:
		% awk "{print NF}"
		prints the number of fields per line.
		awk can actually do a lot of things besides the example above. See below.

awk exercise:
	sort words by syllable (assumption: 1 vowel 1 syllable, 2 non consecutive vowels, 2 syllables)
		% tr -sc 'A-Za-z' '\012' < genesis > genesis.words	
		% sed 's/[^aeiouAEIOU]/ /g' < genesis.words | awk '{print NF}' > genesis.syllable_count
		% paste genesis.syllable_count genesis.words | sort | uniq | sort | awk '{print $2}'

	return words with more than 300 occurences:
		% tr -sc 'A-Za-z' '\012' < genesis | sort | uniq -c | awk '$1 > 300 {print $0}'
		or
		% tr -sc 'A-Za-z' '\012' < genesis | sort | uniq -c | awk '$1 > 300'

	return vowel sequences with more than 1000 occurences:
		% sed 's_[^aeiouAEIOU]_\n_g' < genesis | sed '/^$/d' | tr 'A-Z' 'a-z' | sort | uniq -c | awk '$1 > 1000'

	return bigrams that appear exactly twice:
		% tr -sc 'A-Za-z' '\012' < genesis | tr 'A-Z' 'a-z' > genesis.1
		% tail -n +2 genesis.1 > genesis.2
		% paste -d ' ' genesis.1 genesis.2 | sort | uniq -c | sort -n > genesis.bigram
		% awk '$1 == 2' genesis.bigram

sort -u
	ex:
		% sort -u text file
		its like calling % sort file | uniq | sort

	Find words that are palindromes
		% tr -sc 'A-Za-z' '\012' < genesis | sort | uniq | sort > genesis.words
		or
		% tr -sc 'A-Za-z' '\012' < genesis | sort -u  > genesis.words
		% rev genesis.words | paste - genesis.words | awk '$1 == $2'

	Find words that can be spelled backwards
		% tr -sc 'A-Za-z' '\012' < genesis | sort -u  > genesis.words
		% rev genesis.words > genesis.reversed
		% cat genesis.words genesis.reversed | sort | uniq -c | awk '$1 >= 2'

		* use only '' on awk and not ""

Gnome-Terminal Command: comm
	ex:
		comm textfile1 textfile2
		Returns three columns.
			First column returns all the unique words in textfile1.
			2nd column returns all the unique words in textfile2
			3rd column returns words that exist on both files.

	Compare bible and kingjames if words are unique to each other.
		% tr -sc 'A-Za-z' '\012' < bible | sort -u > bible.words
		%tr -sc 'A-Za-z' '\012' < wsj | sort -u > wsj.words
		% comm bible.words wsj.words

* on awk, regexs should have slashes and ~ at the start but on grep only '' suffices. See below for examples.

awk exercises
	1. Find bigrams with both words ending in 'ing'.
		% awk '$2 ~ /ing$/' genesis.bigram | uniq -u > genesis.bigram.2
		% awk '$1 ~ /ing$/' genesis.bigram | uniq -u > genesis.bigram.1
		% cat genesis.bigram.1 genesis.bigram.2 | sort | uniq -c | sort -n | awk '$1 == 2' | sed 's/ *[0-9] //'
		or
		% awk '$0 ~ /.*ing .*ing/' genesis.bigram

	2. 	Find bigrams with both words ending in ed.
		% awk '$0 ~ /.*ed .*ed/' genesis.bigram

	3. 	write a uniq -c program in awk using sample file abc
		*create a sample file
		echo 'a a b b c c c' > abc
		% awk '$0 == prev { c++ } $0 != prev { print c, prev; c = 1; prev=$0 } END { print c, $0}' abc