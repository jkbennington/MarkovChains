Markov Chains
 "What?"
  Shhh. It's okay. Markov Chains are very powerful algorithms that can be used for simple things such as creating realistic data to seed your web app. They can also be used for something as powerful as speech recognition. 

  "How?"
   A simple markov chain (which we'll explore briefly today) would be one that takes in text, learns each word and the word adjacent to the first word, then outputs text based on the probability that one word will follow another.

   "Dictionaries"
   We create dictionaries by indexing words into a hash and giving them values.

   "Frequency Hashes"


   # (GitHub-Flavored) Markdown Editor

   Basic useful feature list:

    * Ctrl+S / Cmd+S to save the file
    * Ctrl+Shift+S / Cmd+Shift+S to choose to save as Markdown or HTML
    * Drag and drop a file into here to load it
    * File contents are saved in the URL so you can share files


   I'm no good at writing sample / filler text, so go write something yourself.

   Look, a list!

    * foo
    * bar
    * baz

   And here's some code! :+1:

   ```javascript
   $(function(){
     $('div').html('I am a div.');
   });
   ```

   This is [on GitHub](https://github.com/jbt/markdown-editor) so let me know if I've b0rked it somewhere.


   Props to Mr. Doob and his [code editor](http://mrdoob.com/projects/code-editor/), from which
   the inspiration to this, and some handy implementation hints, came.

   ### Stuff used to make this:

    * [markdown-it](https://github.com/markdown-it/markdown-it) for Markdown parsing
    * [CodeMirror](http://codemirror.net/) for the awesome syntax-highlighted editor
    * [highlight.js](http://softwaremaniacs.org/soft/highlight/en/) for syntax highlighting in output code blocks
    * [js-deflate](https://github.com/dankogai/js-deflate) for gzipping of data to make it fit in URLs



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