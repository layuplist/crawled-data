# Layup List Data

This branch holds the crawled data used in Layup List. They are imported using the importers in the `master` branch.

This includes:
* ORC course data
* Term offerings
* Course medians
* Old layup lists and reviews

In addition to the Python dependencies defined in `master`, running the crawling scripts requires **PhantomJS**.

## Updating

* **New Term Timetable is released**

  Occurs near the middle of each term for the next term, twice in the spring (for summer and fall)
  
  If on production, backup the database on Heroku before these steps.
  
  To update Layup List to new term:
  * Modify `scripts/dev/crawl_data.sh` to crawl the new term
  * Run `scripts/dev/crawl_data.sh`. This script crawls all data, not just the new term
  * Check `data/` for irregularities (`git diff`)
  * Import the crawled data into the database:
  
    ```bash
    python scripts/importers/import_orc_courses.py
    python scripts/importers/import_course_pages.py
    python scripts/importers/import_term.py [TERM] [TERM_COURSE_FILE]
    ```
  * Push the `data` branch if there are no irregularities and importing succeeds
  * A new line should be added to `scripts/dev/import_initial_data.sh` to import the new term
  * `CURRENT_TERM` in `lib/constants.py` should be updated to the latest term
  
* **New medians are released**

  Occurs near the beginning of each term.

  If on production, backup the database on Heroku before these steps. 
  
  To update Layup List with the new medians:
  * Modify `scripts/dev/crawl_data.sh` to have the correct parameters on `crawl_medians.js`. 
  * Run `scripts/dev/crawl_data.sh`. This script crawls all data, not just the new medians.
  * Check `data/` for irregularities (`git diff`)
  * Import the crawled data into the database:
  
    ```bash
    python scripts/importers/import_orc_courses.py
    python scripts/importers/import_course_pages.py
    python scripts/importers/import_medians.py
    ```
    
  * Push the `data` branch if there are no irregularities and importing succeeds
  
