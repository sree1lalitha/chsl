# Text Complexity for Hindi Text

# importing the packages
import pandas as pd
import numpy as np
import nltk
from textblob import TextBlob

# blob the text using TextBlob
blob = TextBlob("Text")
blob.sentences # prints all the sentences in the blob
for words in blob.words:    # prints all words in the blob
    print(words) 
p = len(blob.sentences)
print(p)
q = (len(blob.words))
print(q)

import re

vowels = '\u0904-\u0914\u0960-\u0961\u0972-\u0977'
consonants = '\u0915-\u0939\u0958-\u095F\u0978-\u097C\u097E-\u097F'
glottal = '\u097D'

vowel_signs = '\u093E-\u094C\u093A-\u093B\u094E-\u094F\u0955-\u0957\u1CF8-\u1CF9'
nasals = '\u0900-\u0902\u1CF2-\u1CF6'
visarga = '\u0903'
nukta = '\u093C'
avagraha = '\u093D'
virama = '\u094D'

vedic_signs = '\u0951-\u0952\u1CD0-\u1CE1\u1CED'
visarga_modifiers = '\u1CE2-\u1CE8'
combining = '\uA8E0-\uA8F1'

om = '\u0950'

accents = '\u0953-\u0954'
dandas = '\u0964-\u0965'
digits = '\u0966-\u096F'
abbreviation = '\u0970'
spacing = '\u0971'

vedic_nasals = '\uA8F2-\uA8F7\u1CE9-\u1CEC\u1CEE-\u1CF1'
fillers = '\uA8F8-\uA8F9'
caret = '\uA8FA'
headstroke = '\uA8FB'

space = '\u0020'
joiners = '\u200C-\u200D'

def syllabify(inputtext):

    syllables = []
    curr = ''

    # iterate over each character in the input. if a char belongs to a 
    # class that can be part of a syllable, then add it to the curr 
    # buffer. otherwise, output it to syllables[] right away.

    for char in inputtext:

        if re.match('[' + vowels + avagraha + glottal + om + ']', char):

            # need to handle non-initial independent vowel letters,
            # avagraha, and om

            if curr != '':
                syllables.append(curr)
                curr = char
            else:
                curr = curr + char

        elif re.match('[' + consonants + ']', char):

            # if last in curr is not virama, output curr as syllable
            # else add present consonant to curr

            if len(curr) > 0 and curr[-1] != virama:
                syllables.append(curr)
                curr = char
            else:
                curr = curr + char

        elif re.match('[' + vowel_signs + visarga + vedic_signs + ']', char):
            curr = curr + char

        elif re.match('[' + visarga_modifiers + ']', char):

            if len(curr) > 0 and curr[-1] == visarga:
                curr = curr + char
                syllables.append(curr)
                curr = ''
            else:
                syllables.append(curr)
                curr = ''

        elif re.match('[' + nasals + vedic_nasals + ']', char):

            # if last in curr is a vowel sign, output curr as syllable
            # else add present vowel modifier to curr and output as syllable

            vowelsign = re.match('[' + vowel_signs + ']$', curr)
            if vowelsign:
                syllables.append(curr)
                curr = ''
            else:
                curr = curr + char
                syllables.append(curr)
                curr = ''

        elif re.match('[' + nukta + ']', char):
            curr = curr + char

        elif re.match('[' + virama + ']', char):
            curr = curr + char

        elif re.match('[' + digits + ']', char):
            curr = curr + char

        elif re.match('[' + fillers + headstroke + ']', char):
            syllables.append(char)

        elif re.match('[' + joiners + ']', char):
            curr = curr + char

        else:
            pass
            #print ("unhandled: " + char + " ", char.encode('unicode_escape'))

    # handle remaining curr
    if curr != '':
        syllables.append(curr)
        curr = ''

    # return each syllable as item in a list
    return syllables
    
    def getSyllables(inputtext):

    word_syllables = []
    all_words = []

    for word in inputtext.split():

        word = word.strip()
        word = re.sub('[\s\n\u0964\u0965\.]', '', word)

        word_syllables = syllabify(word)
        #number_syllables = len(word_syllables)

        #joined_syllables = '\u00B7'.join(word_syllables)
        joined_syllables = word_syllables

        # make list of lists containing each word
        #all_words.append([word, joined_syllables, number_syllables])
        all_words.append([word, joined_syllables])

    return all_words
    
    def getSyllableStats(inputtext):

    syllcount = {}
    wordcount = {}
    word = ''
    syllable = ''
    count = ''
    syllablestatus = {}

    words = getSyllables(inputtext)

    for entry in words:

        word = entry[0]
        syllables = entry[1]
        #count = entry[2]

        #word_syllables = syllables.split('\u00B7')
        word_syllables = syllables

        # count all words in input
        if word in wordcount:
            wordcount[word] += 1
        else:
            wordcount[word] = 1

        # count all syllables in input
        for syll in word_syllables:
            if syll in syllcount:
                syllcount[syll] += 1
            else:
                syllcount[syll] = 1

    syllablestatus.update({'words' : wordcount})
    syllablestatus.update({'syllables' : syllcount})

    return (syllablestatus)
    
    syllabify(blob)
    
    results = getSyllables(blob)

for x in results:
    print (x[0], ":", '\u00B7'.join(x[1]), ":", len(x[1]))
    
    from IPython.display import HTML, display
import tabulate
display(HTML(tabulate.tabulate(results, tablefmt='html')))

stats = getSyllableStats(text)
for k, v in stats.items():
    print (k)
    for x, y in v.items():
        print ("\t", x, ":", y)
    print ("total", k, "=", sum(v.values()))
    print ("unique", k, "=", len(v), end='\n\n')
    
    # Now to check the readablity we will use the Flesch Reading Ease formula,

# Flesch Reading Ease = 206.835-(1.015xASL)-(84.5xASW)

# and Flesch-kincaid Grade-Level = (0.39xASL)+(11.8XASW)-15.59

#Where, ASL = Total Number of words/Total number of sentences

      # ASW = Total syllable count/number of words
      
      def ASL (w,s):
    ASL = w/s
    return ASL
    
    def ASW(sy,w):
    asw = sy/w
    return asw
    
    def FRE(ASL,ASW):
    flesch_reading_ease = 206.835-(1.015 * ASL)-(84.5 * ASW)
    return flesch_reading_ease
    
    def FKGL(ASL,ASW):
    flesch_kincaid_Grade_level = (0.39*ASL)+(11.8*ASW)-15.59
    return flesch_kincaid_Grade_level
    
    # The FRE should be in the following level
    
    Score	      School level	              Notes
100.00-90.00	  5th grade     	      Very easy to read. Easily understood by an average 11-year-old student. # for hindi language the score should be >150
90.0–80.0	      6th grade	            Easy to read. Conversational English for consumers.
80.0–70.0	      7th grade	            Fairly easy to read.
70.0–60.0	      8th & 9th grade	      Plain English. Easily understood by 13- to 15-year-old students.
60.0–50.0	      10th to 12th grade	  Fairly difficult to read.
50.0–30.0	      College             	Difficult to read.
30.0–0.0	      College graduate	    Very difficult to read. Best understood by university graduates.



      
      
