# ADR-239: Show people's activity per team by default, and names only after a DPIA

## Status

accepted (2026-11-21). Applies to every product in the line that shows people's activity.

## Context

Several products in the line show what people do: Themis.NET makes developers' agent sessions visible, and others may show runs, decisions or usage per person. In the Netherlands and the EU, monitoring named employees needs a lawful basis, often a data protection impact assessment (DPIA), and in the Netherlands consent of the works council (ondernemingsraad) for a system that can monitor staff. A product that shows named people by default puts that burden on every customer from day one.

## Decision

* A product shows people's activity per team by default, without names. This holds for overviews and also for a single item, such as one session in Themis.NET.
* Showing names is a setting per team or node. Turning it on requires an approval reference (for example the DPIA or works council decision), which the product checks is present.
* A team view needs at least 3 people. A smaller team is shown only as part of the level above it.
* The visibility in force is stamped on each activity record when it is recorded. Switching back to per team hides names at once, also for older records.
* A person always sees their own activity.

## Consequences

* Customers can start with names off, and turn them on deliberately. The approval reference is a gate in the product, not an exemption: the customer stays responsible for its own assessment, such as a DPIA, and the product only checks that a reference is present.
* For Themis.NET, a team lead sees the full sessions of the team, but not whose they are, until names are turned on.
* While names are off, a product also hides the identifiers it knows that point to a person, such as the git author, branch name, computer name and the person's account in repository remotes. Free text a person typed (a prompt) is shown as it is. *Which identifiers exactly, per product, is decided in that product's Spec.*
* A team lead can approve, deny or pause the session of an unnamed developer; "a developer in your team" is enough.
