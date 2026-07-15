# PR Response Doc — CineLog Watchlist Feature

## AI Usage
I used claude minimally during this project just to understand various git commands because I was unfamiliar with rebase, checkout, etc. I also used claude to verify my git commits comply with standards, making my commits both consistant with industry standards and with this project.

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
I would keep the watchlist films ordered alphabetically.

**Reasoning:**
Keeping the watchlist films ordered alphabetically ensures that the films stay in a consistant, predictable order. If a user is looking specifically for a film, they can just browse through the watchlist, skipping forward to a particular section depending on the starting letter of the movie. If the user wanted to watch a film but does not know when they added it to the watchlist, sorting by date added would force them to scroll through their entire watchlist, whereas the film would be easier to find if the watchlist was sorted alphabetically.

**Engagement with reviewer's point:**
Although having the films sorted by date added might be convenient for users wanting to watch films they most recently added to their watchlist, this ordering will likely be inconvenient for users wanting to browse through their watchlist and pick a film since it the order of films will not be structured well. Also, sorting watchlist films by date added will make it more likely users will just pick more recently added films from their watchlist to watch, making it likely that watchlist films from a while ago will not be watched.


## Comment 6 — Rebase
**What conflicted:**
I did not get any rebase conflicts.

**How I resolved it:**
N/A

**How I verified no conflict remains:**
N/A

## PR Description
Overview
This pull request adds the CineLog watchlist feature. Users can add films to their watchlist and retrieve their saved watchlist entries through the watchlist API endpoints. The implementation follows the existing collection-service patterns, prevents duplicate watchlist entries, validates that films exist before saving them, and supports the UUID-based film IDs introduced on main.

Design Decisions
Visibility default
I chose to make new watchlist entries private by default.

I made this choice because I wanted to optimize for user privacy. The tradeoff is that users will have to take an extra step to share their watchlist.

Sort order
I chose to return watchlist entries in alphabetical order.

I chose this order to make sure that entries are in a consistent and reliable order. I considered the reviewer’s preference for order by"date added", but decided that overall watchlist order consistency when wanting to watch an earlier watchlist addition is more important, while acknowledging that recent watchlist additions will not conveniently remain at the top.

Manual Testing
Check out the feature branch:

git checkout feature/watchlist
Create and activate the virtual environment:

python -m venv .venv
source .venv/bin/activate
Install the project dependencies:

pip install -r requirements.txt
Start the application:

python app.py
In a second terminal, add an existing film to a user’s watchlist:

curl -X POST http://127.0.0.1:5000/watchlist/1/add \
  -H "Content-Type: application/json" \
  -d '{"film_id":"REPLACE_WITH_AN_EXISTING_FILM_UUID"}'
Retrieve the user’s watchlist:

curl http://127.0.0.1:5000/watchlist/1
Confirm that the newly added film appears and that the entries use the documented visibility default and sort order.

Send the same add request again and confirm that a duplicate watchlist entry is not created.

Try adding a nonexistent film UUID and confirm that the API returns the expected error response.

Run the complete automated test suite:

pytest tests/ -v