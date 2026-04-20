[development/design](../design.md)

# Class relations
- For extended relations, please look at a [Class figure](classFigure.md)
- For details, please look at [JavaDoc](http://www.siprop.org/ja/2.0/javadoc/SIProp-2.0)



# Starting & initialization part
### B2BUAMain class
- This is a class with the main method for starting. 
    - This processes instance-ization of Provider. 

### Provider interface
- This carries out all the initializes.

### Repository interface
- This is initialized in every layer (Manager interface).

# Message part
### Message interface
- This is a super class of all the messages.

### Context interface
- This is a message holding the information over between layers.
    - Information until it receives from Opposite UA and is transmitted to Opposite UA.

### Packet interface
- This is a message holding the information used only within a layer.
    - Information until it receives from Router and is transmitted to Router.

### FlatMessage interface
- This is a message showing the normalized message. 

### Info interface
- This is the message which extracted only required information from the FlatMessage interface.

### ControlMessage interface
- This is a message how it should operate to a layer, and for directing.

# Router part
### Router interface
- This carries out routing processing of a message.

### RouterInfo interface
- This holds routing information. 

### Listener interface
- This is an interface showing the entry point of the routing point.

### RouteKey interface
- This is an interface holding the routing information which a Router interface uses.

### SendToKey class
- This is a class holding src and dst between layers. 


# B2BUA Layer
### B2BUAManager interface
- This is an interface used as the entry point of the Router interface for B2BUA layer. 

### B2BUAModule interface
- This is an interface for defining B2BUA sequence. 

### B2BUAContext interface
- This is an interface for holding required dialog information.

### B2BUAInfo interface
- This is a ControlMessage unit and is an interface for holding information. 


# UA Layer
### UAManager interface
- This is an interface used as the entry point of the Router interface for UA layer. 

### UACommand interface
- This is an interface which defines and processes operation of a message.

### UAContext interface
- This is an interface for holding required dialog information.

### UAInfo interface
- This is a ControlMessage unit and is an interface for holding information. 


# Stack Layer
### StackManager interface
- This is an interface used as the entry point of the Router interface for Stack layer. 

### TransactionUser interface
- This is an interface showing TU of SIP.

### TransactionController interface
- This is an interface which exists for forking.

### TransactionEntry interface
- This is an interface for describing concrete transaction processing.

### TransactionInfo interface
- This is an interface for holding information required of TransactionEntry. 

# Transport Layer
- This is MINA base class.
### TransportManager interface
- This is an interface used as the entry point of the Router interface for Transport layer.
