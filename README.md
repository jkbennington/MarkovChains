# Markov Chains

## What?:

  Shhh. It's okay. Markov Chains are very powerful algorithms that can be used for simple things such as creating realistic data to seed your web app. They can also be used for something as powerful as speech recognition. 

  **The important thing to remember is that Markov Chains use the current state of something to predict the next state.** 

## How?:

   A simple markov chain (which we'll explore briefly today) would be one that takes in text, learns each word and the word adjacent to the first word, then outputs text based on the probability that one word will follow another.

## Dictionaries:

   We create dictionaries by indexing words into a hash and giving them values.

## Frequency Hashes:

   Adds a count to how frequent something occurs. In this case we'll explore we're checking how frequently one word comes after another. 

## Models/Order of:
   Models set the amount of characters or words that we want to use as keys and we can determine how to do so by something called *order of*. Here's an example of three words with an order of 2 with the words "the these them" provided as input.
### Example:

```
   |     Keys      |    Values     |   
   | ------------- | ------------- | 
   |      Th       | [e, e, e]     |
   |      he       | [s, m ]       |
   |      es       | [e]           |          
```

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |


### Basic Test:

```
   |     Keys      |    Values     |   Frequency          | Chance of value coming after Key | 
   | ------------- | ------------- | -------------------  | -------------------------------- | 
   |      Th       | [e, e, e]     | rand(0..value.length)|  e = 3/3 = 100%                  |   
   |      he       | [s, m ]       | rand(0..value.length)|  s = 1/2 = 50% , m = 1/2 = 50%   |   
   |      es       | [e]           | rand(0..value.length)|  e = 1/1 = 100%                  |
```

  The keys take two letters because the model is to the order of 2, then stores the following letter as a key and shifts over 1 character to set a new key and repeat the process. Once all the keys and values have been stored we check the frequency of these letters by checking how often they appear as a stored value. 


 
##Ruby Example

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
    word = "Murder"
    until sentence.count(".") == 4
      sentence << word << " "
      word = mc.get(word)
    end
    puts sentence << "\n\n"
```

## Sources:

     * http://rubyquiz.com/quiz74.html
     * http://www.cs.princeton.edu/courses/archive/spr05/cos126/assignments/markov.html