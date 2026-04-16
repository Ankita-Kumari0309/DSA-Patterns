# 📌 What is Game Theory?

### Game Theory problems involve:

Two players
Alternate moves
Both play optimally (smartly)

### 👉 Goal:

Determine who wins OR
Find the maximum score

### 🧠 Core Principle (Golden Rule)

If you can force your opponent into a losing state → YOU WIN

### ⚡ General Approach

## Define State
What describes the game?
Example: number of stones, index range, etc.

## Define Function

f(state) = can current player win?

## Try All Moves

If ANY move leads opponent to lose → WIN
Else → LOSE

## Use DP / Memoization

🟢 Type 1: Take / Remove Stones

Example:
Remove 1, 2, or 3 stones

Last move wins
Recurrence:

dp[n] = true if any dp[n - move] == false

Code: 

boolean solve(int n)
{
    if(n == 0) return false;

    if(n >= 1 && !solve(n-1)) return true;
    if(n >= 2 && !solve(n-2)) return true;
    if(n >= 3 && !solve(n-3)) return true;

    return false;
}

### 💡 Pattern:

n % 4 == 0 → Lose
otherwise → Win


## The DP formulation you write

dp(state) = score I gain — score opponent gains from here

At my turn: dp(state) = max over all my moves of [ gain + dp(next_state) ]

At opp turn: dp(state) = min over all opponent moves of [ dp(next_state) ]

OR equivalently (relative score trick):

dp(state) = max over all moves of [ gain - dp(next_state) ]

              ↑ note the minus — opponent becomes "you" next turn
