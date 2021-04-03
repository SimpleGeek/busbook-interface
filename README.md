# BusBook

This is the web interface for the BusBook application.

## Issues
It is expected that all open issues will be re-evaluated and worked after version
1.0.0 is released.

## Hints

### Fetch RESTful calls
After some annoyance, I discovered you must initialize the the parent object you
are retrieving from the api with an empty array for whatever list you want to iterate
over from said object.  Otherwise, you get bizarre "ctx undefined" errors.  Also,
create an async function to run the fetch and set the script-level object from within
itself, as opposed to returning a value.
