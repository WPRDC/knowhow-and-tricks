# knowhow-and-tricks
Technical tricks for maintaining servers or whatever

## CKAN tricks (eventually this can be broken off into its own repository)
- [Speed up datastore_search queries](https://ckan.org/2017/08/10/faster-datastore-in-ckan-2-7/) by including the `include_total=False` parameter to skip calculation of the total number rows (which can reduce response time by a factor of 2).  The [datastore_search API call](https://docs.ckan.org/en/ckan-2.7.3/maintaining/datastore.html#ckanext.datastore.logic.action.datastore_search) lets you search a given datastore by column values and return subsets of the records. There's more on benchmarking CKAN performance [here](http://urbanopus.net/benchmarking-the-ckan-datastore-api/).
- Another way to speed up datastore search queries is to index fields used in the filtering. Note that (at least when the primary key is a combination of fields), if you don't list each primary key field as a separate field to index, those fields don't get indexed and queries take way longer.
- To avoid keeping local databases about datasets, store such information (such as the last time an ETL job was run on a given package) in the 'extras' metadata field of the CKAN package, as much as possible. This stores information in a centralized location so ETL jobs can be run from multiple computers without any other coordination. The extras metadata fields are currently cataloged [here](https://github.com/WPRDC/data-guide/blob/master/docs/metadata_extras.md).
- Queries/API responses can be cached based upon nginx settings. If you find that your SQL query is getting a stale response, try changing your query slightly. For instance, instead of `SELECT MAX(some_field) AS biggest FROM <resource-id>`, you could change the assigned variable name (`SELECT MAX(some_field) AS biggest0413 FROM <resource-id>`) or add another field that you ignore (`SELECT MAX(some_field) AS biggest, MAX(some_field) AS whatever FROM <resource-id>`).

## Server sorcery
- [Safest way to clean up boot partition - Ubuntu 14.04LTS-x64, Ubuntu 16.04LTS-x64](https://gist.github.com/ipbastola/2760cfc28be62a5ee10036851c654600)
- [Add new version of Python on Ubuntu 16.04LTS](https://linuxize.com/post/how-to-install-python-3-7-on-ubuntu-18-04/) - Note that it was necessary to run `sudo apt-get update` before finally being able to `sudo apt install python3.7`.
- Using Python 3.3+'s venv module is [recommended by official documentation](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/) for creating virtual environments, but it has issues under Ubuntu. One workaround is to just fall back to using the less limited `virtualenv` option.

## ETL tricks
- An ETL trick of last resort is screen-scraping a web site. To run such a scraping job in the background, one can a) use Selenium and b) use a headless browser. Here are [some tips on running Chrome headless with Selenium under Python](https://medium.com/@pyzzled/running-headless-chrome-with-selenium-in-python-3f42d1f5ff1d).

## Package management
- Use `python3.7 -m pip` or `./env/bin/python -m pip` to invoke the pip for a particular installation of Python.

## Field-testing
1) Create field-test branch to hold modified version of ETL job code.
2) Deploy to testing directory with `git clone -b field-test --single-branch git://sub.domain.com/repo.git`
3) Set up cron job to run field-test version of ETL job in `test` mode for a week (pushing data to a private CKAN package).
4) Compare the production and field-test versions of the resulting data tables to verify that the new code is working before pushing to master and deploying changes to production.

## Documentation
- [API documentation for the MOVEit FTP server](https://docs.ipswitch.com/MOVEit/Transfer2019_1/API/Rest/#_getapi_v1_files_id_download-1_0)
