---
toc: true
title: How Rules as Code Makes Laws Better
description: Drafting the Rock Paper Scissors Act in s(CASP)
date: '2021-05-15T15:15:51.201Z'
categories: []
keywords: [scasp]
slug: /@roundtablelaw/how-rules-as-code-makes-laws-better-115ab62ab6c4
---

I want to give you an intuition for why Rules as Code in logical tools like s(CASP) makes laws better.

Let’s imagine that you are the legal knowledge engineer assigned to the Rules as Code process for drafting the Rock Paper Scissors Act of 2021. The purpose of the Rock Paper Scissors Act is to set out the official rules of who is the winner of a game of Rock Paper Scissors.

### The Draft

We need to start with a proposed natural language draft of the law. So here is a first attempt that we can play with.

> 1\. The winner of a game of rock paper scissors is the player in the game who throws a sign that beats the sign of another player in the game.

> 2\. Rock beats Scissors, Scissors beats Paper, and Paper beats Rock.

### The Procedure

1.  Start small.
2.  Check to see if your law behaves as expected.
3.  If not, figure out why, and fix it.
4.  If anything else needs to be added, start over at 1.
5.  Do verification to learn how the rules behave.
6.  If it behaves improperly, fix it.
7.  If it behaves in ways that you can’t categorize as proper or improper, go back to the client for more instructions.

I’m going to take you through 5 iterations of steps 1 through 4, and a little of step 5.

### First Iteration

We are going to start super small. We’re going to start with the conclusion of rule 1, that a player wins a game, but we are going to start only with the requirement that the player is “in the game”.

For that we are going to need two terms in our vocabulary:
```prolog
#pred player_in_game(Player,Game) :: '@(Player) participated in @(Game)'.  
#pred winner_of_game(Player,Game) :: '@(Player) is the winner of @(Game)'.
```
These `#pred` statements tell s(CASP) how to translate your code back into English once s(CASP) has turned your rules and your facts into an answer and an explanation.

Now we need our rule, that a person is the winner of a game if they are a player in that game.
```prolog
winner_of_game(Player,Game) :-  
  player_in_game(Player,Game).
```
Now if we want to test our rule, could create some data. But that would very quickly get onerous as the rule gets more complex. To test it completely, we would need to create a different test for each possible combination of inputs.

Instead, we are going to tell s(CASP) that it should just consider both the possibility that a statement is true, and the possibility that a statement is false. We do that with an `#abducible` statement, like this:
```prolog
#abducible player_in_game(Player,Game).
```
Note that we don’t want s(CASP) to hypothesize about `winner_of_game`, because that is the question that we are going to be asking to see if the code works, right now.

Now we have enough to run our first test, so we ask s(CASP) to describe all of the ways in which a game might be won by posing this query:
```prolog
?- winner_of_game(Player,Game).
```
When I say we run a query, what I mean is that you put all of the above text into a file, give it a name like `rps.pl`, and then run the command `scasp rps.pl --human --tree -s0` . That tells s(CASP) to load that file, execute the query it includes, find all the answers it can, and explain them in English.

When we run that command, we get the following output:
```
QUERY:I would like to know if  
     Player is the winner of Game.

JUSTIFICATION_TREE:  
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game
```
(Note: I’m editing a lot of extraneous information from the output to make it more readable.)

So the explanation is “Player is the winner of Game, because Player participated in Game, because that is assumed.” There is only one answer, so there is only one way to be the winner of game. So that matches what we were expecting.

Congratulations. We have finished our first iteration, and we will move on to adding something else.

### Second Iteration

According to section 1 of our draft, in order to be the winner of a game, you have to throw a sign. So let’s add that as a piece of vocabulary:
```prolog
#pred player_threw_sign(Player,Sign) :: '@(Player) threw @(Sign)'.
```
Now we will modify the rule to require you to throw a sign.
```prolog
winner_of_game(Player,Game) :-  
  player_in_game(Player,Game),  
  player_threw_sign(Player,Sign).
```
And we will tell s(CASP) that it can just make assumptions about whether or not a player threw a sign, this way:
```prolog
#abducible player_threw_sign(Player,Sign).
```
If we run the query again, we get one possible answer:
```
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game  
    Player threw Var2, because  
        it is assumed that Player threw Var2
```
If we are getting one answer, you might wonder how many scenarios did s(CASP) consider? At this point, the answer is four. We have given it two abducibility statements, each of which creates two possibilities, either the statement is true, or the statement is false. Two statements, with two possibilities each, gives four scenarios. And in only one of those scenarios was it true that there was a winner of a game.

### Third Iteration

Next let’s add the idea from section 1 of the draft that in order to win a game, your sign has to beat another player’s sign.
```prolog
#pred sign_beats_sign(Sign,OtherSign) :: '@(Sign) beats @(OtherSign)'.
```
And we can update the rule:
```prolog
winner_of_game(Player,Game) :-  
  player_in_game(Player,Game),  
  player_threw_sign(Player,Sign),  
  sign_beats_sign(Sign,OtherSign).
```
We can also add an abducibility statement about whether or not one sign beats another sign.
```prolog
#abducible sign_beats_sign(Sign,OtherSign).
```
s(CASP) is now considering 8 scenarios, but there is still only one answer to our query:
```
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game  
    Player threw Var2, because  
        it is assumed that Player threw Var2  
    Var2 beats Var3, because  
        it is assumed that Var2 beats Var3
```
So that is still behaving as expected. You might wonder why, if we called it “Sign” and “OtherSign” in the rule, does s(CASP) turn those names into “Var2” and “Var3” in the explanation? The reason is that the explanation is only aware of the variable names that are in your query. The variable names in the rules are invisible to it.

### Fourth Iteration

This time, let’s add section 2.

For this, we don’t need to add any vocabulary, or any rules, or any abduction statements. Here, we just need to add three facts.
```prolog
sign_beats_sign(rock,scissors).  
sign_beats_sign(scissors,paper).  
sign_beats_sign(paper,rock).
```
Because we don’t want anything else to be possible, we will also delete the line that makes `sign_beats_sign` abducible, so that only these three facts will be available to satisfy that requirement in section 1.

What do you think will happen if we run the query?
```
QUERY:I would like to know if  
     Player is the winner of Game.

ANSWER: 1  
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game  
    Player threw rock, because  
        it is assumed that Player threw rock

ANSWER: 2  
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game  
    Player threw scissors, because  
        it is assumed that Player threw scissors

ANSWER: 3  
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game  
    Player threw paper, because  
        it is assumed that Player threw paper
```
There are now three ways to win a game, one for each of the possible signs that the winner can throw. Makes sense, right?

### An Aside

Let’s take a second here to notice what just happened. We have encoded part of section 1, and all of section 2. If you read our code for section 1, we did not need to convert section 1 into an algorithm, or a function, or an object, or anything else. Section 1 was originally written as a rule in English, and we just made it a rule written in s(CASP).

Likewise, section 2 is stating facts that are always true, and can be used while trying to figure out the implications of the other sections of the draft act. And when we translated it we just re-wrote the facts in s(CASP).

We didn’t have to change the form, just the language.

Why is that possible, and why is it important?

It’s possible because s(CASP) has an underlying semantics of formal logic, and _so does law_.

> If you already have natural language rules you are encoding, or you need to write natural language rules to match an encoding, your job is much easier when you use a computer language that is based on logic.

It is important for a lot of reasons, but one major reason is how simple it is to turn rules into an application.

Look at how much work we did, after we had translated the rules, to be able to learn what the computer thought the rules meant.

We wrote _three lines of code._
```prolog
#abducible player_threw_sign(Player,Sign).  
#abducible player_in_game(Player,Game).  
?- winner_of_game(Player,Game).
```
In fact, you can translate those lines of code, back into English, too. “Assuming that a player may or may not have thrown any particular sign, and assuming that a player may or may not be a participant in a game, under what possible scenarios is it true that a player won the game?”

> It is _also easier_ to translate questions, facts, and even uncertainty about facts into logical programming languages, the combination of which turn your rules into an application.

Logical programming languages make it easier to use laws, and facts, and questions, as the source code for applications.

### Fifth Iteration

Let’s add the requirement from section 1 that there actually be a “sign of the other player” to beat. We just modify the rule to read like this:
```prolog
winner_of_game(Player,Game) :-  
  player_in_game(Player,Game),  
  player_threw_sign(Player,Sign),  
  player_in_game(OtherPlayer,Game),  
  player_threw_sign(OtherPlayer,OtherSign),  
  sign_beats_sign(Sign,OtherSign).
```
What should we see? Well, each of the three possibilities should now include another player, and what the other player threw should be the thing that is beaten by what the winner threw. Let’s see what happens:
```
QUERY:I would like to know if  
     Player is the winner of Game.

ANSWER: 1  
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game  
    Player threw rock, because  
        it is assumed that Player threw rock  
    Var2 participated in Game, because  
        it is assumed that Var2 participated in Game  
    Var2 threw scissors, because  
        it is assumed that Var2 threw scissors

ANSWER: 2  
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game  
    Player threw scissors, because  
        it is assumed that Player threw scissors  
    Var2 participated in Game, because  
        it is assumed that Var2 participated in Game  
    Var2 threw paper, because  
        it is assumed that Var2 threw paper

ANSWER: 3  
Player is the winner of Game, because  
    Player participated in Game, because  
        it is assumed that Player participated in Game  
    Player threw paper, because  
        it is assumed that Player threw paper  
    Var2 participated in Game, because  
        it is assumed that Var2 participated in Game  
    Var2 threw rock, because  
        it is assumed that Var2 threw rock
```
It works! If “Player” threw paper, “Var2” threw rock, etc.

### A Wrinkle or Two

These answers are a little deceptive, because Player and Var2 are variables with different names. But using different variable names in s(CASP) doesn’t imply that the things they refer to are different things.

What happens if we change the abduction rules so that the only thing we can assume about which players threw which sign is that “bob” threw something?
```prolog
#abducible player_threw_sign(bob,Sign).
```
`bob`, because it is lower-case, refers to a specific thing, and anywhere you use `bob` across any rules and facts it always refers to the same thing. `Sign` , because it is upper case, is a variable, and can refer to anything.

So how many ways of winning should there be if there is only possibly one player, named bob? How many answers do we expect?

Perhaps surprisingly, the answer is still three! Let’s take a look at the first explanation to see why.
```
bob is the winner of Game, because  
    bob participated in Game, because  
        it is assumed that bob participated in Game  
    bob threw rock, because  
        it is assumed that bob threw rock  
    bob participated in Game, because  
        it is assumed that bob participated in Game  
    bob threw scissors, because  
        it is assumed that bob threw scissors
```
So s(CASP) was considering a situation where a player beat themselves because they were the `Player`, _and_ they were the `OtherPlayer`, and they threw two different signs!

That’s clearly not what we wanted. Again, inside a rule, using the same variable name twice means you are referring to the same thing. But, using different variable names does not mean that you are referring to different things. So even if you use the names “Player” and “OtherPlayer” you might be referring to the same thing twice. In this case, “bob”.

#### Making Disunity Explicit

So let’s add a line to the rule that actually imposes the condition “Player and OtherPlayer should not be the same thing.” In logical language, that’s called a “disunity”, and it is expressed in s(CASP) with the `\=` operator.
```prolog
winner_of_game(Player,Game) :-  
  player_in_game(Player,Game),  
  player_threw_sign(Player,Sign),  
  player_in_game(OtherPlayer,Game),  
  player_threw_sign(OtherPlayer,OtherSign),  
  sign_beats_sign(Sign,OtherSign),  
  Player \= OtherPlayer.
```
Note that we are not adding anything to what is written in section 1 of the draft. The the semantic meaning of “another player” in section 1 is that it should be a distinct entity from the “player”. We’re just translating that implied semantic meaning into s(CASP).

Now, we should get zero models, because we have told s(CASP) that it is only allowed to assume that “bob” threw signs. And if we run the same query again, that’s what happens!

Now if we change the abducibility statement back to the line below, we would like to be getting three answers again.
```prolog
#abducible player_threw_sign(Player,Sign).
```
But if we make that change and run the code, we still get zero models. Why is that?

Essentially, it’s because s(CASP) can’t do disunity across abducible entities in the conditions of a rule. I’m genuinely not sure why, because it does seem to be able do disunity across abducible entities in a query. But no worries, there’s a relatively easy workaround.

#### Quickly Generating Instances

Create a file called `example.pl` and include the following:
```prolog
example(0).  
example(1).  
example(2).  
... 
```
continuing for as long as you need. Now in we can include that code in our program by adding a line like this at the top.
```prolog
#include 'example.pl'.
```
Now we can create a way to quickly say “for testing purposes, create X objects of this type” by adding this rule to our code:
```prolog
player(test_player(X)) :- example(X), number_of_players(N), X .<. N.
```
Now, we can add a single fact, right above our query in the code, which reads:
```prolog
number_of_players(2).
```
That will create two players in every scenario, `test_player(0)` and `test_player(1)` . And if you want to change the number of players that you are testing against, you just change that one line before you run your query.

We now need to modify our rule to include the fact that both “Player” and “OtherPlayer” refer to a specific player entity, as follows:
```prolog
winner_of_game(Player,Game) :-  
  player(Player),  
  player(OtherPlayer),  
  player_in_game(Player,Game),  
  player_threw_sign(Player,Sign),  
  player_in_game(OtherPlayer,Game),  
  player_threw_sign(OtherPlayer,OtherSign),  
  sign_beats_sign(Sign,OtherSign),  
  Player \= OtherPlayer.
```
We can also add the term `player` to our vocabulary, like this:
```prolog
#pred player(X) :: '@(X) is a player'.
```
Now we can run our query again, and we might expect to get three results again.

But what we actually get is 6!

Now what!?
```
QUERY:I would like to know if  
     Player is the winner of Game.

ANSWER: 1  
test_player(0) is the winner of Game, because  
    test_player(0) is a player, because  
        'example' holds (for 0).  
    test_player(1) is a player, because  
        'example' holds (for 1).  
    test_player(0) participated in Game, because  
        it is assumed that test_player(0) participated in Game  
    test_player(0) threw rock, because  
        it is assumed that test_player(0) threw rock  
    test_player(1) participated in Game, because  
        it is assumed that test_player(1) participated in Game  
    test_player(1) threw scissors, because  
        it is assumed that test_player(1) threw scissors  
    rock beats scissors.

ANSWER: 2  
test_player(0) is the winner of Game, because  
    test_player(0) is a player, because  
        'example' holds (for 0).  
    test_player(1) is a player, because  
        'example' holds (for 1).  
    test_player(0) participated in Game, because  
        it is assumed that test_player(0) participated in Game  
    test_player(0) threw scissors, because  
        it is assumed that test_player(0) threw scissors  
    test_player(1) participated in Game, because  
        it is assumed that test_player(1) participated in Game  
    test_player(1) threw paper, because  
        it is assumed that test_player(1) threw paper  
    scissors beats paper.

ANSWER: 3  
test_player(0) is the winner of Game, because  
    test_player(0) is a player, because  
        'example' holds (for 0).  
    test_player(1) is a player, because  
        'example' holds (for 1).  
    test_player(0) participated in Game, because  
        it is assumed that test_player(0) participated in Game  
    test_player(0) threw paper, because  
        it is assumed that test_player(0) threw paper  
    test_player(1) participated in Game, because  
        it is assumed that test_player(1) participated in Game  
    test_player(1) threw rock, because  
        it is assumed that test_player(1) threw rock  
    paper beats rock.

ANSWER: 4  
test_player(1) is the winner of Game, because  
    test_player(1) is a player, because  
        'example' holds (for 1).  
    test_player(0) is a player, because  
        'example' holds (for 0).  
    test_player(1) participated in Game, because  
        it is assumed that test_player(1) participated in Game  
    test_player(1) threw rock, because  
        it is assumed that test_player(1) threw rock  
    test_player(0) participated in Game, because  
        it is assumed that test_player(0) participated in Game  
    test_player(0) threw scissors, because  
        it is assumed that test_player(0) threw scissors  
    rock beats scissors.

ANSWER: 5  
test_player(1) is the winner of Game, because  
    test_player(1) is a player, because  
        'example' holds (for 1).  
    test_player(0) is a player, because  
        'example' holds (for 0).  
    test_player(1) participated in Game, because  
        it is assumed that test_player(1) participated in Game  
    test_player(1) threw scissors, because  
        it is assumed that test_player(1) threw scissors  
    test_player(0) participated in Game, because  
        it is assumed that test_player(0) participated in Game  
    test_player(0) threw paper, because  
        it is assumed that test_player(0) threw paper  
    scissors beats paper.

ANSWER: 6  
test_player(1) is the winner of Game, because  
    test_player(1) is a player, because  
        'example' holds (for 1).  
    test_player(0) is a player, because  
        'example' holds (for 0).  
    test_player(1) participated in Game, because  
        it is assumed that test_player(1) participated in Game  
    test_player(1) threw paper, because  
        it is assumed that test_player(1) threw paper  
    test_player(0) participated in Game, because  
        it is assumed that test_player(0) participated in Game  
    test_player(0) threw rock, because  
        it is assumed that test_player(0) threw rock  
    paper beats rock.
```
If you read the results carefully, 6 results actually makes sense.

Now that we have two concrete players, there is a difference between saying that player(0) beat player(1), and saying that player(1) beat player(0). When we were dealing with players in the abstract, that wasn’t true. So there are three ways to win a game, and two players who might win, for a total of 6 models.

### Formal Verification of Draft Legislation

Now that we have a working encoding, that we can test with any number of players, let’s see what we can find out about the rules to Rock Paper Scissors as we have drafted them.

For instance, we might want to know whether it is possible for all of the players in a game to also be winners of the game. We imagine that we have been told by our clients that this shouldn’t be possible.

Expressing this question in s(CASP) is slightly complicated, because s(CASP) doesn’t have a universal quantifier. So instead of saying “every X is Y”, you need to say “there is no evidence of an X that is not Y”.

So “every game has a loser” becomes “there is no case in which there is a game with no loser. So we first define “there is a loser”, and then we query “it is not true that there is a loser.”
```prolog
loser :- player(P), player_in_game(P,G), not winner_of_game(P,G).

?- not loser.
```
It turns out that this query doesn’t work. Why not? Because our definition of `loser` is “unsafe”. Specifically, in that rule, `G` is a free variable under negation in the conditions of the rule. That means it does not appear in the conclusion, and it appears as part of a negation in the conditions.

Free variables under negation is a thing that logic programming languages can’t deal with, because “under the hood” it sends the software on a wild goose chase.

We can solve that problem by “grounding” games, making `G` no longer “free”. Grounding is actually the same thing that we did to players earlier. So we take the exact same steps.

We modify the rule to add a statement that requires that the game exists:
```prolog
winner_of_game(Player,Game) :-  
  game(Game),  
  player(Player),  
  player(OtherPlayer),  
  player_in_game(Player,Game),  
  player_threw_sign(Player,Sign),  
  player_in_game(OtherPlayer,Game),  
  player_threw_sign(OtherPlayer,OtherSign),  
  sign_beats_sign(Sign,OtherSign),  
  Player \= OtherPlayer.
```
Then we add a rule to create concrete example game objects, just like we did for players.
```prolog
game(test_game(X)) :- example(X), number_of_games(N), X .<. N.
```
And then we add a line just above the query to set the number of games.
```prolog
number_of_games(1).
```
Now when we run the query, it works, and returns “no models.”

Which means it is never true that there is a game with two players in which there is no player who is not a winner. Which in turn means there is no two-player game where all the players win. Which was the result we wanted.

#### Testing gives Evidence, Formal Verification gives Proof

What we just did was formal verification, not testing. Based on the result we got from the program above, we can say categorically that there is no two player game of Rock Paper Scissors under these rules where all the players win.

The conclusion itself is unimpressive, because the rules are not complicated. What is impressive is that the process is automated, and can be applied to much more complicated problems. Problems beyond the human capacity to analyse in their heads.

Consider a person who wants to know whether the amendments to a tax act increase anyone’s taxes. You can test your encoding, and that will give you scenarios in which it does not. And maybe with enough of those scenarios, and sufficiently representative scenarios, that is sufficient evidence to be reasonably confident that the amendment _probably_ doesn’t increase anyone’s taxes.

But if we can do formal verification as easily as we can do testing, or perhaps even easier, why wouldn’t we?

#### NGL, It’s Complicated

This process, with all the warts included, is an honest representation of the reality of trying to do things like this with s(CASP) at the moment.

It’s not “easy”. But it is, in most cases, “easier” than the alternatives. And if you encode your law in s(CASP) once, you get a tool that can answer many questions. In many non-logical tools, you get a tool that can answer only one. And in many logical tools you do not have the ability to access explanations, or you don’t have the ability to access multiple explanations for the same conclusion, or you don’t have the ability to access explanations for why things are not true, all with no additional coding required.

So the return on investment for s(CASP) in Rules as Code is higher than for many other tools, and grows exponentially with the complexity of the law, and how frequently it needs to be updated.

But high ROI doesn’t mean the investment is low.

Unfortunately, because of bugs in s(CASP), and because of the sometimes combinatorially-explosive nature of logic programming, and because of quirks of the operational semantics of logic programming like free variables under negation, there will be things that just don’t work, or don’t work fast enough. When that happens, you need to reformulate your code to create an equivalent version that will answer the same question.

So the bad news is that right now, using s(CASP) and similar tools effectively requires a lot of learning, and that learning is difficult to obtain. My passion is reducing the amount of learning required, making the remaining learning as easy to achieve as possible, and demonstrating the potential of the approach so that people will help.

### Speaking of Bugs…

What happens if we change the number of players the model knows about to 3?

If we set `number_of_players(3).` and query `not loser`, the software goes into an infinite loop. (I’m pretty sure. I didn’t actually wait for infinity to find out.) Which probably means that there is a bug in s(CASP) somewhere, but it also suggests that this is not as simple a question as when the number of players was 1.

As a work-around, we can try to find a counter example ourselves with a more complicated query. Instead of querying `not loser`, we can directly write a query that means “is there a scenario in which there are three different players, each of which is the winner of the same game?”
```prolog
?- player(A), player(B), player(C), A \= B, B \= C, C \= A, winner_of_game(A,Game), winner_of_game(B,Game), winner_of_game(C,Game).
```
If we run this query with the above code in the usual way, we discover a different version of what is probably the same bug… s(CASP) instantly starts printing out a (seemingly) infinite list of scenarios in which this is true.

We can work around that version of the problem by telling s(CASP) we are only interested in one answer, by saying `-s1` in the command line. If we do that, here’s the first answer we get:
```
QUERY:I would like to know if  
     A is a player and  
     B is a player and  
     C is a player and  
     A is not equal B and  
     B is not equal C and  
     C is not equal A and  
     A is the winner of Game and  
     B is the winner of Game and  
     C is the winner of Game.

ANSWER: 1  
test_player(0) is a player, because  
    'example' holds (for 0), and  
    'number_of_players' holds (for 3).  
test_player(1) is a player, because  
    'example' holds (for 1), and  
    'number_of_players' holds (for 3).  
test_player(2) is a player, because  
    'example' holds (for 2), and  
    'number_of_players' holds (for 3).  
test_player(0) is the winner of test_game(0), because  
    test_game(0) is a game, because  
        'example' holds (for 0), and  
        'number_of_games' holds (for 1).  
    test_player(0) is a player, justified above, and  
    test_player(1) is a player, because  
        'example' holds (for 1), and  
        'number_of_players' holds (for 3).  
    test_player(0) participated in test_game(0), because  
        it is assumed that test_player(0) participated in test_game(0)  
    test_player(0) threw rock, because  
        it is assumed that test_player(0) threw rock  
    test_player(1) participated in test_game(0), because  
        it is assumed that test_player(1) participated in test_game(0)  
    test_player(1) threw scissors, because  
        it is assumed that test_player(1) threw scissors  
    rock beats scissors.  
test_player(1) is the winner of test_game(0), because  
    test_game(0) is a game, justified above, and  
    test_player(1) is a player, justified above, and  
    test_player(0) is a player, because  
        'example' holds (for 0), and  
        'number_of_players' holds (for 3).  
    test_player(1) participated in test_game(0), justified above, and  
    test_player(1) threw rock, because  
        it is assumed that test_player(1) threw rock  
    test_player(0) participated in test_game(0), justified above, and  
    test_player(0) threw scissors, because  
        it is assumed that test_player(0) threw scissors  
    rock beats scissors.  
test_player(2) is the winner of test_game(0), because  
    test_game(0) is a game, justified above, and  
    test_player(2) is a player, justified above, and  
    test_player(0) is a player, because  
        'example' holds (for 0), and  
        'number_of_players' holds (for 3).  
    test_player(2) participated in test_game(0), because  
        it is assumed that test_player(2) participated in test_game(0)  
    test_player(2) threw rock, because  
        it is assumed that test_player(2) threw rock  
    test_player(0) participated in test_game(0), justified above, and  
    test_player(0) threw scissors, because  
        it is assumed that test_player(0) threw scissors  
    rock beats scissors.
```
So you can see, if there are three players, and each of them throws a different sign, it is possible for there to be no losers, and for everyone to be a winner according to our rules.

Whoops!

### Back to the (Legislative) Drafting Board

So what we learn from this formal verification exercise is that our draft law has a bug when the number of players goes to 3. Further testing would reveal that there is no winner of a 1-player game under these rules, either.

Now we would revise, and go back to our client seeking instructions for how they want ties, and one-player games to be treated, etc. We come up with new ideas for the rules, encode those, re-run the verifications, lather, rinse, and repeat.

Eventually, maybe our draft act looks something like this:

> 1. The winner of a game of rock paper scissors is:

> a) a player in that game, if they are the only player in that game, or

> b) a player whose sign beats the signs of all other players in that game, or

> c) the winner of a new run-off game among the players whose sign was not beaten by any other player, or

> d) the winner of a run-off game of rock paper scissors among players in a tied game.

> 2. A game of rock paper scissors is tied if:

> a) all players throw the same sign, or

> b) all players throw a sign that is defeated by a sign thrown by another player, and that defeats a sign thrown by another player.

> 3. Rock beats Scissors, Scissors beats Paper, and Paper beats Rock.

Encoding and formal verification in a Rules as Code process with tools like s(CASP) can make it easier to learn more about how the rules you wrote actually behave. That in turn makes it easier to find places they do not behave as expected, ways to make them behave as expected, and areas where you are not certain what behaviour was intended.

These are not new ideas. All of these things are being done, right now, as a part of every legislative drafting effort. The only difference is that they are being done by people, _without the help of computers_.

If we can give drafters that help, we make it easier to get better laws that are easier to automate, and the downstream effects of that for fairness, consistency, efficiency, and access to justice are enormous.