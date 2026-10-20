# Web Activity Log Analysis

**Source:** Commonwealth Bank Cybersecurity Job Simulation (Forage)  
**Status:** Learning notes from the supplied training log and guide

## What the guide shows

The supplied guide illustrates a recurring login sequence:

1. A client requests the dashboard path `/` and receives **401 Unauthorized** because it is not logged in.
2. The client loads `/login` and its CSS/JavaScript resources, which return **200**.
3. A `POST /login` request returns **200**, followed by a successful dashboard request.
4. Requests then access factory and machine status API endpoints with an `authorizedUserId` value.
5. On a later day, a new request to `/` returns **401** again, consistent with an expired session or a new unauthenticated session.

The log contains multiple client addresses and multiple user/session identifiers. These should not be treated as one person or one continuous session without additional evidence.

## What the status codes mean here

- **401 Unauthorized:** the request did not have an accepted authenticated session. In the illustrated sequence, this occurs before login and again on a later day.
- **200 Success:** the server returned a successful response. This does not independently prove that the request was benign or malicious.
- **`authorizedUserId`:** the field appears alongside requests after login. It is a correlation clue, but the value alone does not establish a real-world identity.

## What I can conclude

The sequence is consistent with a web application that requires login before dashboard access, then serves dashboard assets and factory/machine status API responses after authentication. A later 401 followed by the login page is consistent with a new session or expired authentication.

This evidence alone does **not** prove fraud, account compromise, or malicious access. To investigate an anomaly, I would compare the client address, user/session identifier, timestamps, requested factory/machine values, and any related authentication or application events.

## How I would investigate further

- Group events by client address and `authorizedUserId`, without assuming they represent the same person.
- Compare timestamps and distinguish separate days and sessions.
- Review whether the requested factory and machine endpoints fit the expected activity for that account.
- Look for unusual request frequency, unexpected access patterns, and related authentication failures.
- Correlate with other logs before making a security conclusion.

## Evidence and privacy

The original `web_activity.log` contains private-network IP addresses and user/session identifiers. Do not publish the raw log. If adding evidence to this repository, use a redacted excerpt with consistent placeholders and keep the full training file private.

This is an analysis of the supplied simulation data, not a claim that I investigated Commonwealth Bank's live systems or detected real fraud.
