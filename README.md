  # Markov Chains

  ## What?:

  Shhh. It's okay. Markov Chains are very powerful algorithms that can be used for simple things such as creating realistic data to seed your web app. They can also be used for something as powerful as speech recognition. 

  **The important thing to remember is that Markov Chains use the current state of something to predict the next state.** 

  ## "How?":

   A simple markov chain (which we'll explore briefly today) would be one that takes in text, learns each word and the word adjacent to the first word, then outputs text based on the probability that one word will follow another.

   ## "Dictionaries":

   We create dictionaries by indexing words into a hash and giving them values.

   ## "Frequency Hashes":


   I'm no good at writing sample / filler text, so go write something yourself.

   Look, a list!

    * 
    * bar
    * baz



   Props to Mr. Doob and his [code editor](http://mrdoob.com/projects/code-editor/), from which
   the inspiration to this, and some handy implementation hints, came.

 



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
      File.read("Agatha Christie - The Mysterious Affair at Styles.txt")
    )

    sentence = ""
    word = "Murder"
    until sentence.count(".") == 4
      sentence << word << " "
      word = mc.get(word)
    end
    puts sentence << "\n\n"


  ## Sources:

     * http://rubyquiz.com/quiz74.html
     