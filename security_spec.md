# Firebase Firestore security specification for Chef's Gallery

## 1. Data Invariants
- A pantry document belongs to the signed-in user, and its ID matches the user's Auth UID.
- A favorite recipe belongs to the signed-in user, and its `userId` field matches request.auth.uid.

## 2. The "Dirty Dozen" Payloads (Vulnerable Requests)
All the following payloads should be blocked by security rules:
1. Setting another user's `userId` in `pantries/{userId}`.
2. Writing `pantries` without active login.
3. Reading another user's `pantry` list.
4. Setting a massive `ingredients` list (over 100 items).
5. Setting invalid types (e.g., non-array ingredients in pantry).
6. Creating a `favorite` recipe without a valid integers recipeId.
7. Modifying a `favorite` record's `userId` to point to a different owner.
8. Updating standard `createdAt` field on favorites (immutability rule).
9. Writing custom system claims into firebase files.
10. Fetching full collections list without matching user ID filters.
11. Bypassing size restrictions on string fields.
12. Attempting to update favorite field with malicious 5MB payload.

## 3. Firestore Rules
Let's establish the fortress rules in the firestore.rules file.
