Problem 1 (Prove if the conclusion is valid)

  

If it rains,I will take a leave

If it is hot outside, I will go for a shower

Either I will not take a leave or I will not go for a shower

Therefore — Either it does not rain or it is not hot outside

  

$$\text{let} \newline  
r = \text{It rains}\newline  
l = \text{I leave}\newline  
h = \text{It is hot outside}\newline  
s = \text{I shower}\newline$$

Construct premises:

$$r \implies l \newline  
h \implies s \newline  
\neg{l} \lor \neg{s} \newline  
\therefore \neg{r} \lor \neg{h}$$

  

$$P1. r \implies l \newline  
P2. h \implies s \newline  
P3. \neg{l} \lor \neg{s} \newline  
P4. \neg{l} \space \text{(From P3: Disjunctive Syllogism)} \newline  
P5. \neg{s} \space \text{(From P3: Disjunctive Syllogism)} \newline  
P6. \neg{r} \space \text{(From P1 and P4: Modus Tollens)} \newline  
P7. \neg{h} \space \text{(From P2 and P5: Modus Tollens)} \newline  
\therefore \neg{r} \lor \neg{h} \text{(From P6 and P7: Addition)} \newline$$

  

# Alternative

$$\text{Proof By Cases} \newline  
P1. r \implies l \newline  
P2. h \implies s \newline  
P3. \neg{l} \lor \neg{s} \newline  
\text{Case 1: Assume } \neg{l} \newline  
\neg{r} \text{(By Modus Tollens in P1)} \newline  
\therefore \neg{r} \lor \neg{h} \text{(By Addition)} \newline  
\text{Case 2: Assume } \neg{s} \newline  
\neg{h} \text{(By Modus Tollens in P2)} \newline  
\therefore \neg{r} \lor \neg{h} \text{(By Addition)} \newline  
\therefore \neg{r} \lor \neg{h}$$

  

  

Problem 2: Use De Morgan’s Laws to find the negation of each of the following statements

  

1. Kwame will take a job in industry or go to graduate school
2. Yoshiko knows Java and calculus
3. James is young and strong
4. RIta will move to Oregon or Washington

  

$$\text{Recall De Morgan's Laws:} \newline  
\neg{(P \land Q)} = \neg{(P) \lor \neg{(Q)}} \newline  
\neg{(P \lor Q)} = \neg{(P)} \land \neg{(Q)}$$

a.

$$\text{let} \newline  
i = \text{Industry Job} \newline  
g = \text{Graduate School} \newline$$

$$\text{Premises:} \newline  
P1. i \lor g \newline  
\text{Negate} \newline  
P2. \neg{(i \lor g)} \newline  
\therefore \neg{i} \land \neg{g} \newline  
\therefore \text{Kwame will not take a job in industry and he will not go to graduate school}$$

  

b.

$$let \newline  
j = \text{Knows Java} \newline  
c = \text{Knows Calculus} \newline$$

$$j \land c \newline  
\text{Negate} \newline  
\neg{(j \land c)} \newline  
\therefore \neg{j} \lor \neg{c} \newline  
\therefore \text{Yoshiko does not know either Java or Calculus}$$

c.

$$let \newline  
y = \text{Is Young} \newline  
s = \text{Is Strong} \newline$$

$$y \land s \newline  
\text{Negate} \newline  
\neg{(y \land s)} \newline  
\therefore \neg{y} \lor \neg{s} \newline  
\therefore \text{James is neither young or strong.}$$

  

d.

$$let \newline  
o = Oregon,  
w = Washington$$

$$o \lor w \newline  
\neg{(o \lor w)} \newline  
\therefore \neg{o} \land \neg{w} \newline  
\therefore \text{Rita will not move to both Oregen and Washington}$$