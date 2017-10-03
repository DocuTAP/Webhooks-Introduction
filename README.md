# Webhooks in Clockwise.MD
This is a concise introduction to webhook integrations with Clockwise.MD.

## What are webhooks?
A simple notification from us that something about an appointment has changed.
Unlike polling, it is initiated by our servers.

## What is polling?
Polling is when an outside system makes a web request to track changes
(in our case, typically in the form of a REST API call).

## Why webhooks versus regular polling?
Webhooks are useful for keeping track of appoinments as they flow through
Clockwise.MD, and are especially good for granularly tracking them without
increasing your polling frequency. To know what happens to an appointment
every 5 seconds with polling, you need to make a web request every 5 seconds,
whereas with a webhook we notify you the moment a change has been made.

## How do I get started with webhooks?
Webhooks, unlike polling, requires you give us an API endpoint that accepts
a POST request. Here's an example of a url you could supply us:

`https://hooks.example.com/webhook/`

How your server processes this is entirely up to you, but our
only requirement is that it accepts `Content-Type: 'application/json'`.

Once you've supplied us with your webhook URL, you'll be able to receive
POST requests every time an appointment record changes state.  The following
is an example of the content you can expect to receive:

```
{
  "appointment_id": 12345,
  "event_type":     "remove",
}
```

Currently supported Event Types are as follows:

```
create
reschedule
demographics_update
checkin
undo_checkin
remove
undo_remove
callback
undo_callback
discharge
undo_discharge
```

## I'm receiving data from the webhooks!  Now what?
That's entirely up to you. Knowing that an action has taken place gives you
the opportunity to be quicker to react to changes in Clockwise.

An example setup might be that your webhook only cares about the `create`
Event Type, so whenever you receive an Appointment ID with the `create` Event
Type, you can use Clockwise.MD's existing REST API to query for the
appointment's demographics information and update an internal database.

## Security
For security reasons, you may *not* want to expose your webhook endpoint for
general consumption by the open internet. Interally to your application, you
can mantain a list of supported vendors by keys:

`https://hooks.example.com/webhook/CF3uQP8cSFIQB8gB/9285030fceca`
