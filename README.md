This is all about analysing the text complexity in the levels of the Hindi and Telugu Stories.

For this, I had choosen TextBlob library to read the text.
To check the readablity Flesch Reading Ease formula and for the grade leve Flisch-Kincaid Grade level has been used. These are the formulas to them.

Flesch Reading Ease = 206.835-(1.015xASL)-(84.5xASW)

and Flesch-kincaid Grade-Level = (0.39xASL)+(11.8XASW)-15.59

Where, ASL = Total Number of words/Total number of sentences
       ASW = Total syllable count/number of words.
       The score levels and grade level should be as below. For Hindi text the score should be >150.
       
       Score	Difficulty          School level
       
       90-100	Very Easy           5th grade   
       
       80-89	Easy                6th grade
       
       70-79	Fairly Easy         7th grade
       
       60-69	Standard            8th & 9th grade
       
       50-59	Fairly Difficult    10th to 12th grade
       
       30-49	Difficult           college
       
       0-29	Very Confusing          college graduate
       
 To find the syllable count, funtions are defined by using the unicodes of the vowels, consonants etc.,of the languages Hindi and Telugu.
