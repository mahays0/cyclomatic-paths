# Cyclomatic basis path / test path generator

Mark Hays<br/>
Rose-Hulman Institute of Technology<br/>
Indigo BioAutomation<br/>

Everyone has heard of McCabe's Cyclomatic Complexity. It is loosely defined as the number of paths through your code. Fewer paths is better. Depending on who you ask, you want 15, 10, or fewer paths through your code.

But did you know that those paths are <b>particular</b> paths? Did you know that McCabe wrote a <a href="http://mccabe.com/pdf/mccabe-nist235r.pdf">NIST Special Publication</a> describing how to enumerate them?

This project for Ruby 2.1.3 implements a variation of McCabe's algorithm for generating what he calls the <b>basis paths</b> of cyclomatic complexity. You can currently use this tool to see to what sort of tests you would need for 100% basis path coverage.

After I get this fully working, what I'd like to do from here is write code instrumentation that determines to what extent basis path coverage has been satisfied by your tests.

## Why Ruby?

I spent a lot of time writing research tools for C++/Java during my Ph.D. studies so I wanted to start something fresh. Also, Indigo likes Ruby.

## Execution

Still working on a better way...

Suppose in your project directory you have a file called hello.rb. Create a symbolic link to the three ruby files (cyclomatic_tests.rb, cyclomatic_complexity.rb, graph_node.rb), then run:

	ruby-rewrite -l cyclomatic_tests.rb hello.rb

This will tell you what test paths go through each function in hello.rb.

## Known bugs

- I haven't implemented all of Ruby's syntax yet.

- The number of paths you get from this tool will likely disagree with the Cyclomatic Complexity metric from your favorite Ruby code quality tool. Whether that is a bug or not is unclear. For instance, rubocop thinks that a rescue block is a decision point, but McCabe is silent on this issue; you can't enter a rescue block through normal control flow. Likewise, rubocop ignores iterator methods like (0..10).each, even though they are really just loops. Is this a bug in rubocop? It's hard to say; iterators don't exist in McCabe's world. The iterators' blocks could alternatively be considered anonymous functions that should have their own CC measure separate from the parent code. Or not.