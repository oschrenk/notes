# mutt

## Appearance

```
color index         color160        default        "~A"                        # all messages
color index         color166        default        "~E"                        # expired messages
color index         color34         default        "~N"                        # new messages
color index         color33         default        "~O"                        # old messages
color index         color235        default        "~D"                        # deleted messages
color index         color240        default        "~R"                        # read messages
color index         color33         default        "~U"                        # unread messages
color index         color61         default        "~Q"                        # messages that have been replied to
color index         color33         default        "~U~$"                      # unread, unreferenced messages
color index         color254        default        "~v"                        # messages part of a collapsed thread
color index         color254        default        "~P"                        # messages from me
color index         color37         default        "~p!~F"                     # messages to me
color index         color34         default        "~N~p!~F"                   # new messages to me
color index         color34         default        "~U~p!~F"                   # unread messages to me
color index         color240        default        "~R~p!~F"                   # messages to me
color index         color160        default        "~F"                        # flagged messages
color index         color160        default        "~F~p"                      # flagged messages to me
color index         color160        default        "~N~F"                      # new flagged messages
color index         color160        default        "~N~F~p"                    # new flagged messages to me
color index         color160        default        "~U~F~p"                    # new flagged messages to me
color index         color245        default        "~v~(!~N)"                  # collapsed thread with no unread
color index         color136        default        "~v~(~N)"                   # collapsed thread with some unread
color index         color64         default        "~N~v~(~N)"                 # collapsed thread with unread parent
color index         color160        color235        "~v~(~F)!~N"                # collapsed thread with flagged, no unread
color index         color136        color235        "~v~(~F~N)"                 # collapsed thread with some unread & flagged
color index         color64         color235        "~N~v~(~F~N)"               # collapsed thread with unread parent & flagged
color index         color64         color235        "~N~v~(~F)"                 # collapsed thread with unread parent, no unread inside, but some flagged
color index         color37         color235        "~v~(~p)"                   # collapsed thread with unread parent, no unread inside, some to me directly
color index         color136        color160        "~v~(~D)"                   # thread with deleted (doesn't differentiate between all or partial)
color index         color160        default          "~T"                       # tagged messages
color index         color166        color160        "~="                       # duplicated messages
```
