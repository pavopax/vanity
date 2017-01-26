# What

You can now get PyPi download counts for multiple packages, outputted into a single file, `results.txt`

Sample Usage:

	python vanity.py numpy              # one
	python vanity.py numpy pandas       # multiple
	python vanity.py numpy pandas -q	# quiet, return only total


# Details

See [this commit](https://github.com/pavopax/vanity/commit/316cd549b1af4d8980f5cd4fc6e7f2851bb83b74)

vanity.py sends output with `logging` commands so sending output with `>` in
command line didn't seem to work.
