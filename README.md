# knowhow-and-tricks
Technical tricks for maintaining servers or whatever

## Server sorcery
- [Safest way to clean up boot partition - Ubuntu 14.04LTS-x64, Ubuntu 16.04LTS-x64](https://gist.github.com/ipbastola/2760cfc28be62a5ee10036851c654600)
- [Add new version of Python on Ubuntu 16.04LTS](https://linuxize.com/post/how-to-install-python-3-7-on-ubuntu-18-04/) - Note that it was necessary to run `sudo apt-get update` before finally being able to `sudo apt install python3.7`.
- Using Python 3.3+'s venv module is [recommended by official documentation](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/) for creating virtual environments, but it has issues under Ubuntu. One workaround is to just fall back to using the less limited `virtualenv` option.

## Package management
- Use `python3.7 -m pip` or `./env/bin/python -m pip` to invoke the pip for a particular installation of Python.

## Field-testing
1) Create field-test branch to hold modified version of ETL job code.
2) Deploy to testing directory with `git clone -b field-test --single-branch git://sub.domain.com/repo.git`
3) Set up cron job to run field-test version of ETL job in `test` mode for a week (pushing data to a private CKAN package).
4) Compare the production and field-test versions of the resulting data tables to verify that the new code is working before pushing to master and deploying changes to production.
