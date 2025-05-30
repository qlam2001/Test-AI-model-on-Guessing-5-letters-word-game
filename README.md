[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/qAf3sQhg)
# Final Project - Exploring LLM Capabilities

1. Problem Statement
Goal: What are you trying to achieve with the LLM? (1-2 clear sentences.)
The goal of this project is to prompt a large language model (LLM) to play the game of Wordle using words from a predefined dataset. 

Inputs and Outputs: Describe what is given to the model and what it should produce.
Inputs: Feedback in the form of correct letter placements (maybe an ‘O’), correct letters in the wrong place (‘_’), and incorrect letters (‘X’). The set of incorrect letters that the LLM guessed
Outputs: 5 letter word


Connection to Requirements: Why is this project valid for the course? (1-2 sentences.)
This project will help assess the reasoning and adaptability of LLMs in structured word-guessing tasks and could provide insights into optimizing their problem-solving strategies.



2. Dataset
Dataset Name and Source: Name the dataset and where it came from.
Name: Wordle 5 Letter Words
https://www.kaggle.com/datasets/cprosser3/wordle-5-letter-words


Dataset Statistics: Mention #examples, types, input/output lengths, etc. (brief description, bullet points)
Cleaned dataset statistics
Each row/entry of the dataset has 5 columns, each containing a letter in its respective position i.e. column 1 contains first letter, column 2 contains second letter, etc
Percentage of letters that are vowels: 35.40%
Number of rows containing at least one vowel: 998
‘e’ has around a 21% chance of appearing as the 4th letter in a word
‘s’ has around a 30% chance of appearing as the 5th letter in a word
Around 680 words (rows) have 5 distinct letters, around 300 words have 4 distinct letters, and only around 20 words have 3 distinct letters
Only 1 palindrome exists within our dataset, however there’s 51 words with first and last letter matching


Dataset Creation or Changes: If you created or modified the dataset, explain how.
The original dataset has around 2.5k entries so we randomly dropped entries in the dataset so there’s 1k entries left.



3. Prompt Methodology
Prompt Template: Describe the structure (e.g., question → model response).
Structure of prompt
​​Update the current game state with the attempt number (and indicates whether or not it’s a new game).
Show the incorrect letters guessed, previous word guess, and previous word guess feedback.
Give a hint and clear instructions on how to adjust their guess.
Give a note on what ‘X’, ‘O’ and ‘_’ represents as it’s how the feedback is given.
Emphasize to only return a 5 letter word and nothing else
Give an example on how to adjust a guess based on feedback


Sample Input/Output Example:
Input:
We are playing Wordle, try to guess the word within 6 attempts. This is your 3 attempt.

These are incorrect letters that have been guessed: c, r, e, l, u, n

Your previous guess was : taunt   
Previous guess feedback: X_XXO 
Hint : change the letter at index 0, character a should be at index/indices 1, change the letter at index 2, change the letter at index 3, keep the character at index 4 the same

Configure taunt based on the given hint (if applicable)

Notes:
'O' represents a correct letter in the correct position  
'_' represents a correct letter in the wrong position  
'X' represents an incorrect letter

IMPORTANT: YOUR RESPONSE SHOULD ONLY BE A 5-LETTER WORD! DO NOT GIVE ANY REASONING. STRICTLY FOLLOW THE GIVEN HINTS AND RULES

This is an example:
Assume, the word is aback, and your guess was baack. Then, the feedback you get will be "__OOO". Also, you will get some hint. For example, you will get something like: "a should be in position(s) 1 and 3, b should be in position(s) 2” Your job is to correct your previous guess following to the given hint. So in this case, your next guess should be aback since you changed position 1 and 3 with a, position 2 with b, and keep the rest the same

Output: 
aquad

Sampling Parameters:
Model: gpt-4o-mini
Temperature: default
Top-p: default
Max tokens: 100


API Call Description:
OpenAI API targeting gpt 4o mini using client.chat.completions.create() from the openai package



4. Evaluation Approach
Metrics Used: List them and 1-line reason why each was picked.
Number of wins out of 1k games (probability of winning a wordle game)
In order to evaluate if the LLM is good at solving structured word guessing tasks, a straightforward way is to count the number of wins out of 1k games.
Average number of invalid guesses per attempt
The LLM should ideally not make any simple mistakes such as giving 6 letter words as a guess when it’s explicitly stated in the prompt to only return a 5 letter word.
Average number of guesses per attempt
If LLM is good at solving this task, another way we want to evaluate them is based on how many guesses it takes to win the game (the less, the better).
Improvement in guesses
If the LLM isn’t good at solving this task, another way we want to evaluate them is based on whether or not they are actually improving upon their previous guesses using our hints and feedback.


Evaluation Process: How did you run the evaluation? (automatic/human/etc.)
We ran the evaluation automatically since we stored the required data to calculate these metrics in an object per wordle attempt.

Strengths and Weaknesses: Briefly discuss what worked and what didn’t.
The LLM turned out to be horrible at solving this task so the average number of guesses per attempt didn’t turn out to be helpful since it’s close to 6. However, we were able to evaluate the LLM by seeing if the LLM actually improved upon their guesses.



5. Results
Summary of Metrics: Provide scores in a short table or list.

Metrics
Scores
Number of wins out of 1k attempts (games)
59
Number of attempts with invalid guesses
155
Average number of invalid guesses per attempt
0.206
Average number of guesses per attempt
5.904
Number of attempts with increasing feedback values
291
Number of attempts where last feedback value is greater than first
799


Discussion: Interpret what the results mean about your approach.
As shown, out of 1k wordle games, the LLM only won 59 of them, which indicates a bad performance in this type of task, which leads to an average of 5.9 guesses per game.
Although the prompt explicitly told the LLM to only return a 5 letter word, there’s 155 wordle games in which the LLM returned something other than a 5 letter word. Overall, we can observe an average of 0.206 invalid guesses per attempt.
Despite the feedback and explicit hints to tell the LLM what to change, replace, and keep, only 291 wordle games have increasing feedback values (improved guesses). Note that there’s also 201 games where the last guess’ feedback value is actually worse than the first guess’ feedback value.



6. Feedback and Communication
Feedback Received: Summarize the main points of feedback from your draft.
Our mentor gave us positive feedback for our initial proposal draft so we didn’t make any major changes to our project. During our mentor meeting to discuss our LLM outputs, due to the LLM’s poor performance, our mentor suggested that we explore the guesses the LLM made for each wordle attempt and see if we can add an evaluation metric on whether or not the LLM improved upon their guesses.


How You Addressed It: List changes you made based on feedback.
We addressed our mentor’s feedback by assigning values to 'Your guess is not a 5 letter word!', ‘X’, ‘_’, and ‘O’ in our feedback array for each wordle attempt. This way, each wordle attempt now has a feedback value array which stores each guess’ feedback value in that wordle game. 
This is how we assigned the values: 
char_to_val = {'Your guess is not a 5 letter word!': -1, 'X': 0, '_': 1, 'O': 2}

