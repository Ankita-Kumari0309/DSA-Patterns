### What is MCM pattern?
Any problem where you pick an index k inside a range (i, j) and split it into two sub-problems + a cost. Recurrence looks like:

dp(i, j) = min/max over k { cost(i,k,j) + dp(i,k) + dp(k,j) }

# Problems List
1. Matrix Chain Multiplication
2. Burst Balloons
3. Strange Printer
4. Remove Boxes
5. Minimum Cost to Cut a Stick
