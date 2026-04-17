# Auth Session Alignment

## Problem/Feature Description

Your team recently received a support escalation from several enterprise customers complaining that they are being logged out unexpectedly during long working sessions. After investigation, an engineer discovered that the JWT token lifetime was set to 15 minutes while the session cookie was configured for 24 hours. Once the JWT expired, the application silently failed to refresh it, kicking users out mid-session.

The engineer has pushed a fix to the `auth` module. The fix aligns the JWT lifetime with the session cookie duration and configures a proactive background refresh two minutes before expiry. While making this change, the engineer also cleaned up the `validateToken` helper to remove a redundant try/catch wrapper that was obscuring the return value.

You have been asked to write the git commit message for these staged changes.

## Output Specification

Write the commit message and save it to a file named `commit_message.txt` in your working directory.

## Input Files

The following files are provided as inputs. Extract them before beginning.

=============== FILE: inputs/diff.patch ===============
diff --git a/src/auth/jwt.js b/src/auth/jwt.js
index 3a1b2c4..8d9e0f1 100644
--- a/src/auth/jwt.js
+++ b/src/auth/jwt.js
@@ -1,18 +1,18 @@
-const JWT_TTL = 15 * 60; // 15 minutes
+const SESSION_TTL = 24 * 60 * 60; // 24 hours - match session cookie
+const JWT_TTL = SESSION_TTL;

 function createToken(userId) {
-  return jwt.sign({ userId }, SECRET, { expiresIn: JWT_TTL });
+  return jwt.sign(
+    { userId },
+    SECRET,
+    { expiresIn: JWT_TTL, refreshBefore: JWT_TTL - 120 }
+  );
 }

-function validateToken(token) {
-  try {
-    return jwt.verify(token, SECRET);
-  } catch (e) {
-    return null;
-  }
+function validateToken(token) {
+  const result = jwt.verify(token, SECRET);
+  if (!result) return null;
+  return result;
 }

 module.exports = { createToken, validateToken };
