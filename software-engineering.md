# Software Engineering #

## Talks ##

[Greg Wilson](http://en.wikipedia.org/wiki/Gregory_V._Wilson) gave a terrific talk called [What We Actually Know About Software Development, and Why We Believe It's True](http://vimeo.com/9270320) during the Cusec conference on 2010-Feb-07.

It approaches software engineering topics like programming paradigms with more evidence based methods.

### Quotes ###

> Most people would rather fail than change.
> Engineering is what appens when you put science and economics together

## Environment ##

- [Stackoverflow, How Does Noise affect productivity?](http://programmers.stackexchange.com/questions/132952/studies-on-how-noise-affects-productivity-of-programmers#comment249179_132952)

The book [Peopleware](http://rads.stackoverflow.com/amzn/click/0932633439) has several chapters that cover the subject. You can read a decent summary [here](http://javatroopers.com/Peopleware.html).

> Workplace Quality and Product Quality - Companies that provide small and noisy workplaces explain away complaints as workers campaigning for the added status of bigger, more private space. To determine whether noise level had any correlation to work, we divided our sample into those who found the workplace acceptably quiet and those who didn't. Then, looking at workers within each group who completed the entire exercise without a single defect:

> **Workers who reported that their workplace was acceptably quiet before

the exercise were 1/3 more likely to deliver zero-defect work.**

As the noise level gets worse, this trend gets stronger:

- Zero-defect workers: => 66% reported noise level OK
- 1-or-more-defect workers: => 8% reported noise level OK

> A Discovery of Nobel Prize Significance - On February 3, 1984, in a study of 32,346 companies worldwide, the authors confirmed a virtually perfect inverse relationship between people density and dedicated floor space per person. If you're having trouble seeing why this matters, you're not thinking about noise. Noise is directly proportional to density, so halving the allotment of space per person can be expected to double the noise. Even if you managed to prove conclusively that a programmer could work in 30 sq. ft. without being hopelessly space-bound, you still wouldn't be able to conclude that 30 sq. ft. is adequate space. The noise in a 30 sq. ft matrix is more than triple the noise in a 100 sq. ft. matrix, which could make the difference between a plague of product defects and none at all.

## Management ##

### Time Estimates ###

- [Why are software development task estimations regularly off by a factor of 2-3?](http://www.quora.com/Engineering-Management/Why-are-software-development-task-estimations-regularly-off-by-a-factor-of-2-3)

**Management Blame and Politics**

- The insistence of project management to create an unrealistic schedule in the first place.

**Client Blame**

- The changing needs of clients that impact scope significantly.
- Delays in response from clients.

**Relating to Inexperience/Incompetence**

- The unpredictable stability of code depending on quality.
- Overconfidence of developers.
- Lack of a product champion.
- Lack of committed subject-matter experts.
- Lack of buy-in by users/management.
- Failure to assess previous project scopes
- Failure to assess current rate of production vs. budgeted time. 
- Poor discovery work.
- Poor understanding of the project's complexity.
- Unforeseen and unpredictable bugs.

**Potential Root Causes (for some of the above)**

- The idea of absolute control (over ourselves, other human beings and unforeseen circumstances) combined with the idea that if we plan something hard enough, we can have such absolute control.  This is obviously a very bad idea. This is why General Eisenhower once said "Forget the plan. Planning is everything."

- The fact that actual effort depends on decisions not yet taken at the time of estimation
- The software industry as a whole rapidly changes, and to be on the cutting edge requires the use of tools and concepts with which developers have yet to gain decent experience.

- The modern software stack is so massive and complex and constantly changing, that it is almost impossible to predict many of the time consuming issues that inevitably emerge.

**Suggestions**

- Pad every estimate you get from your developers by a factor of three.
- Get a project lead skilled at negotiating with stakeholders

## Branching Strategies & Versioning Control ##

[Branching strategy is not a remedy for instability](http://altdevblogaday.com/2012/02/09/branching-strategy-is-not-a-remedy-for-instability/)

> Branching strategy changes are in response to the instability that follows fast growth.
> Branching is not designed to fix code instability. Branching is a way to isolate changes and manage a release.

Branching isn't a solution, you need a cultural shift towards testing and better infrastructure. These shift takes time and effort.
