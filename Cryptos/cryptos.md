# Cryptos - 400
	Oops! Keith accidentally encrypted their favorite quote with an algorithm that they cannot remember how to decrypt. Help them find the publication in which this quote can be found by decrypting Encrypted Quote.mov.

	Hint: One, two, and my own.

[Encrypted Quote](encrypted_quote.mov)

------------------

We soon see that the file is broken, so we open it in a hex editor to find:

`This is here just to make you mad.` prepended to the front. We remove it, and rename to a pdf, but it still doesn't work. We run strings on it, and find a link:

`https://en.wikipedia.org/wiki/Kryptos`

We remove this data and we are now also able to open the file. The ciphertext is:

	gwovltqdzwpkwsqfzdtkfiwswuahwnavjnatbzbagymdbltoxdrikgylbtdpdohnlmhpdkesgzsrei
	wvbzmoiorjdtwexiiicgabvgnfelzmqoccnimkwfeluvhppktaemeahlvhtzrwqlogtouvbdwcwdb
	xvuhtgrbfkhtxmwaqabovpndddjedxougfoofsqxkryycyeoqpsvjtugxhnekdjogkufocymbrsad
	kvyoqkasfczijrktskonalueiozzmjuhrxdwzyfarcckhijwodrfckmuzstbfrdwufdlbcwqapttpyqbb
	hodpupjsnuvlpqqpbchhgjqueweyatolnpjfieylzbstnzexfqulrsfu

In the Kryptos wikipedia page, we see that the Kryptos parts 1 and 2 used keyed vigeneres, with keys `KRYPTOS, ABSCISSA` and `KRYPTOS, PALIMPSEST`. This is what the hint refers to: one and two are Kryptos decryption keys, and "my own" is yet another vigenere with KRYPTOS and some passphrase. We decode it with both (with keyed vigeneres, it doesn't matter what order you decrypt them in) using [rumkin](http://rumkin.com/tools/cipher/vigenere-keyed.php). Now we have:

	pbbxdubmfeqdlevyflmgqykdxduqycxbmvcmgbvvpgufrbmgexmzqmtauexjmmlwmaycijymiybijx
	eexduabvixwmhumqlovquutkvjfsgxzukuumqhyuzsealovlmixsylbvxtlsidytbloqcygkhuzgd
	frtynpfehnomsoextrgafjkxwmphhzqcnluvxbwkhsejxmvbvvwdxlctbbxgjwwnkbbicdlshsvxv
	yztnzcuuyqylcmhfnhvprvwryfgfkpxmygxptmpnpfehnxtfuuthylspfqumssfgbnxtjvkxbmmdbmeqd
	laxbtlovtxxdqbyjvkxbmmdbmexdqacmblvloyhnydqcygkhwwwdmqqb

Now we need to figure out the "my own" part. We read that Kryptos is located at Langley, so we try `langley` as our key, and it works!
	
	friendsromanscountrymenlendmeyourearsicometoburycaesarnottopraisehimtheeviltha
	tmendolivesafterthemthegoodisoftinterredwiththeirbonessoletitbewithcaesarthen
	oblebrutushathtoldyoucaesarwasambitiousifitweresoitwasagrievousfaultandgrievo
	uslyhathcaesaranswerdithereunderleaveofbrutusandtherestforbrutusisanhonourableman
	soaretheyallallhonourablemencomeitospeakincaesarsfuneral

We were asked to find what this quote was from, and a quick google search shows that it was from a play by Shakespeare titled `Julius Caesar`.

Flag: `Julius Caesar`