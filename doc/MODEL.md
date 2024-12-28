
# Tomorrowland Artist Scheduling
*Vlerick Business School, Master in Data Analytics and Artificial Intelligence 2024-2025. Made by Luna Geens. For the course Decision Optimisation given by Mario Vanhoucke, as a final assignment due on Friday 20 Dec 2024.*

## 1. Introduction

###  <b style="color: #8B0000;">1.1 Problem Description</b>

Tomorrowland, as one of the largest and most prestigious electronic music festivals globally, attracts hundreds of thousands of attendees from over **200 countries**, with tickets often selling out within minutes. Since its founding in **2005**, Tomorrowland has expanded to host over **15 stages** simultaneously, accommodating **800+ artists** across multiple weekends, making efficient scheduling a necessity rather than a choice.

Efficient artist scheduling is not only a logistical challenge, but also a critical factor in enhancing the **festival experience** and **business outcomes**.
- Festivals thrive on providing unforgettable experiences, where attendees expect their favorite artists to perform at ideal times and on prominent stages. Balancing artist popularity with time slots ensures maximum audience engagement and satisfaction.
-  Strategic artist placement directly influences festival success:
    - Ticket Sales: Scheduling popular artists during prime slots drives higher attendance.
    - Reputation: Delivering a seamless experience strengthens Tomorrowland's global standing as a premier festival.
    - Cost Optimization: Efficient scheduling minimizes operational costs related to stage management, logistics, and artist coordination.

<br>

Tomorrowland’s scale introduces significant complexity:

- High Artist Volume: Hundreds of artists perform multiple sets across various stages over several days.
- Time and Stage Constraints: Each stage has a defined availability window per day, while artists have specific limits on the number of performances and required rest time.
- Audience Demand: Aligning artist popularity with desirable time slots adds another layer of optimization.

<br>

At Tomorrowland's scale, manual scheduling is impractical. By leveraging optimizatoin-driven approaches, this research provides insights into optimizing festival scheduling, delivering tangible benefits to both attendees and organizers. In recent years, studies on festival optimization have increasingly relied on **mathematical models** (e.g., linear programming) to address these challenges.

<br>


###  <b style="color: #8B0000;">1.2 Mathematical Problem Description</b>

#### <b style="color: #8B0000;">1.2.1 Input Sets and Parameters</b>

Sets:
- $S$ = Set of **stages**, where $S = { s_1, s_2, \dots, s_n } $.
- $A$ = Set of **artists**, where $ A = { a_1, a_2, \dots, a_m} $.
- $K_a$ = Set of **sets performed** by artist $ a \in A$, where $K_a = {k_1, k_2, \dots , k_l}$ .
- $D$ = Set of **days**, where $D={D=a_1, a_2, \dots, a_p}$.
- $T_s,d$ = Set of **starting points of time slots** available for stage $s$ during day $d$, where $T_{s,d} = {t_1, t_2, \dots, t_q}$.

<br>

Parameters:

**Set-Specific** Parameters: For each artist's set $k \in K_a $:
- $b_{k}$: Time point in minutes of the beginning of the set $k$.  *Note that this is a decision variable in the problem, not a parameter!*
- $d_{k}$: Day of the set $k$. *Note that this is a decision variable in the problem, not a parameter!*
- $e_{k} $: Duration of set $ k $ in minutes.

**Stage-Specific** Parameters: For each stage $s \in S $:
- $t^{open}_{s,d}$: Opening time of stage $s$ on day $d$.
- $t^{close}_{s,d}$: Closing time of stage $s$ on day $d$.
- $c$: Clean-up time required after a performance on each stage (e.g., $ c = 45  \text{ minutes} $).

**Artist-Specific** Parameters: For each artist $a \in A$:
- $r$:  Rest time required for each artist between two sets (e.g., $ r = 120  \text{ minutes} $).
- $ \text{max_sets}$: Maximum number of performances each artist can play per day. (e.g., $ \text{max\_sets} = 2 $).

**Popularity** Parameters:
- $ p_a $: Popularity score for artist $ a $, where $ a \in A $.
- $ p_s $: Popularity score for stage $ s $, where $ s \in S $.
- $ p_t $: Weight indicating the popularity of the starting point of the timeslot $ t $, where $ t \in T $.

<br>

#### <b style="color: #8B0000;">1.2.2 Decision Variables</b>

The decision variable accounts for artists performing multiple sets:

$
x_{a, k, s, d, t} =
\begin{cases}
1 & \text{if artist } a \text{ performs set } k \text{ on stage } s \text{ starting at time slot } t \text{ on day } d \\
0 & \text{otherwise.}
\end{cases}
$

Where:
- $ a \in A $: Artist in the set of  Artists.
- $ k \in K_a $: Set in the set of Sets performed by artist $a$.
- $ s \in S $: Stage in the set of Stages.
- $ t \in T_{s,d} $: Start time in the set of Start times for stage $s$ on day $d$.
- $ d \in D$: Day in the set of Days.

<br>

#### <b style="color: #8B0000;">1.2.3 Objective Function</b>


The objective is to **maximize customer satisfaction** by considering artist, stage, and timeslot popularity. These weights are predefined inputs based on audience preferences or prior analysis.

$
\text{Maximize } Z = \sum_{a \in A} \sum_{k \in K_a} \sum_{s \in S} \sum_{d \in D} \sum_{t \in T_{s,d}} (p_{a} + p_{s} + p_{t}) \cdot x_{a, k, s, d, t}.
$

Where:
- $ a \in A $: Artist in the set of  Artists.
- $ k \in K_a $: Set in the set of Sets performed by artist $a$.
- $ s \in S $: Stage in the set of Stages.
- $ t \in T_{s,d} $: Start time in the set of Start times for stage $s$ on day $d$.
- $ d \in D$: Day in the set of Days.
- $ p_a $: Popularity score for artist $ a $, where $ a \in A $.
- $ p_s $: Popularity score for stage $ s $, where $ s \in S $.
- $ p_t $: Popularity score of the starting point of the timeslot $ t $, where $ t \in T_{s,d}$.
- $x_{a, k, s, d, t}$: Binary variable indicating if artist $a$ performs set $k$ on stage $s$ starting at time slot $t$ on day $d$.

<br>

#### <b style="color: #8B0000;">1.2.4 Constraints</b>

**(1) Each Set is Scheduled Exactly Once.**

Each set $ k $ of artist $ a $ must be scheduled at exactly one stage and time (timeslot and day):

For $\forall a \in A; \ k \in K_{a}$:

$
\sum_{s \in S} \sum_{d \in D} \sum_{t \in T_{s,d}}  x_{a, k, s, d, t} = 1
$.

Where:
- $ a \in A $: Artists.
- $ k \in K_a $: Sets performed by artist $a$.
- $ s \in S $: Stages.
- $ d \in D$: Days.
- $ t \in T_{s,d} $: Start times for stage $s$ on day $d$.
- $x_{a, k, s, d, t}$: Binary variable indicating if artist $a$ performs set $k$ on stage $s$ starting at time slot $t$ on day $d$.

<br>

**(2) Each Set is Scheduled within Stage Availability.**

The start and end times of each set $k$ of artist $a$ played on stage $s$ on a day $d$ must lie within the stage’s open and close times of that day $d$.

For $\forall s \in S; \ d \in D; \ a \in A; \ k \in K_a$: Both conditions (1) and (2) are required to hold.

1. Start time $b_{k}$ of set $k$ on stage $s$ is after the stage's opening time $t^{open}_{s,d}$: $ \ b_{k} \leq t^{open}_{s,d}$.
2. The set finishes ($b_{k} + e_k$) before the stage's closing time $t^{close}_{s,d}$: $ \ b_{k} + e_{k} \leq t^{close}_{s,d}$.

Where:
- $ s \in S $: Stages.
- $ d \in D$: Days.
- $ a \in A $: Artists.
- $ k \in K_a $: Sets performed by artist $a$.
- $b_{k}$: Time point in minutes of the beginning of the set $k$.
- $e_k$: Duration of set $ k $ in minutes.
- $t^{open}_{s,d}$: Opening time of stage $s$ on day $d$.
- $t^{close}_{s,d}$: Closing time of stage $s$ on day $d$.

<br>

**(3) Sets on the Same Stage are Scheduled at a Different Time with a Clean-Up Time inbetween.**

Two sets $k$ and $k′$ on the same stage $s$ do not overlap in time.  Additionally, a clean-up time $c$ is required between the two sets. This ensures that no two performances occur simultaneously on the same stage, and sufficient time is allowed for stage preparation between sets.

For $\forall s \in S$; $d \in D$; $a \in A$;  $k, k' \in K_a$ and $k \neq k' $: At least one of the conditions (1) and (2) are required to hold.

1. Set $k$ must finish before $k′$ starts: $b_k + e_k + c \leq b'_k$.
2. Set $k′$ must finish before $k$ starts: $b'_k + e'_k + c \leq b_k$.

Where:
- $ s \in S $: Stages.
- $ d \in D$: Days.
- $ a \in A $: Artists.
- $ k \in K_a $: Sets performed by artist $a$.
- $b_{k}$: Time point in minutes of the beginning of the set $k$.
- $e_k$: Duration of set $ k $ in minutes.
- $c$: Clean-up Time $(c = 45  \text{ minutes} )$.

<br>

**(4) Sets of the Same Artist are Scheduled at a Different Time with a Rest Time inbetween.**

Two sets $k$ and $k′$ of the same artist $a$ do not overlap in time.  Additionally, a rest time $r$ is required between the two sets. This ensures that no two performances occur simultaneously of the same artist, and sufficient time is allowed for artist preparation between sets.

For $\forall s, s' \in S$; $d \in D$; $a \in A$;  $k, k' \in K_a$ and $k \neq k' $: At least one of the conditions (1) and (2) are required to hold.

1. Set $k$ must finish before $k′$ starts: $b_k + e_k + r \leq b'_k$.
2. Set $k′$ must finish before $k$ starts: $b'_k + e'_k + r \leq b_k$.

Where:
- $ s, s' \in S $: Stages. The sets can be on the same or different stages.
- $ d \in D$: Days.
- $ a \in A $: Artists.
- $ k \in K_a $: Sets performed by artist $a$.
- $b_{k}$: Time point in minutes of the beginning of the set $k$.
- $e_k$: Duration of set $ k $ in minutes.
- $r$: Rest Time $(r = 120  \text{ minutes} )$.

<br>

**(5) No more than 2 Sets of an Artist are Scheduled on the Same Day.**

Each artist is restricted to performing at most two sets on any single day. This ensures a manageable workload and a balanced schedule.

For $d \in D$; $a \in A$:

$\sum_{k \in K_a} \sum_{s \in S} \sum_{t \in T_{s,d}} x_{a, k, s, d, t} \leq 2$

Where:
- $ d \in D$: Days.
- $ a \in A $: Artists.
- $ k \in K_a $: Sets performed by artist $a$.
- $ s \in S $: Stages.
- $ t \in T_{s,d} $: Start times for stage $s$ on day $d$.
- $x_{a, k, s, d, t}$: Binary variable indicating if artist $a$ performs set $k$ on stage $s$ starting at time slot $t$ on day $d$.

<br>

<b style="color: #8B0000;">5. Outputs</b>

The solution will provide:
1. **Artist Schedule**: Table mapping each artist's set to a stage, time, and day.
2. **Stage Utilization**: Ensures clean-up time is respected between sets.
3. **Artist Rest Validation**: Verifies artists have sufficient rest and do not exceed two sets per day.
4. **Stage Availability**: Summarizes the overall availability for each stage.

<br>

<b style="color: #8B0000;">6. Summary Table of Symbols</b>

| **Symbol**                | **Description**                                                                                                             |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| $S$                       | Set of stages, where $S = \{s_1, s_2, \dots, s_n\} $.                                                                       |
| $A$                       | Set of artists, where $ A = \{a_1, a_2, \dots, a_m\} $.                                                                     |
| $ K_a $                 | Set of sets performed by artist $ a \in A $, $ K_a = \{k_1, k_2, \dots, k_l\} $.                                            |
| $ D $                   | Set of days, where $ D = \{d_1, d_2, \dots, d_p\} $.                                                                        |
| $ T_{s,d} $             | Set of starting points of time slots available for stage $ s $ during day $ d $.                                            |
| $ b_k $                 | Time point in minutes of the beginning of the set $ k $.                                                                    |
| $ d_k $                 | Day of the set $ k $.                                                                                                       |
| $ e_k $                 | Duration of set $ k $ in minutes.                                                                                           |
| $ t^{open}_{s,d} $      | Opening time of stage $ s $ on day $ d $.                                                                                   |
| $ t^{close}_{s,d} $     | Closing time of stage $ s $ on day $ d $.                                                                                   |
| $ c $                   | Clean-up time required after a performance on each stage.                                                                   |
| $ r $                   | Rest time required for each artist between two sets.                                                                        |
| $ \text{max_sets} $     | Maximum number of performances each artist can play per day.                                                                |
| $ p_a $                 | Popularity score for artist $ a $.                                                                                          |
| $ p_s $                 | Popularity score for stage $ s $.                                                                                           |
| $ p_t $                 | Popularity of the starting point of the timeslot $ t $.                                                                     |
| $ x_{a, k, s, d, t} $   | Binary variable: 1 if artist $ a $ performs set $ k $ on stage $ s $ starting at time slot $ t $ on day $ d $, 0 otherwise. |
