[FrontPage](FrontPage.md)

# Whole block
It is the modularized layer structure which made the four following things one unit.<br>
At present, it has only a module relevant to "SIP." <br>
In the future, other things are due to be added.

- [B2BUA Layer](architecture#gf94fd32.md)
- [UA Layer](architecture#nde67d6f.md)
- [Stack Layer](architecture#fb15ebe6.md)
- [Transport Layer](architecture#ec301860.md)

### design
- [detail design](development/design.md)

# B2BUA Layer
This layer defines a sequence required as operation of UA.<br>
and it has responsibility about two or more transactions and dialogs.

# UA Layer
This layer implements the function in the single functional unit.<br>
It exists as a module (API) for B2BUA Layer to use it.

# Stack Layer
This layer has responsibility to processing of a transaction level. 

# Transport Layer
This layer treats processing of the Socket level which adopted [MINA](http://mina.apache.org/).
