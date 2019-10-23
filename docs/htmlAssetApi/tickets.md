<a name="GbaseTickets"></a>

## GbaseTickets
**Kind**: global class  

* [GbaseTickets](#GbaseTickets)
    * [new GbaseTickets()](#new_GbaseTickets_new)
    * [.sendTicket(receiverHumanId, header, payload, callback)](#GbaseTickets+sendTicket)
    * [.sendTicketVk(receiverVkId, header, payload, callback)](#GbaseTickets+sendTicketVk)
    * [.sendTicketOk(receiverOkId, header, payload, callback)](#GbaseTickets+sendTicketOk)
    * [.sendTicketFb(receiverFbId, header, payload, callback)](#GbaseTickets+sendTicketFb)
    * [.listSendedTickets(skip, limit, callback)](#GbaseTickets+listSendedTickets)
    * [.listReceivedTickets(skip, limit, callback)](#GbaseTickets+listReceivedTickets)
    * [.confirmTicket(ticketId, callback)](#GbaseTickets+confirmTicket)
    * [.confirmTicketVk(ticketId, callback)](#GbaseTickets+confirmTicketVk)
    * [.confirmTicketOk(ticketId, callback)](#GbaseTickets+confirmTicketOk)
    * [.confirmTicketFb(ticketId, callback)](#GbaseTickets+confirmTicketFb)
    * [.rejectTicket(ticketId, callback)](#GbaseTickets+rejectTicket)
    * [.rejectTicketVk(ticketId, callback)](#GbaseTickets+rejectTicketVk)
    * [.rejectTicketOk(ticketId, callback)](#GbaseTickets+rejectTicketOk)
    * [.rejectTicketFb(ticketId, callback)](#GbaseTickets+rejectTicketFb)
    * [.dischargeTicket(ticketId, callback)](#GbaseTickets+dischargeTicket)
    * [.dismissTicket(ticketId, callback)](#GbaseTickets+dismissTicket)
    * [.releaseTicket(ticketId, callback)](#GbaseTickets+releaseTicket)

<a name="new_GbaseTickets_new"></a>

### new GbaseTickets()
A section of API holding tickets mechanism

<a name="GbaseTickets+sendTicket"></a>

### gbaseTickets.sendTicket(receiverHumanId, header, payload, callback)
Send non-social ticket to some player through providing his human-ID. Ticket is a form of communication which helps you to
send some schema-less data to players. Using this feature you can build trading or presents system.
Ticket have limited life time configured on backend side.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| receiverHumanId | <code>Number</code> | self titled |
| header | <code>String</code> | a user-defined header that will be visible for communicating |
| payload | <code>Object</code> | all the data you consider to provide |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+sendTicketVk"></a>

### gbaseTickets.sendTicketVk(receiverVkId, header, payload, callback)
Send ticket to some player linked with VK.com through providing his VK.com ID. Ticket is a form of communication which helps you to
send some schema-less data to players. Using this feature you can build trading or presents system.
The ticket will be available for player even if he is not yet linked with VK.com(or if there is no such player yet).
After signing up player anyway will receive this ticket.
Ticket have limited life time configured on backend side.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| receiverVkId | <code>String</code> | self titled |
| header | <code>String</code> | a user-defined header that will be visible for communicating |
| payload | <code>Object</code> | all the data you consider to provide |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+sendTicketOk"></a>

### gbaseTickets.sendTicketOk(receiverOkId, header, payload, callback)
Send ticket to some player linked with OK.ru through providing his OK.ru ID. Ticket is a form of communication which helps you to
send some schema-less data to players. Using this feature you can build trading or presents system.
The ticket will be available for player even if he is not yet linked with VK.com(or if there is no such player yet).
After signing up player anyway will receive this ticket.
Ticket have limited life time configured on backend side.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| receiverOkId | <code>String</code> | self titled |
| header | <code>String</code> | a user-defined header that will be visible for communicating |
| payload | <code>Object</code> | all the data you consider to provide |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+sendTicketFb"></a>

### gbaseTickets.sendTicketFb(receiverFbId, header, payload, callback)
Send ticket to some player linked with Facebook through providing his Facebook ID. Ticket is a form of communication which
helps you to send some schema-less data to players. Using this feature you can build trading or presents system.
The ticket will be available for player even if he is not yet linked with VK.com(or if there is no such player yet).
After signing up player anyway will receive this ticket.
Ticket have limited life time configured on backend side.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| receiverFbId | <code>String</code> | self titled |
| header | <code>String</code> | a user-defined header that will be visible for communicating |
| payload | <code>Object</code> | all the data you consider to provide |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+listSendedTickets"></a>

### gbaseTickets.listSendedTickets(skip, limit, callback)
You can list your sended tickets with all useful information like ticket ID (tid), data and recipient's reaction on it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| skip | <code>Number</code> | <code>0</code> | pagination skip |
| limit | <code>Number</code> | <code>20</code> | pagination limit |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+listReceivedTickets"></a>

### gbaseTickets.listReceivedTickets(skip, limit, callback)
You can list your inbox of tickets with all useful information.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| skip | <code>Number</code> | <code>0</code> | pagination skip |
| limit | <code>Number</code> | <code>20</code> | pagination limit |
| callback | <code>function</code> |  | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+confirmTicket"></a>

### gbaseTickets.confirmTicket(ticketId, callback)
A conditional action of confirming non-social ticket. It will do nothing special to interlocutors but you can base your
domain logic on this confirm/reject reaction.
A ticket will be marked as confirmed on backend and both participants will see it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+confirmTicketVk"></a>

### gbaseTickets.confirmTicketVk(ticketId, callback)
A conditional action of confirming VK.com ticket. It will do nothing special to interlocutors but you can base your
domain logic on this confirm/reject reaction.
A ticket will be marked as confirmed on backend and both participants will see it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+confirmTicketOk"></a>

### gbaseTickets.confirmTicketOk(ticketId, callback)
A conditional action of confirming OK.ru ticket. It will do nothing special to interlocutors but you can base your
domain logic on this confirm/reject reaction.
A ticket will be marked as confirmed on backend and both participants will see it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+confirmTicketFb"></a>

### gbaseTickets.confirmTicketFb(ticketId, callback)
A conditional action of confirming Facebook ticket. It will do nothing special to interlocutors but you can base your
domain logic on this confirm/reject reaction.
A ticket will be marked as confirmed on backend and both participants will see it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+rejectTicket"></a>

### gbaseTickets.rejectTicket(ticketId, callback)
A conditional action of rejecting non-social ticket. It will do nothing special to interlocutors but you can base your
domain logic on this confirm/reject reaction.
A ticket will be marked as rejected on backend and both participants will see it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+rejectTicketVk"></a>

### gbaseTickets.rejectTicketVk(ticketId, callback)
A conditional action of rejecting VK.com ticket. It will do nothing special to interlocutors but you can base your
domain logic on this confirm/reject reaction.
A ticket will be marked as rejected on backend and both participants will see it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+rejectTicketOk"></a>

### gbaseTickets.rejectTicketOk(ticketId, callback)
A conditional action of rejecting OK.ru ticket. It will do nothing special to interlocutors but you can base your
domain logic on this confirm/reject reaction.
A ticket will be marked as rejected on backend and both participants will see it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+rejectTicketFb"></a>

### gbaseTickets.rejectTicketFb(ticketId, callback)
A conditional action of rejecting Facebook ticket. It will do nothing special to interlocutors but you can base your
domain logic on this confirm/reject reaction.
A ticket will be marked as rejected on backend and both participants will see it.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+dischargeTicket"></a>

### gbaseTickets.dischargeTicket(ticketId, callback)
Sender can discharge ticket - basically cancel it while it is not confirmed or rejected.
Be ready to get a logic error with index 266 - it means that no ticket meets requirements, it was confirmed or rejected,
been discharged previously or just unknown ticket ID.
Ticket will be removed immediately.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+dismissTicket"></a>

### gbaseTickets.dismissTicket(ticketId, callback)
Sender to close rejected ticket. No affect on player data unless you'll implement it by yourself.
Be ready to get a logic error with index 271 - it means that no ticket meets requirements, it was not rejected,
been dismissed previously or just unknown ticket ID.
Ticket will be removed immediately.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |

<a name="GbaseTickets+releaseTicket"></a>

### gbaseTickets.releaseTicket(ticketId, callback)
Sender to close confirmed ticket. No affect on player data unless you'll implement it by yourself.
Be ready to get a logic error with index 276 - it means that no ticket meets requirements, it was not confirmed,
been released previously or just unknown ticket ID.
Ticket will be removed immediately.

**Kind**: instance method of [<code>GbaseTickets</code>](#GbaseTickets)  

| Param | Type | Description |
| --- | --- | --- |
| ticketId | <code>Number</code> | you can receive this parameter on callback for sending and on listing("tid" node) |
| callback | <code>function</code> | standard callback(GbaseError err, GbaseResponse response) |