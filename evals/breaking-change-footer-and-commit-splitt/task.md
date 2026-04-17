# User API Response Cleanup

## Problem/Feature Description

Your team maintains a user management service consumed by multiple internal clients. During a recent API review, it was agreed that the `username` field returned by the `/users/:id` endpoint was ambiguous — it actually stored the user's display handle, not their login username. The decision was made to rename it to `handle` across the response schema to better reflect its meaning.

While a junior developer was preparing this change, they also noticed and corrected a typo in the project README (`"libary"` → `"library"`). Both changes ended up in the same working branch and have been staged together.

You have been asked to produce the commit message(s) for this staged diff. Save your output to a file named `commit_message.txt`. If you produce more than one message, separate them with a line containing only `---`.

## Output Specification

- `commit_message.txt` — the commit message or messages to use for these changes

## Input Files

The following files are provided as inputs. Extract them before beginning.

=============== FILE: inputs/diff.patch ===============
diff --git a/src/api/users.js b/src/api/users.js
index 1234567..abcdef0 100644
--- a/src/api/users.js
+++ b/src/api/users.js
@@ -10,7 +10,7 @@ function serializeUser(user) {
   return {
     id: user.id,
-    username: user.name,
+    handle: user.name,
     email: user.email,
     created_at: user.createdAt,
   };
diff --git a/docs/README.md b/docs/README.md
index 9876543..fedcba0 100644
--- a/docs/README.md
+++ b/docs/README.md
@@ -1,5 +1,5 @@
 # User Management Service
-This libary provides utilities for managing users, profiles, and permissions.
+This library provides utilities for managing users, profiles, and permissions.
