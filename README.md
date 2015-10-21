# Markov Chains

## What?:

   Markov Chains are very powerful algorithms that can be used for simple things such as creating realistic data to seed your web app. They can also be used for something as powerful as speech recognition. 

## Mental Note:
  **The important thing to remember is that Markov Chains use the current state of something to predict the next state.** We'll explore this more later.

## How?:

   A simple markov chain (which we'll explore briefly today) would be one that takes in text, learns each word and the word adjacent to the first word, then outputs text based on the probability that one word will follow another.

## Dictionaries:

   We create dictionaries by indexing words into a hash and giving them values.

## Frequency Hashes:

   Adds a count to how frequent something occurs. In this case we'll explore we're checking how frequently one word comes after another. 

## Models/Order of:
   Models set the amount of characters or words that we want to use as keys and we can determine how to do so by something called *order of*. Here's an example of three words with an order of 2 with the words "the these them" provided as three separate inputs to simplify this example.

### Example:

|     Keys      |    Values     |   
| ------------- | ------------- | 
|      Th       | [e, e, e]     |
|      he       | [s, m ]       |
|      em       | []            |
|      es       | [e]           | 
|      se       | []            |

  The keys take two letters because the model is to the order of 2, then stores the following letter as a value and shifts over 1 character to set a new key and repeat the process.


### Results:

|     Keys      |    Values     |   Frequency          | Chance of value coming after Key | 
| ------------- | ------------- | -------------------  | -------------------------------- | 
|      Th       | [e, e, e]     | rand(0..value.length)|  e = 3/3 = 100%                  |   
|      he       | [s, m ]       | rand(0..value.length)|  s = 1/2 = 50% , m = 1/2 = 50%   |   
|      es       | [e]           | rand(0..value.length)|  e = 1/1 = 100%                  |
|      em       | []            | rand(0..value.length)|                                  | 
|      se       | []            | rand(0..value.length)|                                  |


   Once all the keys and values have been stored we check the frequency of these letters by checking how often they appear as a stored value. 

#### So now what?
   Remember that **mental note** we took? If "Th" is the *current state* the chance that the next state is "he" is what percent? Since we know "e" comes after the key "Th" 100% of the time we can infer that the *next state* will be "he".


 
##Ruby Example
   This example generates new text based on the text that is input on initialize. Let's take a look:
```ruby

    class MarkovChain
      def initialize(text)
        @words = Hash.new
        wordlist = text.split
        wordlist.each_with_index do |word, index|
          add(word, wordlist[index + 1]) if index <= wordlist.size - 2
        end
      end

      def add(word, next_word)
        @words[word] = Hash.new(0) if !@words[word]
        @words[word][next_word] += 1
      end

      def get(word)
        return "" if !@words[word]
        followers = @words[word]
        sum = followers.inject(0) {|sum,kv| sum += kv[1]}
        random = rand(sum)+1
        partial_sum = 0
        next_word = followers.find do |word, count|
          partial_sum += count
          partial_sum >= random
        end.first
        next_word
      end
    end

    mc = MarkovChain.new(
      File.read("DorianGray.txt")
    )

    sentence = ""
    word = "Picture"
    until sentence.count(".") == 4
      sentence << word << " "
      word = mc.get(word)
    end
    puts sentence << "\n\n"
```
## What does it all mean?
   Let's do a general overview. In def initialize we create a hash, split the text on whitespace, index each word and invoke the add method. The add method then checks if the word already exists and creates a frequency hash adding the following word as a value. Finally in the def get we find our current word index and look for all the words that are stored as values taking the frequencies of each and checking them against a random number that's equal to our total stored words(*remember our rand(1..value.length) example*).


## Sources:

     * http://rubyquiz.com/quiz74.html
     * http://www.cs.princeton.edu/courses/archive/spr05/cos126/assignments/markov.html