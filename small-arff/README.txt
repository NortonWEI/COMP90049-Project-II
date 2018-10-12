This README describes the various files created for 2018S2 COMP90049 Project 2. The dataset has been adapted from a much larger dataset of tweets from users associated with the Internet Research Agency. The construction of the dataset is described in the following working paper (http://pwarren.people.clemson.edu/Linvill_Warren_TrollFactory.pdf):
Linvill, Darren and Patrick Warren (2018) Troll Factories: The Internet Research Agency and State-Sponsored Agenda Building (working paper). Clemson University.

The data was subsequently made available via FiveThirtyEight's GitHub (https://github.com/fivethirtyeight/russian-troll-tweets/), who further provide some discussions underlying sharing the data, and some preliminary analysis, via the following article:
Roeder, Oliver (2018) Why we're sharing 3 Million Russian Troll Tweets. FiveThirtyEight. 31 Jul 2018, https://fivethirtyeight.com/features/why-were-sharing-3-million-russian-troll-tweets/.

The dataset, as a whole, comprises about 3M tweets, issued by 2848 unique users. To make the data easier for you to work with, we have constructed a smaller dataset of 175 randomly chosen users and a total of 223K tweets. This was then partitioned into training (roughly 60%), development (20%), and test (20%) sets, where all of the tweets from a single user appear in the same dataset.

There are various files in these archives: other than this README, each one can be identified by its filename, in the format {set}-{type}.{filetype}
- {set} refers to either train, dev, or test:
    train: You should use this data when building a model
    dev: You should use this data when evaluating a model
    test: You should submit the outputs on this data; the labels (?) are not given

- {type} refers to either tweets, mostXX, or bestXX:
    tweets: This contains the raw text of the corresponding tweets, one tweet per line, in the following format:
            Tweet-ID,User-ID,Tweet-Text,Class
              where Tweet-ID is a unique value, corresponding to the line number, prefixed by "1" (train), "2" (dev), or "3" (test);
              User-ID has been coverted to a number (i.e. deliberately obscured);
              and Class is one of LeftTroll, RightTroll, or Other.

    mostXX: For these files, we have pre-processed the corresponding tweets, and have recorded the term frequency for the top XX terms according to document frequency.

    bestXX: For these files, we have pre-processed the corresponding tweets, and have recorded the term frequency for the terms with the greatest Mutual Information and Chi-Square values. The total number of features does not correspond to XX: rather, for each of the two feature selection methods, the top XX features were determined for each of the three classes, and then the resulting list was sorted, with duplicate entries removed. 

    It is worth noting that to process the raw text of the tweets, we folded case, and removed all characters that are not English alphabetic characters ([a-z]) or whitespace. This is significant, as many tweets contained hashtags, at-mentions, and hypertext links which were consequently garbled, and numerous tweets were not in English.

- {filetype} refers to either csv or arff:
    csv: The various processed files (like train-best10.csv, etc.) should be conformant CSV, with the following format:
         Tweet-ID,User-ID,List-of-term-frequencies,Class
            where Tweet-ID is a unique value, corresponding to the line number, prefixed by "1" (train), "2" (dev), or "3" (test);
            User-ID has been coverted to a number (i.e. deliberately obscured);
            and Class is one of LeftTroll, RightTroll, or Other.
            The easiest way to find the corresponding terms is by reading the header of the corresponding ARFF file, as described below.

         The tweets.csv files are not guaranteed to be in conformant CSV (as the tweet text may contain unescaped quotation marks, and other unusual characters). On the other hand, each line does match the following regular expression:
         /^[0-9]+,[0-9]+,\".*\",([A-Za-z]+|?)$/

    arff: The various CSV files above (except for the tweets) have been converted into a format suitable for use with Weka (http://www.cs.waikato.ac.nz/ml/weka/).
          Each ARFF file contains a number of instances (in CSV), proceeded by a a header. As an example, consider the first few lines of the train-most10.arff file:
@RELATION twitter-troll-most10	# This is simply a name for the dataset.
@ATTRIBUTE tweet-id NUMERIC	# Each of the attributes has a line,
@ATTRIBUTE user-id NUMERIC	# One for the tweet ID, and
@ATTRIBUTE a NUMERIC		# One for the user ID, and
@ATTRIBUTE and NUMERIC		# 10 for the (numeric) token frequencies.
@ATTRIBUTE for NUMERIC
@ATTRIBUTE i NUMERIC
@ATTRIBUTE in NUMERIC
@ATTRIBUTE is NUMERIC
@ATTRIBUTE of NUMERIC
@ATTRIBUTE the NUMERIC
@ATTRIBUTE to NUMERIC
@ATTRIBUTE you NUMERIC		# By convention, the class is given last
@ATTRIBUTE class {LeftTroll,RightTroll,Other}
@DATA				# This is the final line in the header; the instances follow
11,21,0,1,0,0,0,1,0,2,1,0,RightTroll
...

Note that when using development/test data, Weka will insist that the header is _exactly_ the same as the training data. Otherwise, it will refuse to evaluate the model and/or predict the classes of the test instances.
