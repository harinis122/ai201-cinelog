# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:**
I renamed save_to_watchlist() to add_to_watchlist() in services/watchlist_service.py and updated all call sites.
**How I verified:**
I used VS Code's find-all-references search to confirm that I didn't miss any save_to_watchlist function calls.

## Comment 2 — Deduplication
**What I did:**
I added a deduplication check to add_to_watchlist function in watchlist_service.py. I mirrored the way add_to_collection was done to keep the code consistant.
**How I verified:**
I wrote a test case in test_watchlist.py confirming that adding duplicate films to the watchlist throws AlreadyInWatchlistError. This test case passed.


## Comment 3 — Missing test
**What I did:**
I created test_watchlist.py and added test_add_to_watchlist_nonexistent_film_raises test case.

**How I verified:**
I verified that this new test case passes, and that all test cases in test_watchlist.py pass and follow consistant formatting as test_collection.py.

## Comment 4 — Default visibility
**My position:**
I would change the default to public=False in watchlist_service.md.

**Reasoning:**
I think watchlists should be private by default because users may save films for personal reasons and may not expect that activity to be visible to other people. A private default gives users more control over what they share and avoids exposing their watchlist without an explicit choice. This would not necessarily cut down on the social aspect of this app as users are still sharing the movies in their collection, and setting public=False would give the user the ability to explore movies in private. Of course, they can also manually change it to public if they specifically want their friends to see their watchlist. I'm prioritizing personal privacy.

**Tradeoff acknowledged:**
The downside is that users who want to share their watchlists with friends will need to take an extra step to make them public. This creates slightly more friction for CineLog’s social features, but I think protecting user privacy by default is the safer choice.


## Comment 5 — Sort order
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->