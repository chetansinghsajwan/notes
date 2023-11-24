Parser is a computer program which accepts the natural language sentence as input and generates an output structure suitable for analysis.
![[/Untitled 12.png|Untitled 12.png]]
  
The parsing technique can be categorized into two types such as
1. Top down Parsing
2. Bottom up Parsing
### Top Down Parsing
Top down parsing starts with the starting symbol and proceeds towards the goal.
In top down parsing words of the sentence are replaced by their categories like verb phrase (VP), Noun phrase (NP), Preposition phrase (PP), Pronoun (PRO) etc.
- **Starts at the top:** Top-down parsing begins with the top-level grammar rule and tries to match the input sentence with this rule.
- **Explores from start symbol:** It starts with the start symbol of the grammar and attempts to transform it into the input sentence by recursively expanding non-terminals into terminals.
- **Recursive Descent Parsing:** Top-down parsing often involves recursive descent parsing, where each grammar rule is associated with a parsing function. These functions recursively call themselves for each non-terminal in the grammar.
- **Predictive Parsing:** Predictive parsing is a specific type of top-down parsing where the parser predicts which production to use by looking at the next input symbol without backtracking.
- **Backtracking is common:** Top-down parsers may need to backtrack if a chosen production does not match the input. This can make them less efficient in some cases.
![[/Untitled 1 3.png|Untitled 1 3.png]]
### Bottom-Up Parsing:
It is also called shift reducing parsing.
In bottom up parsing the construction of parse tree starts at the leaves and proceeds towards the root.
Generally bottom up algorithms are more efficient than top down  
algorithms.
![[/Untitled 2 4.png|Untitled 2 4.png]]
- **Starts at the bottom:** Bottom-up parsing starts with individual input symbols and tries to build up towards the start symbol of the grammar.
- **Matches terminals to form non-terminals:** It identifies sequences of terminals in the input that match the right-hand side of some grammar rule and replaces them with the corresponding non-terminal symbol.
- **Shift-Reduce Parsing:** Bottom-up parsing often involves shift-reduce parsing, where the parser shifts input symbols onto a stack until it can reduce them to a higher-level construct (non-terminal) according to a grammar rule.
- **Reduce-Reduce Conflict:** Bottom-up parsing can face reduce-reduce conflicts, where the parser has to decide which production to reduce when there are multiple applicable rules. This requires additional techniques like associativity and precedence rules.
- **Efficient for a wide range of grammars:** Bottom-up parsers are generally more powerful and can handle a broader class of grammars without modification.
- **No backtracking:** Unlike top-down parsing, bottom-up parsing doesn't involve backtracking, making it more efficient in certain scenarios.
### Deterministic Parsing
A deterministic parser is one which permits only one choice for each word  
category.
That means there is only one replacement possibility for every word  
category.
In deterministic parsing back tracking to some previous positions is not possible. Always the parser has to move forward.
### Non Deterministic Parsing
The non deterministic parsing allows different arcs to be labeled with the some  
test. Thus, they can uniquely make the choice about the next arc to be taken. In non  
deterministic parsing, the back tracking procedure can be possible. Suppose at  
some extent of point, the parser does not find the correct word, then at that stage it  
may backtracks to some of its previous nodes and then start parsing.