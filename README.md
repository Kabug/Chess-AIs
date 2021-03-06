# Chess-AIs

## 4 Different Chess AIs. 

 * [Generic AI](#generic-ai)
 * [Minimax](#minimax)
 * [Minimax + Iterative Deepening](#minimax-with-iterative-deepening)
 * [Monte Carlo](#monte-carlo-tree-search)


The MonteCarlo Algorithm needs training so feel free to run it to update the database :)

## Generic AI
It randomly selects moves from all possible moves it has available.

## Minimax
The MiniMax AI I implemented was very similar to the one the one commonly used for TicTacToe.
The only significant difference between the two was having to implement a utility function that wasn’t “WIN” or “LOSE” since the amount of depth it would take to reach those states would take way too long compared to TicTacToe that no human would be able to sit patiently to wait for it. So, I created a utility function that weighed the total value of all the pieces on each side and subtracted whites pieces from black. The image below shows the weights for each piece. The king is 39 as it’s 1 more than all the pieces combined.

![Source: Wikipedia](https://i.imgur.com/i2FueCI.png)

*Source: Wikipedia*

This means that the white side would want to keep the number as small as possible by making sure it’s piece count total is greater and blacks piece count total is smaller and black would want vice versa. Although this AI is considerably better than the GenericAI that randomly moves, it is now able to take trades and defend it’s pieces. Unfortunately though, due to how simple I made the utility function that’s all it’s able to do. It doesn’t have tactics, it doesn’t know how to control the center and it does a lot of wasted moves. Instead of trying to move pawns forward, it’ll move the rook back and forth. The AI plays quite reactively instead of proactively. 


## Minimax with Iterative Deepening
The Iterative Deepening AI is supposed to use Iterative Deepening to find the best move within a certain amount of time. Each time it calls a function with an ever increasing depth until it runs out of time. Once it runs out of time, it chooses the move with the most depth to use. For example, you simulate all the game states that black will have at depth 1 then depth 2 etc. Then find the best states for black at those depths. I implemented my Iterative Deepening AI with a combination of Minimax. I looked online and it seemed that it was a popular implementation to improve Minimax so I decided to go with that approach. Weirdly enough however, the implementation I found was actually worse than the base MiniMax function in the early game. I gave the AI 15 seconds to think, which unfortunately meant that if it had a lot of options for possible moves, it wouldn’t be able to reach the same amount of depth as the base minimax. However, when there were less possible moves, especially in late game it was able to go to depth 5 and even 7 consistently.

## Monte Carlo Tree Search
The Monte Carlo Tree Search Algorithm AI took me the longest amount of time to implement. The main issue was me trying to understand how exactly I was supposed to implement it, but I think I figured it out. I looked online for implementations of the Monte Carlo algorithm for tic tac toe and I realised that it would simulate the playthrough of entire tic tac toe games, then choose the most beneficial move based on the outcome. I originally tried to implement it this way, but it just wasn’t feasible for chess. It takes too long to simulate just one game, let alone 1000 (for most tic tac toe games). So, I decided to train the AI, save a history of all it’s moves and for each move the outcome they saw (whether it was a win, loss or tie) in a dictionary and save it to a file. Each move would keep track of whether it led to a win, a loss or a tie and update accordingly. All moves also have a list of children.

![Dictionary Explanation](https://i.imgur.com/z8geBcg.png)

I only track the data of moves and add them to the database when the AI is training against itself (MCTSAI vs MCTSAI). If it’s not, I don’t track any of the data of the games. When the AI is training by playing against itself, I had it always explore instead of choosing moves that it already knew. It wouldn’t care about which moves won and lost and would just randomly choose. That way, I would have a larger database of moves. However, when it’s playing against a player, it will prioritise moves that exist in the database that have a high chance to win. This is done with my score function. For example, for white I add up the values of the total wins then subtract it by the losses and ties. Then divide that by the total amount of games to calculate the score. I subtract the ties for white and add the ties for black because generally black would play to tie since they’re playing from behind whereas white would have a higher chance of winning so they wouldn’t want to tie. If the moves score was above .5, then I would add it to a list of good moves. Then select the move with the highest chance to win. If no moves existed, then it’d choose a random move. I had the AI play against the Generic AI after 150 simulations and I found it to tie quite a lot. After 30 games, this was the result. Generally, the ties were favoured for MCTS, but it gave up its free pieces, forcing it to be tied as there were only two kings left.

![Graph](https://i.imgur.com/rOsRE0b.png)

This AI was still quite bad, even compared to the minimax one but that was due to how little data I had at the time when I played against it.
