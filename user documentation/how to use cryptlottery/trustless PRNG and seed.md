# Trustless PRNG and Seed

Crypt Lottery allows its players to directly provide the seed in the form of a word that is hashed. This makes the input seed truly random as players may provide any seed to the smart contracts without depending upon any predictable algorithm.

The smart contracts then use the random seeds to generate random numbers (RNG) that are impossible for anyone to predict. Thus, tampering with the game results becomes an extraordinarily scarce possibility.

When the game ends, the hashed words are revealed.

Someone may try and predict the result then, but the game would have already concluded by that time.