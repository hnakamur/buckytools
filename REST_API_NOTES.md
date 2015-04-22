Buckyd REST API Specification
=============================

/metrics
--------

Returns a JSON array listing the metrics on the local host.  May return a status
code of 202 Accepted when the internal cache is being rebuilt.  In that case
the client should sleep and try again.

Methods:

* GET
* POST

Query Parameters:

* list - This should be a JSON encoded array of Graphite metric keys.  The
  request will return any metrics in the list that are also present in the
  local store.
* force - Force a cache rebuild.  This will force the API to return a status
  code of 202 Accepted.
* regex - A regular expression.  Metric keys found locally that match this
  expression will be returned.

/metrics/<metric.key>
---------------------

Operates on specific metrics.  The metric key is the Graphite metric key
or name and not a file path.

Methods:

* GET - Fetch the raw Whisper DB file.
* PUT - Replace the raw Whisper DB with supplied content
* POST - Update the Whisper DB by backfilling the on disk version.  Does not
  overwrite existing points, but will fill in data if the matching on disk
  data point is null.  See Carbonate's whisper-fill.py.
* DELETE - Remove this metric from the file system