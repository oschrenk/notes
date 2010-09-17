# iPad

## Usability ##

[Nielsen Norman Group Report][nngroup.ipad]

### Mental Model ###

In the field of Human–Computer Interaction (HCI)7, the term _mental models_ refers to "many different aspects of users’ knowledge about the systems they use" [Payne08][#Payne:2008]. 

Mental models are powerful for immersive apps, which often heavily rely on (beautiful) graphics. IPad's strength is its amazing display and offers such possibilities.

#### Magazine Mental Model ####

Applications for magazines blend the mental model of a paper magazine with the mental mode of a computer. Eg.
- swiping for turning pages
- access to hyperlinks

Good
- page indicators let user know that they can/must swipe

Bad
- page numbers are a poor navigation mechanism
- scroll or swipe to get to next page
- mix scrollable and swipe-able content in multiple panels
- inconsistent swipes - show part of bigger picture or next page article.
- no information scent ("next article" - which article?)

Conclusion
- page-by-page model is unnecessary, does not make use of habits for the web
- sequential model for navigating doesn't work

## Touch Screen ##

- Implement two gestures for turning pages: Tapping Side of the screen and Swiping
- clear action oriented tappable areas 
- plan for accidental taps (how do I get back?)
- A target area must look as if it can be tapped.
- The context of a target influences the affordance of that target. (Text by itself may seem more tappable than text surrounded by widgets.)

## Lack of Affordances: Where Can I Tap?

- don't hide controls
- hyperlink content
- provide search
- bottom tab bar is often ignored - focus near top

## Multiple Panels ##

- good for pulling information into single view
- don't disconnect action/feedback


[nngroup.ipad]:http://s3.amazonaws.com/nngroup/ipad-usability.pdf

[#Payne:2008]: Payne, S. (2008). "Mental Models in Human-Computer Interaction". In Sears, A., Jacko, J. (eds.). Handbook of Human-Computer Interaction. CRC Press.