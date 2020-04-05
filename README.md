# Guess Penalty Challenges for CTFd

Often times in a CTF, you may want to penalize users that brute force answers
until they get it correct. This is especially important for multiple choice
challenge types.

This CTFd plugin creates a challenge type which implements this behavior.
Each guess penalty challenge starts with an initial point value and then
each failure will issue a negative value award until a minimum overall point
value is reached. The penalty is only assessed once the challenge is successfully
solved.

The way CTFd calculates score today, negative awards (penalties) were the only
real way to approach this problem that I could see without adjusting the internals.

Within CTFd you are free to mix and match other challenge types with this one.

The current implementation requires the challenge to keep track of three values:

 * Initial - The original point valuation
 * Decay - The amount of solves before the challenge will be at the minimum
 * Minimum - The lowest possible point valuation

The penalty value follows the same decay logic as the built-in dynamic_challenge type.

If the penalty would cause the net value to be lower than the minimum, the penalty
will be adjusted accordingly.

## Caveats

1. Since the penalties are issued as negative value awards, penalties become
known to everyone that can view the scoreboard. Adding a feature to offer
"hidden" awards might help address this better.

2. There currently is no key relationship with a given challenge and awards issued.
Due to this, if you delete the challenge itself, there is no good way to cascade
delete all associated penalties. Same goes if you deleted the user's solve.
Also, if you delete the penalty, if the user re-solves the challenge (because you
deleted the solve), the wrong submissions still count towards a new penalty issued.

# Installation

**REQUIRES: CTFd >= v2.0.0**

1. Clone this repository to `CTFd/plugins`. It is important that the folder is
named `guess_penalty_challenges` so CTFd can serve the files in the `assets`
directory.
