# Tickets

Are another player's interaction mechanism. You can consider them as email sent by either `Human ID` or social ID. They have useful payloads and can be reacted by recipients what gives you a possibility to build an exchange/gift system.

## Send ticket

The ticket can be sent using these coordinates: **human ID**, **VK ID**, **OK ID** or **Facebook ID**. And a social profile doesn't need to exist. All tickets can be sent on social IDs even before the actual registration of the recipient and read later. Tickets have limited ttl - configured on the backend side.
```javascript
/* Skipping GbaseApi init */

var recipientHumanId,	// Let's imagine that we have all of these values
	recipientFbId,
	recipientVkId,
	recipientOkId;

gbaseApi.tickets.sendTicket(recipientHumanId, 'A ticket\'s header', { useful: 'payload' }, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// We sent a ticket with useful payload to some player by known human ID
});
gbaseApi.tickets.sendTicketFb(recipientFbId, 'A ticket\'s header', { useful: 'payload' }, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// We sent a ticket with useful payload to some player by known Facebook ID
});
gbaseApi.tickets.sendTicketVk(recipientVkId, 'A ticket\'s header', { useful: 'payload' }, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// We sent a ticket with useful payload to some player by known VK.com ID
});
gbaseApi.tickets.sendTicketOk(recipientOkId, 'A ticket\'s header', { useful: 'payload' }, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// We sent a ticket with useful payload to some player by known OK.ru ID
});
```

## List tickets

You can list sended and received tickets using standard pagination mechanism. Only tickets without reaction are listed.
```javascript
/* Skipping GbaseApi init */

gbaseApi.tickets.listSendedTickets(0, 20, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// We can access our sended tickets here: response.details.originalResponse
});
gbaseApi.tickets.listReceivedTickets(0, 20, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// We can access our received tickets here: response.details.originalResponse
});
```

## Confirming tickets

The recipient can confirm the ticket as an act of positive reaction - the sender will find out about it. Ticket disappears from listings. Use `ticket ID` to refer and 4 methods to interact, depends on receiver id(human ID or one of the social IDs) 
```javascript
/* Skipping GbaseApi init */

var ticketSendedOnHumanIdTicketId,		// Let's imagine that we already have these IDs
	ticketSendedOnFacebookIdTicketId,
	ticketSendedOnVkIdTicketId,
	ticketSendedOnOkIdTicketId;

gbaseApi.tickets.confirmTicket(ticketSendedOnHumanIdTicketId, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Ticket is confirmed and no more listed
});
gbaseApi.tickets.confirmTicketFb(ticketSendedOnFacebookIdTicketId, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Ticket is confirmed and no more listed
});
gbaseApi.tickets.confirmTicketVk(ticketSendedOnVkIdTicketId, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Ticket is confirmed and no more listed
});
gbaseApi.tickets.confirmTicketOk(ticketSendedOnOkIdTicketId, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Ticket is confirmed and no more listed
});
```

## Rejecting tickets

The recipient can reject tickets as an act of negative reaction - the sender will find out about it. Ticket disappears from listings. Use `ticket ID` to refer and 4 methods to interact, depends on receiver id(human ID or one of the social IDs)
```javascript
/* Skipping GbaseApi init */

var ticketSendedOnHumanIdTicketId,		// Let's imagine that we already have these IDs
	ticketSendedOnFacebookIdTicketId,
	ticketSendedOnVkIdTicketId,
	ticketSendedOnOkIdTicketId;

gbaseApi.tickets.rejectTicket(ticketSendedOnHumanIdTicketId, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Ticket is confirmed and no more listed
});
gbaseApi.tickets.rejectTicketFb(ticketSendedOnFacebookIdTicketId, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Ticket is confirmed and no more listed
});
gbaseApi.tickets.rejectTicketVk(ticketSendedOnVkIdTicketId, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Ticket is confirmed and no more listed
});
gbaseApi.tickets.rejectTicketOk(ticketSendedOnOkIdTicketId, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Ticket is confirmed and no more listed
});
```

## Discharging ticket

Basically cancel sended ticket by a sender without any particular reaction by recipient.
```javascript
/* Skipping GbaseApi init */

var aTicketIdToDischarge;	// Let's imagine that we already have this value

gbaseApi.tickets.dischargeTicket(aTicketIdToDischarge, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Will be successful only if ticket have no reaction on it. Otherwise you'll get an error with index 266
});
```

## Dismissing and Releasing ticket

Basically, confirm sent tickets with some recipient's reaction. Dismissing for confirming a ticket with negative reaction and releasing for the ticket with positive reactions. By itself, they do nothing to player data but you can build domain logic around these methods.
```javascript
/* Skipping GbaseApi init */

var aTicketIdToDismiss,	// Let's imagine that we already have this value
	aTicketIdToRelease;

gbaseApi.tickets.dismissTicket(aTicketIdToDismiss, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Will be successful only if ticket have negative reaction on it. Otherwise you'll get an error with index 271
});
gbaseApi.tickets.releaseTicket(aTicketIdToRelease, (err, response) => {
	if(err){
		console.error('Oops!');
		process.exit(-1);
	}

	// Will be successful only if ticket have positive reaction on it. Otherwise you'll get an error with index 276
});
```