---
title: "Screwed Up Poker"
date: 2014-08-01
tags: [ games, poker, cards, probability ]
---


Every so often I have the pleasure of playing low stakes poker with some
friends. Usually we play [Texas hold 'em] or some variant of it. It's a
tough game to play (at least for unskilled people such as myself), so often
near the end of the night when we're tired (or drunk), we switch to a much
simpler game called [Screw Your Neighbor].

{{< right >}}
{{< figure src="ace.jpg" width="300px"
attr="© Steven Depolo, Attribution 2.0 Generic"
attrlink_="http://creativecommons.org/licenses/by/2.0/"
>}}
{{< /right >}}

This game is a form of poker in a very loose sense in that it involves a
standard deck of bridge cards and involves betting money, but the
similarity stops there. The game is a process of elimination. Each person
has a single card and the person or people with the lowest value card (ace
high) lose. Three losses and you are eliminated. The play begins with the
dealer giving one card to each player. The player to the dealer's left
starts by deciding whether to keep or exchange her card. If she exchanges
her card, she trades with the player on her left. As an exception to the
rule, if the player to the left happens to have a king, the exchange is
blocked and the first player has to keep her card.  In any case, play
continues to that player on the left in the same way around the table until
it is the dealer's turn. The dealer also has a choice to keep or exchange
his card. But because there are no further players, he gets a new card from
the deck if he chooses to exchange. At this point everyone shows their
cards; the 1 or more people with the lowest value card get eliminated, and
the deal shifts to the left.

The strategy for this game is simplistic. Keep high cards; exchange low
cards (unless you know there is a lower card). The only real decision comes
when you have a between ('tween) card around an 8, in which case you are as
likely to get a worse card as a better one in an exchange. When several
people are playing, I usually hold on to 'tween cards in the hopes someone
will get stuck with a lower card in a bad exchange.

Strategy starts to change, however, as players get eliminated. Eventually
(usually) play is reduced to two players: the dealer and the dealer's
opponent. Here things get intense because there is a fair amount of money
in the pot and it all comes down to one quick game. Thus, it is helpful to
know the odds of winning and the strategy for exchanging.

Here is what my intuition says about the game. First, the dealer has a much
better chance of winning this last game. After all, if the opponent
exchanges cards, the dealer knows exactly whether he is winning or losing
and still gets another chance to win. Second, the opponent should sandbag
the 'tween cards because once the exchange happens, the dealer gets the
advantage of knowing both cards.

But, hey, why rely on intuition? The game is simple enough to iterate all
the possibilities so even I can figure out the probabilities of each
outcome. So let's test this hypothesis and see just how this game is
played.

Since each player makes only one choice, we can characterize the strategy
of each with one variable. The first is the threshold at which the opponent
exchanges cards. The second is the threshold at which the dealer exchanges
cards if the opponent does not. (If the opponent does exchange cards, the
dealer behavior is obvious and deterministic.)

There are multiple ways to approach the problem. I decided to write [a
small program that tries all possible combinations]. That is, given a
strategy for dealer and opponent, it looks at all possible deals to
determine who is the winner. You can then count the number of wins and
losses to determine the odds. I then repeat that for lots of different
strategies. The following table summarizes the dealer odds/opponent odds
for a range of viable strategies. (I computed a wider range of thresholds
as well, but am only showing the interesting range here.)

<style>
table.StrategyTableContainer {
    margin-left:auto;
    margin-right:auto;
}

.StrategyTable td {
    padding-left: 5px;
    padding-right: 5px;
}
</style>

<table class="StrategyTableContainer">
  <tr>
    <td></td>
    <th>Opponent Exchange Threshold</th>
  </tr>
  <tr>
    <th><br/>Dealer<br/>Exchange<br/>Threshold</th>
    <td><table class="StrategyTable">
      <tr>
        <td></td>
        <th>7</th>
        <th>8</th>
        <th>9</th>
        <th>10</th>
      </tr>
      <tr>
        <th>7</th>
        <td>0.443/0.470</td>
        <td>0.442/0.477</td>
        <td>0.452/0.473</td>
        <td>0.475/0.457</td>
      </tr>
      <tr>
        <th>8</th>
        <td>0.453/0.463</td>
        <td>0.449/0.468</td>
        <td>0.457/0.467</td>
        <td>0.478/0.453</td>
      </tr>
      <tr>
        <th>9</th>
        <td><b>0.457</b>/0.462</td>
        <td>0.456/<b>0.464</b></td>
        <td>0.462/0.460</td>
        <td>0.481/0.448</td>
      </tr>
      <tr>
        <th>10</th>
        <td>0.454/<b>0.467</b></td>
        <td><b>0.457</b>/0.467</td>
        <td>0.466/0.459</td>
        <td>0.483/0.444</td>
      </tr>
    </table></td>
  </tr>
</table>

Looking at these odds, we see that the optimal strategies for the players
(assuming both use optimal strategies) are for the opponent to exchange
cards when she has an 8 or lower and for the dealer to exchange cards when
he has a 10 or lower.

These strategies are bit surprising. Exchanging a 10 card is more likely to
yield a smaller card than a larger card. However, in retrospect this still
makes sense because if the opponent does not switch cards, then she has a
card greater than 8. Thus, the dealer is more likely to get a better card
than beat his opponent with a 10. Likewise, I expected the opponent to keep
lower cards, but again because the dealer can swap cards, his median card
is a 10 or better.

But the really surprising thing is that the odds of winning are almost even
money between dealer and opponent. The apparent advantage of (sometimes)
knowing the opponent's card does not exist. What gives?

To understand this, let us take a closer look at the odds of winning given
a particular dealt hand.

<style>
table.OpeningOdds {
    margin-left:auto;
    margin-right:auto;
}

.OpeningOdds td {
    padding-left: 5px;
    padding-right: 5px;
    text-align: center;
}
.OpeningOdds th {
    padding-left: 5px;
    padding-right: 5px;
}
.HeaderRow th {
    border-bottom: solid 1px grey;
}
</style>

<table class="OpeningOdds">
  <tr class="HeaderRow">
    <th>Dealer<br/>Starting<br/>Card</th>
    <th>Dealer<br/>Winning<br/>Odds</th>
    <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
    <th>Opponent<br/>Starting<br/>Card</th>
    <th>Opponent<br/>Winning<br/>Odds</th>
  </tr>
  <tr>
    <th>Ace</th>
    <td>0.38</td>
    <td/>
    <th>Ace</th>
    <td>0.87</td>
  </tr>
  <tr>
    <th>King</th>
    <td>0.85</td>
    <td/>
    <th>King</th>
    <td>0.74</td>
  </tr>
  <tr>
    <th>Queen</th>
    <td>0.31</td>
    <td/>
    <th>Queen</th>
    <td>0.61</td>
  </tr>
  <tr>
    <th>Jack</th>
    <td>0.28</td>
    <td/>
    <th>Jack</th>
    <td>0.48</td>
  </tr>
  <tr>
    <th>10</th>
    <td>0.25</td>
    <td/>
    <th>10</th>
    <td>0.43</td>
  </tr>
  <tr>
    <th>9</th>
    <td>0.30</td>
    <td/>
    <th>9</th>
    <td>0.37</td>
  </tr>
  <tr>
    <th>8</th>
    <td>0.34</td>
    <td/>
    <th>8</th>
    <td>0.31</td>
  </tr>
  <tr>
    <th>7</th>
    <td>0.41</td>
    <td/>
    <th>7</th>
    <td>0.34</td>
  </tr>
  <tr>
    <th>6</th>
    <td>0.48</td>
    <td/>
    <th>6</th>
    <td>0.36</td>
  </tr>
  <tr>
    <th>5</th>
    <td>0.53</td>
    <td/>
    <th>5</th>
    <td>0.38</td>
  </tr>
  <tr>
    <th>4</th>
    <td>0.57</td>
    <td/>
    <th>4</th>
    <td>0.39</td>
  </tr>
  <tr>
    <th>3</th>
    <td>0.60</td>
    <td/>
    <th>3</th>
    <td>0.40</td>
  </tr>
  <tr>
    <th>2</th>
    <td>0.62</td>
    <td/>
    <th>2</th>
    <td>0.40</td>
  </tr>
</table>

{{< right >}}
{{< figure src="king.jpg" width="300px"
attr="© Ashley Sturgis, Attribution 2.0 Generic"
attrlink_="http://creativecommons.org/licenses/by/2.0/"
>}}
{{< /right >}}

The odds for the opponent are about what we would expect. Getting a higher
card yields a better chance of winning. But the odds for the dealer are
strange. With the exception of the king, which blocks the opponent, getting
dealt a higher card actually means a worse chance at winning. There is a
better than 50% chance of having to give up a high card to the opponent. It
is actually better for the dealer to deal himself a low card in hopes that
his opponent exchanges it.

All that said, any rational strategy is within 2% probability of the
optimal strategy, and the difference in odds between the two players is
similarly close (which is probably insignificant compared to the
assumptions I made in computing them). We might as well determine the
winner by flipping a coin. It's a stupid game, but of course that's why we
play it.

[Texas hold 'em]: http://en.wikipedia.org/wiki/Texas_hold_%27em
[Screw Your Neighbor]: http://en.wikipedia.org/wiki/Ranter-Go-Round
[a small program that tries all possible combinations]: https://gist.github.com/kmorel/f26bc008ab53e100ec02
