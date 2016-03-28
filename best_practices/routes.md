# Route to Every Destination

Where a given UI state can be considered a destination:

- Do not transition UI state from methods handling a user interaction
- User interaction should generate a route and call `redirectTo`
- Route handler handles every step necessary to realize desired UI state
- Ensure all route handlers work correctly when navigated to forwards, backwards, directly, and from arbitrary routes in other sections