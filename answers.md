# CMPS 6610 Extra Credit Answers
  
In this extra credit assignment, we will test and review concepts you
   have learned since the midterm exam. Please add your written answers
   to `answers.md` which you can convert to a PDF using
   `convert.sh`. Alternatively, you may scan and upload written
   answers to a file named `answers.pdf`.

By: Keith J. Mitchell

1. **Algorithmic Paradigms**
- My favorite algorithmic paradigm is divide and conquer. This is the method of breaking a problem into independent subproblems (unlike dynamic programming, which handles overlapping subproblems), often leading to parallelism. This allows for simple recurrence analyses (such as the Master theorem) and leads to asymptotically faster algorithms (merge sort, FFT, Karatsuba, Strassen). It also forces one to think about problem structure and the right representation for combining solutions, which often reveals deeper insights about the problem itself.

2. **Divide and Conquer**
- No, divide and conquer problems do not necessarily satisfy optimal substructure. The optimal substructure property is a property of optimization problems where an optimal solution to the problem contains within it optimal solutions to subproblems. A counterexample to this is finding the maximum element in a given array. In this problem, the array would be split in two halves recursively to find the maximum of both left and right sides. It would then return the maximum of the two. This does not illustrate optimal substructure as there is no case of the optimal solution to the whole must contain optimal solutions to subproblems. Finding the maximum is not an optimization problem in this sense as the solution to any given subproblem is just a number and is not a part of the global solution. The combination of the halves determines the optimal solution. The maximum of the whole problem is not constructed from partial components but is simply chosen from the candidates returned by subproblems. 

3. **Randomization**

- 3a. 

$\mathbf{P}[X \geq cn^2] \leq \frac{\mathbf{E}[X]}{cn^2} = O\frac{(n \log n)}{cn^2} = O(\frac{\log n}{n})$

As $n\to\infty$, this probability approaches 0. The algorithm is highly unlikely to exhibit worst-case behavior. 

- 3b.

$\mathbf{P}!\left[X \geq 10^{c} n \ln n \right]
\leq \frac{\mathbf{E}[X]}{10^{c} n \ln n}
= O!\left(\frac{n \ln n}{10^{c} n \ln n}\right)
= O!\left(10^{-c}\right).$

As (c) increases, this probability decreases exponentially.
Thus large constant-factor deviations from the expected $n \ln n$ work are increasingly unlikely, indicating strong concentration around the expectation.

4. **Greedy Algorithms**

Theorem: Scheduling jobs in the order of their processing times minimizes the average waiting time.

Proof by Exchange ArgumentWe prove this by showing that if a schedule $S$ is not the SJF order, we can always improve it by swapping two adjacent jobs, thus proving the SJF schedule must be optimal.

(1) Let $S$ be a schedule that is not in SJF order. This means there must be at least one pair of adjacent jobs, $j_a$ followed immediately by $j_b$, such that the shorter job $j_b$ is scheduled after the longer job $j_a$.Let $p_a$ and $p_b$ be their processing times, with $p_a > p_b$.

(2) Consider the total waiting time for $S$. Let $W_{\text{prior}}$ be the sum of processing times of all jobs scheduled before $j_a$. The positions of all other jobs are fixed.

(3) The combined contribution of $j_a$ and $j_b$ to the total waiting time $W_{\text{total}}$ is:
- $W_a = W_{\text{prior}}$
- $W_b = W_{\text{prior}} + p_a$
- Contribution in $S$: $C_S = W_a + W_b = 2 W_{\text{prior}} + p_a$

(4) Now, consider the schedule $S'$ created by swapping $j_a$ and $j_b$, so $j_b$ is now followed by $j_a$.
- $W'_b = W_{\text{prior}}$
- $W'_a = W_{\text{prior}} + p_b$
- Contribution in $S'$: $C_{S'} = W'_b + W'_a = 2 W_{\text{prior}} + p_b$

(5) Compare the contributions $C_S$ and $C_{S'}$:$$C_S - C_{S'} = (2 W_{\text{prior}} + p_a) - (2 W_{\text{prior}} + p_b) = p_a - p_b$$Since we assumed $p_a > p_b$, it follows that $p_a - p_b > 0$.$$C_S - C_{S'} > 0 \implies C_{S'} < C_S$$Since $C_{S'}$ is strictly less than $C_S$, the total waiting time for $S'$ is strictly less than $W_{\text{total}}(S)$. Swapping the jobs to put the shorter one first improves the schedule.

This process can be repeated until no such violating pair exists, which only happens when the entire schedule is sorted in non-decreasing order of processing times (SJF). Since every non-SJF schedule can be improved, the SJF schedule must be the unique optimal schedule (assuming distinct processing times).

5. **Dynamic Programming**

**Maximum span (no parallelism): Fibonacci Numbers**

Recurrence: $F(n) = F(n-1) + F(n-2)$

The dependency DAG is a chain: $F(n) \to F(n-1) \to F(n-2) \to \ldots \to F(1)$

Span: $O(n)$ — completely sequential, no parallelism possible.

**Polylogarithmic span: Matrix Chain Multiplication**

Recurrence: $M[i,j] = \min_{i \leq k < j} \{M[i,k] + M[k+1,j] + \text{cost}(i,k,j)\}$

The dependency DAG has width increasing with subproblem size. For interval $[i,j]$, we can compute all splits $k$ in parallel. The longest path has length proportional to $\log n$ intervals.

Span: $O(\log^2 n)$ — highly parallel due to interval structure allowing simultaneous computation of many independent subproblems.

6. **Graphs**

Theorem: Let $C$ be any cycle in $G$, and let $e$ be the maximum-weight edge in $C$. Then $e$ cannot be in any MST of $G$.

Proof:

Assuming $T$ is an MST that contains $e$, where $e = (u,v)$ is the maximum-weight edge in cycle $C$.

Since $T$ is a tree, removing $e$ disconnects $T$ into two components, say $T_1$ and $T_2$, with $u \in T_1$ and $v \in T_2$.

Since $C$ is a cycle, there exists an alternate path in $C$ from $u$ to $v$ that doesn't use $e$. This path must contain some edge $e' = (x,y)$ where $x \in T_1$ and $y \in T_2$ (it crosses the cut).

Now consider $T' = T - \{e\} + \{e'\}$. Then $T'$ is connected (since $e'$ reconnects the two components), $T'$ is acyclic (we removed an edge from a tree and added one that wasn't in it), so $T'$ is a spanning tree.

Since $e$ is the maximum-weight edge in $C$ and $e'$ is also in $C$, we have $w(e') \leq w(e)$.

If $w(e') < w(e)$, then $w(T') < w(T)$, contradicting that $T$ is an MST.

If $w(e') = w(e)$, then $w(T') = w(T)$, so $T'$ is also an MST. But this means we can always construct an MST without $e$ by this exchange process.

Therefore, no MST can contain the maximum-weight edge of any cycle.