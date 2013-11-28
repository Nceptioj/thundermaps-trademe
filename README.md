Thundermaps-TradeMe
===================

This repository provides Python modules for using the TradeMe API to get property listings and using the ThunderMaps API to post reports, and a module that periodically creates Thundermaps reports for the latest TradeMe listings.

Dependencies
------------

* The `requests` and `requests_oauthlib` libraries for Python.
* A Thundermaps API key and account ID.
* *(Optional)* A TradeMe API key, API secret, OAuth token, and OAuth secret.

Usage
-----

### TradeMe module

To use the TradeMe module, import it into your code using `import trademe`.

To get listings for a certain category, you can use the `getListings` method:

```python
import trademe

# Categories.
TRADEME_CATEGORY_RENTAL = 4233

# Get rentals from TradeMe.
listings = trademe.getListings(category_id=TRADEME_CATEGORY_RENTAL, limit=10)
```

If you have a TradeMe developer account and have generated an OAuth token and OAuth secret, then you can authenticate with the TradeMe API prior to making a request in order to increase you rate limit and the number of results you can get per API call. E.g.

```python
import trademe

# Replace ... with the actual values.
TRADEME_API_KEY = "..."
TRADEME_API_SECRET = "..."
TRADEME_OAUTH_KEY = "..."
TRADEME_OAUTH_SECRET = "..."

# Categories.
TRADEME_CATEGORY_RENTAL = 4233

# Authenticate with TradeMe.
trademe.authenticate(TRADEME_API_KEY, TRADEME_API_SECRET, TRADEME_OAUTH_KEY, TRADEME_OAUTH_SECRET)

# Get rentals from TradeMe.
listings = trademe.getListings(category_id=TRADEME_CATEGORY_RENTAL, limit=10)
```

### Thundermaps module

To use the Thundermaps module, import it into your code using `import thundermaps`. All of the methods in this module require your Thundermaps API key as the first argument. E.g.

```python
import thundermaps

# Replace ... with the actual values.
THUNDERMAPS_API_KEY = "..."
ACCOUNT_ID = ...

# Get reports for an account.
reports = thundermaps.getReports(THUNDERMAPS_API_KEY, ACCOUNT_ID)
```

### Updater module

The updater module combines both the TradeMe and ThunderMaps module and provides a higher level interface for generating ThunderMaps reports for the latest TradeMe listings.
Using the updater module typically consists of these steps:

* Creating a new instance of `Updater` with a ThunderMaps API key.
* *(Optional)* Authenticating with TradeMe.
* Adding categories to generate reports for.
* Starting the updater.

An example usage is shown below.

```
import updater

# Define categories, keys, and accounts here.

# Create updater and authenticate.
properties_updater = updater.Updater(THUNDERMAPS_API_KEY)
properties_updater.authenticate(TRADEME_API_KEY, TRADEME_API_SECRET, TRADEME_OAUTH_KEY, TRADEME_OAUTH_SECRET)

# Add categories.
properties_updater.add_category("rentals", TRADEME_CATEGORY_RENTAL, THUNDERMAPS_ACCOUNT_RENTALS, THUNDERMAPS_CATEGORY_RENTAL)

# Start updating.
properties_updater.start()
```
