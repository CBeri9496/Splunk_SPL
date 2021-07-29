# Splunk_SPL

Here I am writing tstats queries:

Find list of indexes count

|tstats count where index=* OR index=_* by index

The above query results the list of indexes with record counts in each index

